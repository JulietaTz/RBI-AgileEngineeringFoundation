---
id: spec-delivery-calc-engine
status: specified
created: 2026-08-20
parent: .sdlc/prds/sprint7-delivery-overhaul/plan.md
story: "Story 1"
---

# Specification: Delivery Calculation Engine

## Context

Sprint 7 replaces the ToolShop's Sprint 6 weight-and-region-based delivery pricing
(US4200) with a value-based model. This story is the **foundation** of that overhaul:
it creates the backend rule engine, exposes it as a REST endpoint, and seeds the
configurable pricing rules into the database.

Stories 2, 3, and 4 all depend on this story:
- Story 2 (`delivery-admin-config`) adds the admin update endpoint that mutates the
  config this story seeds and caches.
- Story 3 (`delivery-checkout-ui`) wires the Angular delivery step to the
  `POST /delivery/calculate` endpoint this story creates.
- Story 4 (`delivery-order-persistence`) persists the delivery choice the customer
  makes in Story 3.

This story produces no visible UI change. The new endpoint is "dark" until Story 3
calls it.

---

## Dependencies

- **Blocked by:** None — this is the foundation story.
- **Blocks:**
  - `delivery-admin-config` (needs the `delivery_config` settings row and
    `Cache::forget('delivery:config')` method this story establishes)
  - `delivery-checkout-ui` (needs `POST /delivery/calculate` to exist)
  - `delivery-order-persistence` (needs the cost calculation logic to validate against)

---

## Architecture Approach

1. A DB migration creates (or extends) a generic `settings` key-value table and inserts
   the initial delivery config JSON document under key `delivery_config`.
2. `DeliveryCalculatorService` reads the config from Redis (`Cache::remember`) with a
   1-hour TTL, falling back to the `settings` table. It applies the pricing rule
   pipeline to a cart and returns structured delivery options.
3. `DeliveryController` validates the incoming POST request, delegates to the service,
   and returns the JSON response.
4. A new route `POST /delivery/calculate` is registered in `api.php` as a public
   (unauthenticated) route — delivery cost is public information and can be queried
   without login.

Data flow:

```
Angular (Step 3) → POST /delivery/calculate
                          ↓
                  DeliveryController::calculate()
                          ↓ validates request
                  DeliveryCalculatorService::calculate()
                          ↓ reads config
                  Cache::remember('delivery:config', 3600, fn → settings table)
                          ↓ applies rule pipeline
                  returns DeliveryOptionsResult array
                          ↓
                  JSON response to Angular
```

---

## Business Rules (complete — do not reference PRD to implement)

All rules below must be encoded in `DeliveryCalculatorService`. These are not
configurable at the logic level; only the **numeric values** (thresholds, costs) come
from the JSON config.

