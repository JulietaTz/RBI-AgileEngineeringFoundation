---
id: spec-delivery-admin-config
status: specified
created: 2026-08-20
parent: .sdlc/prds/sprint7-delivery-overhaul/plan.md
story: "Story 2"
---

# Specification: Admin Delivery Config API

## Context

Story 1 (`delivery-calc-engine`) seeded the initial delivery pricing rules as a JSON
document in the `settings` table and introduced a `DeliveryCalculatorService` that
Redis-caches that document. However, updating those rules currently requires a direct
DB change — violating the PRD's "configurable without code deployment" requirement
(BAC-14).

This story closes that gap: a protected admin endpoint allows any authenticated admin
user to replace the delivery config document at runtime. The endpoint validates the
incoming payload, preserves the previous config in a history slot, updates the settings
table, and invalidates the Redis cache — so the next customer request to
`POST /delivery/calculate` picks up the new rules immediately.

No customer-facing UI changes are made by this story. Story 3 (`delivery-checkout-ui`)
and Story 4 (`delivery-order-persistence`) are unaffected by this story and can be
developed in parallel with it.

---

## Dependencies

- **Blocked by:** `delivery-calc-engine` (Story 1) — must complete first because:
  - The `settings` table and `delivery_config` key are created in Story 1's migration.
  - `DeliveryCalculatorService::clearConfigCache()` (called by this story) is defined
    in Story 1.
- **Blocks:** nothing — Story 2 is a leaf node in the dependency graph. Stories 3 and
  4 do not wait for it.

---

## Architecture Approach

A single `PUT /admin/delivery-config` endpoint is added under the existing admin
middleware group in `api.php`. When called:

1. `UpdateDeliveryConfigRequest` validates all required numeric and array fields.
2. `Admin\DeliveryConfigController::update()` delegates to business logic (inline in
   controller is acceptable given the S complexity — no separate service class needed).
3. Controller reads the current `delivery_config` from the settings table, prepends it
   to the history array in `delivery_config_history`, trims history to 5 entries, writes
   both keys back, then calls `DeliveryCalculatorService::clearConfigCache()`.
4. Returns 200 with the stored config document.

```
Admin client → PUT /admin/delivery-config (Bearer token)
                     ↓ auth:sanctum + role:admin
               UpdateDeliveryConfigRequest (validation)
                     ↓
               DeliveryConfigController::update()
                 1. read current delivery_config from settings
                 2. prepend to delivery_config_history (max 5)
                 3. write new delivery_config to settings
                 4. write trimmed history to delivery_config_history
                 5. DeliveryCalculatorService::clearConfigCache()
                     ↓
               return 200 { new config document }
```

---

## Business Rules (complete — do not reference PRD to implement)

### Config document schema
The request body is the full delivery config document. All six fields are **required**:

| Field | Type | Constraint |
|---|---|---|
| `free_threshold_eur` | float | required, numeric, min:0 |
| `flat_rate_eur` | float | required, numeric, min:0 |
| `oversized_surcharge_eur` | float | required, numeric, min:0 |
| `express_rate_eur` | float | required, numeric, min:0 |
| `oversized_weight_threshold_kg` | float | required, numeric, min:0 |
| `zasilkovna_countries` | array of strings | required, array, min:1; each element: string, size:2, alpha |

### Config history
- Before writing the new config, read the current `delivery_config` value from the
  settings table.
- Retrieve (or initialise as `[]`) the `delivery_config_history` array from the
  settings table. Each history entry is the full previous config document.
- Prepend the current (outgoing) config to the history array.
- Trim the history array to a **maximum of 5 entries** (oldest entries discarded).
- Write the trimmed history back to `delivery_config_history` in settings.
- Then write the new config to `delivery_config`.

Both settings writes should happen inside a **DB transaction** to prevent a partial
update if either write fails.

### Cache invalidation
- After the DB transaction commits, call `DeliveryCalculatorService::clearConfigCache()`.
- This method (defined in Story 1) calls `Cache::forget('delivery:config')`.
- Do NOT inline the `Cache::forget` call in the controller — always delegate to
  `DeliveryCalculatorService::clearConfigCache()` to keep the cache key canonical.