### Oversized classification
- An item is **oversized** if its `weight_kg > 25`. (Sprint 7 drops the "large
  dimensions" clause from US2450 — weight is the sole criterion.)
- `is_oversized` is evaluated per line item, not per cart total.

### Non-oversized total
- Sum of `price_eur × quantity` for all items where `weight_kg ≤ 25`.
- Items where `weight_kg > 25` are **excluded** from this total.
- This total is the input to the free-shipping threshold check.

### Base delivery cost (applies to Standard and Zásilkovna identically)
- If non-oversized total **> 75.00**: base cost = **0.00** (free shipping)
- If non-oversized total **≤ 75.00**: base cost = **5.95** (flat rate)
- Boundary: exactly 75.00 → flat rate applies (not free).

### Oversized surcharge
- If ANY item in the cart is oversized: surcharge = **20.00**
- Surcharge is additive — it appears alongside (not instead of) the base cost.
- Surcharge applies equally to Standard and Zásilkovna options.
- Surcharge does NOT affect the free-shipping threshold check.

### Express delivery
- Available only when ALL of the following are true:
  1. Every item has `in_stock = true`
  2. No item is oversized (`weight_kg ≤ 25` for every item)
- If either condition fails → Express is absent from the response.
- Express cost = **12.95**. Express is never free (threshold does not apply to it).
- Express is **never** available for Zásilkovna (Zásilkovna is a parcel-pickup method,
  not a courier method).

### Zásilkovna eligibility
- Available only when `shipping_country` is one of: **CZ, DE, AT, CH**
- Zásilkovna uses the same base cost formula as Standard.
- If `shipping_country` is NOT in that set → Zásilkovna absent from response.

### Currency
- All returned costs are in **EUR** (single currency for Sprint 7). No CZK/USD
  conversion is performed in this story.

### Unsupported destination
- Standard delivery is always returned (covers all destinations). There is no
  unsupported-destination case at the calculation level — the "no eligible methods"
  scenario is handled client-side in Story 3.

---

## Config JSON Schema

The JSON document stored in `settings` under key `delivery_config` must conform to
this schema (all fields required):

```
{
  "free_threshold_eur":         75.00,   // float — non-oversized total above which base = 0
  "flat_rate_eur":               5.95,   // float — base cost when at or below threshold
  "oversized_surcharge_eur":    20.00,   // float — added when any item > oversized_weight_kg
  "express_rate_eur":           12.95,   // float — Express cost (not subject to threshold)
  "oversized_weight_threshold_kg": 25.0, // float — items above this weight are oversized
  "zasilkovna_countries": ["CZ","DE","AT","CH"]  // array of ISO-3166-1 alpha-2 strings
}
```

`UpdateDeliveryConfigRequest` in Story 2 will validate this same schema.
`DeliveryCalculatorService` must validate that all keys exist after deserialising —
throw a descriptive exception if any key is missing (guards against corrupted config).

---

## Request and Response Contract

### Request: `POST /delivery/calculate`

Content-Type: `application/json`

```
{
  "cart_items": [
    {
      "product_id":   integer (required),
      "weight_kg":    float   (required, >= 0),
      "price_eur":    float   (required, >= 0),
      "quantity":     integer (required, >= 1),
      "in_stock":     boolean (required)
    }
  ],
  "shipping_country": string (required, 2-char ISO code, e.g. "CZ")
}
```

Validation rules:
- `cart_items`: required, array, min 1 item, max 100 items
- `cart_items.*.product_id`: required, integer
- `cart_items.*.weight_kg`: required, numeric, min 0
- `cart_items.*.price_eur`: required, numeric, min 0
- `cart_items.*.quantity`: required, integer, min 1
- `cart_items.*.in_stock`: required, boolean
- `shipping_country`: required, string, size 2, alpha

### Response: 200 OK

```
{
  "delivery_options": [
    {
      "method":         string,  // "standard" | "zasilkova" | "express"
      "label":          string,  // human-readable label
      "base_cost_eur":  float,   // 0.00 for free shipping, else flat/express rate
      "available":      boolean  // always true (unavailable options are omitted)
    }
  ],
  "oversized_surcharge_eur": float,   // 20.00 if any oversized item, else 0.00
  "non_oversized_total_eur": float    // sum used for threshold check (for UI display)
}
```

Notes:
- Only **available** delivery options appear in the array (filtering happens server-side).
- `available` is always `true` for returned options (the field is included for forward
  compatibility — Story 3 uses it for display logic).
- The `method` value `"zasilkova"` matches the Sprint 6 spelling from US4200 (not
  "zasilkovna") — use this exact spelling for consistency with the existing codebase.

### Response: 422 Unprocessable Entity (validation failure)

Standard Laravel validation error format:
```
{ "message": "...", "errors": { "field": ["reason"] } }
```

---

## Files to Create

| File Path | Purpose | Pattern Reference |
|---|---|---|
| `sprint5/API/database/migrations/<timestamp>_create_settings_table_and_seed_delivery_config.php` | Creates `settings` (key/value) table if not present; inserts initial `delivery_config` JSON row | `sprint5/API/database/migrations/2026_04_20_181500_add_journey_id_to_invoices_table.php` |
| `sprint5/API/database/seeders/DeliveryConfigSeeder.php` | Standalone seeder for `delivery_config` (used in tests to reset state) | Existing seeders in `sprint5/API/database/seeders/` |
| `sprint5/API/app/Services/DeliveryCalculatorService.php` | Rule engine: reads cached config, applies pricing pipeline, returns options | `sprint5/API/app/Services/` (existing service classes) |
| `sprint5/API/app/Http/Controllers/DeliveryController.php` | Accepts POST, validates via Form Request, delegates to service, returns JSON | `sprint5/API/app/Http/Controllers/PaymentController.php` |
| `sprint5/API/app/Http/Requests/DeliveryCalculateRequest.php` | Form Request with validation rules for cart_items and shipping_country | `sprint5/API/app/Http/Requests/` (existing request classes) |
| `sprint5/API/tests/Feature/DeliveryCalculationTest.php` | Feature tests for all pricing combinations | `sprint5/API/tests/Feature/InvoiceTest.php` |

---

## Files to Modify

| File Path | Change Description | Target Function/Component |
|---|---|---|
| `sprint5/API/routes/api.php` | Add `Route::post('/delivery/calculate', [DeliveryController::class, 'calculate'])` in the public (unauthenticated) route group | Public routes section (same group as other unauthenticated endpoints) |

---

## Data and Contract Mappings

### Settings table schema

The `settings` table must have (at minimum):

| Column | Type | Constraint |
|---|---|---|
| `id` | BIGINT UNSIGNED | PK, auto-increment |
| `key` | VARCHAR(255) | NOT NULL, UNIQUE |
| `value` | JSON (or LONGTEXT) | NOT NULL |
| `created_at` | TIMESTAMP | nullable |
| `updated_at` | TIMESTAMP | nullable |

The migration must use `Schema::hasTable('settings')` to detect whether the table
already exists before creating it. If the table exists, use
`DB::table('settings')->updateOrInsert()` to upsert the config row.

### Config → Rule engine mapping

| Config key | Used in rule | Notes |
|---|---|---|
| `free_threshold_eur` | Base cost = 0 if non-oversized total > this | Strict greater-than, not ≥ |
| `flat_rate_eur` | Base cost when total ≤ threshold | Applies to Standard and Zásilkovna |
| `oversized_surcharge_eur` | Added to delivery total when any item > threshold | Always additive |
| `express_rate_eur` | Express cost | Not subject to threshold logic |
| `oversized_weight_threshold_kg` | Item weight > this → oversized | Strict greater-than |
| `zasilkovna_countries` | Array of eligible `shipping_country` values | Case-insensitive comparison recommended |

### Cart item → rule inputs mapping

| Request field | Rule input | Derivation |
|---|---|---|
| `weight_kg` (per item) | `is_oversized` flag | `weight_kg > oversized_weight_threshold_kg` |
| `price_eur × quantity` (non-oversized items only) | `non_oversized_total` | Sum across non-oversized items |
| `in_stock` (all items) | Express eligibility | All items must be `true` |
| `shipping_country` | Zásilkovna eligibility | Must be in `zasilkovna_countries` |

### Rule pipeline pseudocode (for DeliveryCalculatorService — do not implement directly, use as logic spec)

```
function calculate(cartItems, shippingCountry, config):
  oversizedItems    = cartItems where weight_kg > config.oversized_weight_threshold_kg
  nonOversizedItems = cartItems where weight_kg <= config.oversized_weight_threshold_kg
  nonOversizedTotal = sum(item.price_eur * item.quantity for item in nonOversizedItems)
  hasOversized      = oversizedItems is not empty
  allInStock        = all(item.in_stock for item in cartItems)
  zasilkovnaEligible = shippingCountry (case-insensitive) in config.zasilkovna_countries

  baseCost = nonOversizedTotal > config.free_threshold_eur
             ? 0.00
             : config.flat_rate_eur

  surcharge = hasOversized ? config.oversized_surcharge_eur : 0.00

  options = [
    { method: "standard", label: "Standard Delivery",
      base_cost_eur: baseCost, available: true }
  ]

  if zasilkovnaEligible:
    options += { method: "zasilkova", label: "Zásilkovna",
                 base_cost_eur: baseCost, available: true }

  if allInStock AND NOT hasOversized:
    options += { method: "express", label: "Express Next-Day",
                 base_cost_eur: config.express_rate_eur, available: true }

  return {
    delivery_options: options,
    oversized_surcharge_eur: surcharge,
    non_oversized_total_eur: nonOversizedTotal
  }
```

### Cache design

| Aspect | Specification |
|---|---|
| Cache key | `delivery:config` |
| Cache TTL | 3600 seconds (1 hour) |
| Cache driver | Default (Redis via Predis, per project config) |
| Cache population | `Cache::remember('delivery:config', 3600, fn() => DB::table('settings')->where('key','delivery_config')->value('value'))` |
| Cache invalidation method | `DeliveryCalculatorService::clearConfigCache()` — calls `Cache::forget('delivery:config')`. Story 2 will call this method after admin update. |
| Config decoding | JSON-decode in service after reading from cache/DB; validate all required keys are present |

---

## Key Patterns to Follow

| Pattern | Reference Implementation |
|---|---|
| Controller structure (validate → delegate → respond) | `sprint5/API/app/Http/Controllers/PaymentController.php` |
| Feature test structure (postJson → assertStatus → assertJson) | `sprint5/API/tests/Feature/InvoiceTest.php` |
| Migration structure (create table + seed row) | `sprint5/API/database/migrations/2026_04_20_181500_add_journey_id_to_invoices_table.php` |
| Form Request validation class | `sprint5/API/app/Http/Requests/` (existing request classes) |
| Service class constructor injection | `sprint5/API/app/Services/` (existing services in BMAD artifacts) |
| Route registration (public group) | Unauthenticated routes section in `sprint5/API/routes/api.php` |
| Redis caching | `Cache::remember($key, $ttl, $callback)` pattern (Laravel Cache facade) |

---

## Existing Code to Reuse

| Existing Asset | Location | How to Reuse | Why Not Create New |
|---|---|---|---|
| `PaymentController` | `sprint5/API/app/Http/Controllers/PaymentController.php` | Use as structural template for `DeliveryController` (method signature, validation delegation, JSON response pattern) | Already establishes the project's controller convention; `DeliveryController` should mirror its structure |
| `InvoiceController` | `sprint5/API/app/Http/Controllers/InvoiceController.php` | Reference for how controllers interact with models and return structured responses | Same reason — project-established pattern |
| Laravel `Cache` facade | Already registered via Predis | Use `Cache::remember()` and `Cache::forget()` directly — no wrapper needed | Redis already integrated; wrapping adds no value |
| Laravel `DB` facade | Already registered | Use `DB::table('settings')` for settings table operations | Native query builder sufficient for this simple CRUD |
| Laravel Form Request | `sprint5/API/app/Http/Requests/` | Extend `FormRequest` for `DeliveryCalculateRequest` with `rules()` method | Already the project's validation convention per architecture |

---

## Verification Steps

### Unit Tests

Test `DeliveryCalculatorService` in isolation by constructing the service with a
hard-coded config array (bypass DB/Redis). All assertions on the returned
`delivery_options` array and scalar fields.

| Test Name (from plan) | What it verifies | BAC |
|---|---|---|
| `T1_deliveryCalc_nonOversizedCartAbove75EUR_standardDeliveryIsFree` | Service returns `base_cost_eur: 0.00` for standard when non-oversized total = 76.00 | BAC-1 |
| `T2_deliveryCalc_nonOversizedCartAt75EUR_standardDeliveryIs5_95EUR` | Service returns `base_cost_eur: 5.95` when non-oversized total = 75.00 | BAC-2 |
| `T3_deliveryCalc_nonOversizedCartExactlyAt75EUR_flatRateApplied` | Boundary: total = 75.00 → flat rate, not free (strict greater-than) | BAC-2 |
| `T1_deliveryCalc_cartContainsItemOver25kg_oversizedSurcharge20EURApplied` | `oversized_surcharge_eur: 20.00` returned when cart has one item at 25.1 kg | BAC-3 |
| `T2_deliveryCalc_onlyOversizedItemInCart_baseDeliveryIs5_95AndSurchargeIs20EUR` | Single 30 kg item at €80: non-oversized total = 0 → flat rate + surcharge | BAC-4 |
| `T3_deliveryCalc_mixedCartOver75EURWithOversizedItem_baseIsFreeAndSurchargeIs20EUR` | €80 non-oversized items + one 30 kg item → base = 0.00, surcharge = 20.00 | BAC-5 |
| `T1_deliveryCalc_allItemsInStockAndNoneOversized_expressOptionAvailableAt12_95EUR` | All in_stock=true, all weight ≤25 kg → express present at €12.95 | BAC-6 |
| `T2_deliveryCalc_cartContainsOutOfStockItem_expressOptionNotReturned` | One in_stock=false → express absent from options array | BAC-7 |
| `T3_deliveryCalc_cartContainsOversizedItemAllInStock_expressOptionNotReturned` | All in_stock=true but one item = 26 kg → express absent | BAC-8 |
| `T4_deliveryCalc_cartContainsOversizedAndOutOfStock_expressBlockedByBothGates` | Oversized AND out-of-stock → express absent; confirms both gates applied independently | BAC-8 |
| `T1_deliveryCalc_shippingCountryCZ_zasilkovnaOptionReturned` | shipping_country = "CZ" → zasilkova present | BAC-9 |
| `T2_deliveryCalc_shippingCountryDE_zasilkovnaOptionReturned` | shipping_country = "DE" → zasilkova present | BAC-9 |
| `T3_deliveryCalc_shippingCountryOutsideCZandDACH_zasilkovnaNotReturned` | shipping_country = "US" → zasilkova absent | BAC-10 |
| `T1_deliveryCalc_zasilkovnaSelectedCartAbove75EUR_deliveryCostIsFree` | zasilkova option has base_cost_eur = 0.00 when non-oversized total > 75 | BAC-11 |
| `T2_deliveryCalc_zasilkovnaSelectedCartAt75EUR_deliveryCostIs5_95EUR` | zasilkova option has base_cost_eur = 5.95 when non-oversized total = 75.00 | BAC-11 |

### Integration Tests

Test `DeliveryController` via `$this->postJson('/delivery/calculate', $payload)` against a
running application with database. Use `DeliveryConfigSeeder` to populate the `settings`
table before each test class.

| Test Name (from plan) | What it verifies | BAC |
|---|---|---|
| `T1_deliveryCalc_configUpdatedInSettings_recalculationUsesNewRules` | Directly update `delivery_config` in settings table, call `clearConfigCache()`, then POST `/delivery/calculate` → response uses updated threshold | BAC-14 |
| All unit test scenarios above | Same rules verified end-to-end through HTTP layer | All |

Additional integration-only assertions:
- POST with missing `cart_items` → 422 with `errors.cart_items`
- POST with empty `cart_items` array → 422
- POST with missing `shipping_country` → 422 with `errors.shipping_country`
- POST with `shipping_country = "XY"` (non-alpha-2) → 422
- POST with `quantity = 0` → 422

### E2E / Manual Verification

1. Start the Laravel API: `php artisan serve`
2. POST to `http://localhost:8000/delivery/calculate` with the following payload and
   verify response structure:
   ```
   { "cart_items": [{"product_id":1,"weight_kg":1.5,"price_eur":30,"quantity":2,"in_stock":true}],
     "shipping_country": "CZ" }
   ```
   Expected: standard + zasilkova at €5.95 (non-oversized total = €60 < €75),
   no express (wait — 2 items × 1.5 kg = 3 kg, both in stock, not oversized → express SHOULD appear).
   Correct expected: standard at €5.95, zasilkova at €5.95, express at €12.95, surcharge = 0.
3. Repeat with `price_eur: 40` (total = €80 > €75):
   Expected: standard at €0.00, zasilkova at €0.00, express at €12.95, surcharge = 0.
4. Repeat with one item at `weight_kg: 30`:
   Expected: standard at base cost, NO express, surcharge = €20.00, zasilkova dependent on country.
5. Update the `delivery_config` row in the DB to set `free_threshold_eur: 100`,
   clear Redis, POST again with total €80 → verify flat rate is now returned.

---

## Blast Radius

- **Files directly modified:**
  - `sprint5/API/routes/api.php` — one route addition in public group

- **Files created (net new, no existing code modified):**
  - `DeliveryController.php`, `DeliveryCalculatorService.php`,
    `DeliveryCalculateRequest.php`, migration, seeder, feature test class

- **Dependent systems affected:**
  - Story 2 depends on `clearConfigCache()` method and `delivery_config` settings key
  - Story 3 depends on `POST /delivery/calculate` endpoint and its response schema
  - Story 4 depends on the logic being correct for validation during order submission

- **Regression risk:**
  - Adding a route to `api.php` is low-risk (additive only)
  - The settings table creation migration uses `Schema::hasTable()` guard — safe if
    the table already exists in a different form
  - No existing endpoint is modified; no existing service is changed

---

## Threat Model Considerations

- **New attack surface:** `POST /delivery/calculate` is a public endpoint (no auth).
  Malicious actors could spam it with large payloads.
  - Mitigation: validate `cart_items` array max size to 100 items; existing gateway
    rate limiting applies.
- **Data sensitivity:** Request contains product prices and weights — not PII. No
  credentials or financial transactions.
- **Input validation:** All fields validated via `DeliveryCalculateRequest`. Reject
  non-numeric weights/prices, non-boolean `in_stock`, and invalid country codes.
- **Config integrity:** `DeliveryCalculatorService` must validate all required keys
  exist in the deserialized JSON config — prevents silent logic errors from a
  corrupted or incomplete config document.

---

## Test Data Requirements

- **Fixtures / factories needed:**
  - `DeliveryConfigSeeder` (created in this story) — inserts canonical Sprint 7 config
    into `settings` table; used in all feature tests via `$this->seed(DeliveryConfigSeeder::class)`
  - Cart item arrays hard-coded in test methods (no DB product lookup required — the
    endpoint is stateless; all data comes from the request body)

- **Seed data:**
  - `settings` row: key = `delivery_config`, value = the canonical Sprint 7 config JSON

- **Edge case test data:**
  ```
  Exactly €75.00 non-oversized total → flat rate (boundary)
  Exactly 25.0 kg → NOT oversized (boundary; threshold is strictly > 25)
  Exactly 25.1 kg → oversized
  shipping_country = "AT" → Zásilkovna eligible (DACH)
  shipping_country = "CH" → Zásilkovna eligible (DACH)
  shipping_country = "PL" → Zásilkovna NOT eligible
  Mixed in_stock: one false → express blocked
  Cart with 100 items → max payload (boundary validation)
  ```

---

## Out of Scope

- **Admin config update endpoint** (`PUT /admin/delivery-config`) — specified in
  Story 2 (`delivery-admin-config`)
- **Angular delivery step UI** — specified in Story 3 (`delivery-checkout-ui`)
- **Order persistence** (saving delivery method + cost to the order) — Story 4
  (`delivery-order-persistence`)
- **Multi-currency support** (CZK/USD) — Sprint 7 uses EUR only; conversion is
  explicitly out of scope
- **Regional pricing differences** (e.g., different flat rates per country) — Sprint 7
  uses a single flat rate regardless of destination
- **Zásilkovna pickup-point selection UI** — not in scope for any Sprint 7 story
- **Product weight data backfill** — the `weight` field is assumed to exist on the
  products table (from Sprint 6 US4200). If any product has a null weight, the service
  should treat it as 0 kg (not oversized, but contribute nothing to threshold total)