### Admin authentication
- Use the same middleware group as other admin routes in `api.php`.
- If the existing admin group uses `auth:sanctum` + a role gate, register this route
  in that group.
- If no admin group exists yet, register with `middleware(['auth:sanctum', 'role:admin'])`.
  The `role:admin` middleware must verify that `auth()->user()->role === 'admin'` (or
  equivalent role attribute on the `users` table).
- Unauthenticated requests → 401. Authenticated non-admin → 403.

---

## Request and Response Contract

### Request: `PUT /admin/delivery-config`

Headers: `Authorization: Bearer <admin_token>`, `Content-Type: application/json`

Body (flat JSON — same structure as the stored `delivery_config` document):
```
{
  "free_threshold_eur":            float  (required, ≥ 0),
  "flat_rate_eur":                 float  (required, ≥ 0),
  "oversized_surcharge_eur":       float  (required, ≥ 0),
  "express_rate_eur":              float  (required, ≥ 0),
  "oversized_weight_threshold_kg": float  (required, ≥ 0),
  "zasilkovna_countries":          array  (required, ≥ 1 string items, each 2 chars)
}
```

### Response: 200 OK

Body: the full config document exactly as stored (JSON re-encoded from DB):
```
{
  "free_threshold_eur":            float,
  "flat_rate_eur":                 float,
  "oversized_surcharge_eur":       float,
  "express_rate_eur":              float,
  "oversized_weight_threshold_kg": float,
  "zasilkovna_countries":          array
}
```

### Response: 422 Unprocessable Entity

Standard Laravel validation error format:
```
{ "message": "...", "errors": { "<field>": ["<reason>"] } }
```

### Response: 401 Unauthorized

Returned when no `Authorization` header is provided or token is invalid.

### Response: 403 Forbidden

Returned when the authenticated user is not an admin.

---

## Files to Create

| File Path | Purpose | Pattern Reference |
|---|---|---|
| `sprint5/API/app/Http/Controllers/Admin/DeliveryConfigController.php` | Handles `PUT /admin/delivery-config`: reads history, writes new config, invalidates cache | `sprint5/API/app/Http/Controllers/PaymentController.php` |
| `sprint5/API/app/Http/Requests/UpdateDeliveryConfigRequest.php` | Form Request: validates all 6 required config fields | `sprint5/API/app/Http/Requests/Recommendation/StoreRecommendationSession.php` (if exists) or any existing Request in `app/Http/Requests/` |
| `sprint5/API/tests/Feature/Admin/DeliveryConfigTest.php` | Integration tests: update + validation + auth + history | `sprint5/API/tests/Feature/InvoiceTest.php` |

---

## Files to Modify

| File Path | Change Description | Target Function/Component |
|---|---|---|
| `sprint5/API/routes/api.php` | Add `Route::put('/admin/delivery-config', [Admin\DeliveryConfigController::class, 'update'])` inside the existing admin middleware group (or new group if none exists) | Admin middleware group |

---

## Data and Contract Mappings

### Settings table operations (within DB transaction)

| Step | Operation | Key | Value |
|---|---|---|---|
| 1 — Read current config | `DB::table('settings')->where('key','delivery_config')->value('value')` | `delivery_config` | JSON string → decode to PHP array |
| 2 — Read history | `DB::table('settings')->where('key','delivery_config_history')->value('value')` | `delivery_config_history` | JSON string → decode to PHP array (or `[]` if null) |
| 3 — Prepend + trim | `array_slice(array_merge([$currentConfig], $history), 0, 5)` | — | max 5 entries |
| 4 — Write history | `DB::table('settings')->updateOrInsert(...)` | `delivery_config_history` | JSON-encoded trimmed array |
| 5 — Write new config | `DB::table('settings')->updateOrInsert(...)` | `delivery_config` | JSON-encoded new config |

All 5 steps must be wrapped in `DB::transaction(fn() => ...)`.

### Incoming request → stored document

| Request field | Stored key | Type | Notes |
|---|---|---|---|
| `free_threshold_eur` | `free_threshold_eur` | float | stored as-is |
| `flat_rate_eur` | `flat_rate_eur` | float | stored as-is |
| `oversized_surcharge_eur` | `oversized_surcharge_eur` | float | stored as-is |
| `express_rate_eur` | `express_rate_eur` | float | stored as-is |
| `oversized_weight_threshold_kg` | `oversized_weight_threshold_kg` | float | stored as-is |
| `zasilkovna_countries` | `zasilkovna_countries` | string[] | stored as JSON array |

No transformation needed — the validated request payload is stored verbatim.

---

## Key Patterns to Follow

| Pattern | Reference Implementation |
|---|---|
| Controller structure (thin: validate via FormRequest → delegate → respond) | `sprint5/API/app/Http/Controllers/PaymentController.php` |
| Form Request with typed validation rules | Any existing class in `sprint5/API/app/Http/Requests/` |
| DB transaction wrapping multiple writes | `DB::transaction(fn() => ...)` (Laravel built-in) |
| Admin middleware group in routes | Existing admin route group in `sprint5/API/routes/api.php` |
| Feature test with admin auth | `$this->actingAs($adminUser, 'sanctum')->putJson('/admin/delivery-config', ...)` |
| Custom middleware structure | `sprint5/API/app/Http/Middleware/EnsureTelemetryConsent.php` |

---

## Existing Code to Reuse

| Existing Asset | Location | How to Reuse | Why Not Create New |
|---|---|---|---|
| `DeliveryCalculatorService::clearConfigCache()` | `sprint5/API/app/Services/DeliveryCalculatorService.php` (created in Story 1) | Call directly from `DeliveryConfigController::update()` after DB transaction | Canonical cache key lives in Story 1; delegating avoids duplicating the key string |
| `DB::transaction()` (Laravel) | Built-in | Wrap the two settings writes | Prevents partial updates; already available without extra setup |
| `Cache::forget()` | Built-in (via `DeliveryCalculatorService`) | Invoked indirectly through `clearConfigCache()` | See above |
| `DB::table('settings')` query builder | Built-in (used in Story 1 service) | Same `updateOrInsert` pattern for writing history and new config | Story 1 established `DB::table('settings')` as the access pattern; consistency required |
| Existing admin middleware group | `sprint5/API/routes/api.php` | Register new route inside existing group | Avoids duplicating auth logic; ensures consistent protection |

---

## Verification Steps

### Integration Tests

All tests live in `sprint5/API/tests/Feature/Admin/DeliveryConfigTest.php`.
Each test must seed the settings table with a valid `delivery_config` row using
`DeliveryConfigSeeder` (created in Story 1) before asserting.

| Test Name (from plan) | What it verifies | BAC |
|---|---|---|
| `T1_deliveryAdminConfig_validJsonConfigProvided_configUpdatedAndCacheInvalidated` | PUT with valid payload → 200; `delivery_config` in settings matches new values; Redis `delivery:config` key absent after update (verified by subsequent `POST /delivery/calculate` returning new rules) | BAC-14 |
| `T2_deliveryAdminConfig_invalidJsonSchemaMissingKey_requestRejectedWith422` | PUT with one required field omitted (e.g., `express_rate_eur` missing) → 422 with `errors.express_rate_eur` | BAC-14 |
| `T3_deliveryAdminConfig_successfulUpdate_previousConfigPreservedInHistory` | After valid PUT, `delivery_config_history` in settings contains the old config as first entry | BAC-14 |
| `T4_deliveryAdminConfig_unauthenticatedRequest_returns401` | PUT with no `Authorization` header → 401 | BAC-14 |

**Additional integration assertions (not named in plan — add to same test class):**
- Authenticated non-admin user → 403
- `zasilkovna_countries` with a non-alpha-2 string (e.g., `"CZK"`) → 422
- `zasilkovna_countries` as empty array → 422
- `free_threshold_eur` as a negative number → 422
- After 6 consecutive valid updates, `delivery_config_history` contains exactly 5 entries
  (confirms FIFO trimming)

### E2E / Manual Verification

1. Seed the initial config via `php artisan db:seed --class=DeliveryConfigSeeder`.
2. Obtain an admin Bearer token (e.g., `POST /tokens/create` with admin credentials).
3. Call `PUT /admin/delivery-config` with a modified `free_threshold_eur` (e.g., 100.00).
4. Verify 200 response body shows `free_threshold_eur: 100.00`.
5. Check settings table: `SELECT value FROM settings WHERE key = 'delivery_config'` →
   confirms `free_threshold_eur` = 100.
6. Immediately call `POST /delivery/calculate` with a cart totalling €80 (non-oversized).
   Expect flat rate (€5.95) — confirming cache was invalidated and new threshold applied.
7. Call `SELECT value FROM settings WHERE key = 'delivery_config_history'` → first entry
   should show the old config with `free_threshold_eur: 75`.

---

## Blast Radius

- **Files directly modified:**
  - `sprint5/API/routes/api.php` — one route addition in the admin group

- **Files created (net new):**
  - `DeliveryConfigController.php`, `UpdateDeliveryConfigRequest.php`,
    `DeliveryConfigTest.php`

- **Dependent systems affected:**
  - `DeliveryCalculatorService` (Story 1) is called by this story — no code change to
    it, only its public `clearConfigCache()` method is invoked
  - `POST /delivery/calculate` behaviour changes immediately after a successful PUT (the
    new rules take effect on the next request — this is intentional)

- **Regression risk:**
  - `api.php` modification is additive (new route); no existing route changes
  - A corrupted `delivery_config_history` value in settings would not crash
    `DeliveryCalculatorService` — it only reads `delivery_config`, not the history
  - A failed DB transaction (e.g., MySQL timeout) leaves both settings keys unchanged;
    the Redis cache remains valid — safe failure mode

---

## Threat Model Considerations

- **New attack surface:** `PUT /admin/delivery-config` is authenticated admin-only.
  Correct middleware is the primary guard.
  - Risk: if the admin middleware group has misconfigured policy, this endpoint could
    be reached by non-admin users. Verify with `T4` (401) and the 403 test.
- **Data sensitivity:** Delivery pricing config is not PII. However, a malicious admin
  could set `free_threshold_eur: 0` (everything free) or `flat_rate_eur: 0`. This is
  an insider-threat risk, not an external exploit risk. Config history (5 snapshots)
  provides an audit trail and quick rollback visibility.
- **Input validation:** All numeric fields validated as `numeric|min:0`. String array
  items validated as `string|size:2|alpha`. These rules prevent injection via pricing
  values or country codes.
- **JSON injection:** The config is stored as a JSON string and re-serialized from
  validated PHP types — no raw user string is stored without type coercion. SQL
  injection via `updateOrInsert` uses parameterized bindings (Laravel query builder).

---

## Test Data Requirements

- **Seed data:** `DeliveryConfigSeeder` (from Story 1) — run before each test to
  populate a known `delivery_config` baseline and an empty `delivery_config_history`.
- **Admin user fixture:** A `User` factory variant with `role = 'admin'` (or equivalent)
  created inline in the test with `User::factory()->create(['role' => 'admin'])`.
- **Edge case data:**
  ```
  Valid: zasilkovna_countries = ["CZ"] (single-country minimum)
  Valid: all rates = 0.00 (zero is allowed; no business minimum enforced here)
  Invalid: free_threshold_eur = -1 (negative → 422)
  Invalid: zasilkovna_countries = [] (empty → 422)
  Invalid: zasilkovna_countries = ["CZK"] (3-char code → 422)
  Invalid: missing key free_threshold_eur entirely → 422 with field error
  History trim: after 6 updates, history length = 5 (oldest dropped)
  ```

---

## Out of Scope

- **History retrieval endpoint** — no `GET /admin/delivery-config/history` is defined
  in this story. History is stored for manual DB inspection only.
- **Admin UI for delivery config** — this story is API-only; no Angular admin panel.
- **Audit log to DB** — the config history in the settings table IS the lightweight
  audit mechanism for Sprint 7. A formal `audit_logs` table (FR49) is out of scope.
- **Partial config updates** (`PATCH`) — only full replacement (`PUT`) is supported;
  partial updates are not defined in the PRD.
- **Cache warm-up after update** — the controller only invalidates (`forget`); the
  next `POST /delivery/calculate` call warms the cache lazily via
  `Cache::remember` in `DeliveryCalculatorService`.
- **Notification/webhook on config change** — not in scope for any Sprint 7 story.
