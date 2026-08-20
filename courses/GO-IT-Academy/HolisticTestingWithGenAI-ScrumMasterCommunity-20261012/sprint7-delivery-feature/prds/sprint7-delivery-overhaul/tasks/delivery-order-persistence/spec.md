---
id: spec-delivery-order-persistence
status: specified
created: 2026-08-20
parent: .sdlc/prds/sprint7-delivery-overhaul/plan.md
story: "Story 4"
---

# Specification: Order Persistence and Itemised Confirmation

## Context

Story 3 (`delivery-checkout-ui`) writes the customer's delivery selection —
`{method, base_cost_eur, oversized_surcharge_eur}` — to the Angular checkout state.
Currently that selection is lost when the order is submitted: the backend does not
receive, validate, or store it. The order confirmation page has no delivery line items.

This story closes both gaps: the backend stores the three delivery fields on the order
record, and the Angular order-overview (confirmation) page renders them as separate
itemised lines.

This is the last story in the four-story delivery overhaul. When this story is merged and
deployed alongside Stories 1, 3, and 4, the complete end-to-end delivery flow is live:
calculation → display → selection → persistence → confirmation.

---

## Dependencies

- **Blocked by:**
  - `delivery-calc-engine` (Story 1) — provides the delivery cost logic that determines
    what costs will be submitted; also needed for integration test validation
  - `delivery-checkout-ui` (Story 3) — provides the checkout state `delivery` object
    that `payment.component.ts` reads and forwards to `POST /payment/check`
- **Blocks:** nothing — Story 4 is a leaf node; the feature is complete after this story

---

## Architecture Approach

Three coordinated changes across backend and frontend:

**Backend:**
1. A new migration adds three nullable columns to the `orders` table
   (the table used by the `Invoice` model).
2. `Invoice.php` adds the three fields to `$fillable`.
3. `PaymentController::check()` validates the three delivery fields in the incoming
   request and writes them to the Invoice on creation.
4. `InvoiceController` is verified (and updated if needed) to include delivery fields
   in the `GET /invoices/{id}` response.

**Frontend:**
5. `payment.component.ts` (Step 4) reads `checkoutState.delivery` and appends the
   three delivery fields to the `POST /payment/check` payload.
6. `order-overview.component.ts` maps delivery fields from the invoice API response
   into component properties.
7. `order-overview.component.html` renders the delivery block (method label + cost,
   conditional surcharge line, updated order total).

```
checkout state.delivery (from Story 3)
    ↓ payment.component.ts reads + appends to payload
POST /payment/check { ..., delivery_method, delivery_base_cost_eur,
                      delivery_oversized_surcharge_eur }
    ↓ PaymentController validates + creates Invoice
orders table { delivery_method, delivery_base_cost_eur, delivery_oversized_surcharge_eur }
    ↓ InvoiceController returns fields
GET /invoices/{id} response → order-overview.component
    ↓ renders
Confirmation page: Delivery (Standard) €5.95 | Oversized surcharge €20.00 | Total ...
```

---

## Plan Discrepancy

The plan lists `sprint5/API/app/Models/Order.php` — the actual model file is
`sprint5/API/app/Models/Invoice.php`. The underlying DB table is `orders`; the
model class is `Invoice`. Use `Invoice.php` throughout; there is no separate `Order.php`.

The plan does not list `sprint5/UI/src/app/checkout/payment/payment.component.ts` as a
file to modify, but this file must forward the delivery selection from checkout state
into the `POST /payment/check` payload. Without this change, the backend never receives
the delivery fields and tests T3/T4 (validation) cannot fail correctly. The file is
included in Files to Modify below.

---

## Business Rules (complete — implement without reading the PRD)

### Backend validation rules for `POST /payment/check`

All three delivery fields are **required** for any new order submission:

| Field | Validation |
|---|---|
| `delivery_method` | required, string, in: `standard`, `zasilkova`, `express` |
| `delivery_base_cost_eur` | required, numeric, min: 0 |
| `delivery_oversized_surcharge_eur` | required, numeric, min: 0 |

Return 422 if any field is missing or fails validation, with Laravel's standard error
envelope `{ "message": "...", "errors": { "<field>": ["<reason>"] } }`.

### DB column definitions

| Column | Type | Nullable | Default |
|---|---|---|---|
| `delivery_method` | VARCHAR(20) | YES | NULL |
| `delivery_base_cost_eur` | DECIMAL(8,2) | YES | 0.00 |
| `delivery_oversized_surcharge_eur` | DECIMAL(8,2) | YES | 0.00 |

All columns nullable so existing orders (without delivery data) are unaffected.
New orders receive values via controller validation — null is only possible for
legacy pre-migration records.

### Confirmation page display rules

- Show "Delivery" line with the method label and `delivery_base_cost_eur`:
  - `"standard"` → label "Standard Delivery"
  - `"zasilkova"` → label "Zásilkovna"
  - `"express"` → label "Express Next-Day"
  - If `delivery_base_cost_eur === 0` → show "Free" (not "€0.00")
- Show "Oversized item surcharge" line with `delivery_oversized_surcharge_eur`
  **only if `delivery_oversized_surcharge_eur > 0`**. Hide the line entirely when 0.
- Order total = product subtotal + `delivery_base_cost_eur` + `delivery_oversized_surcharge_eur`.
- All amounts formatted as `€X.XX` in EUR.
- For legacy orders (null delivery fields): hide the entire delivery block gracefully
  (null-guard in template).

### Angular payment component (Step 4) changes

`payment.component.ts` must read from checkout state:
```
checkoutState.delivery.method               → delivery_method
checkoutState.delivery.base_cost_eur        → delivery_base_cost_eur
checkoutState.delivery.oversized_surcharge_eur → delivery_oversized_surcharge_eur
```
and include these three fields in the `POST /payment/check` request body alongside
existing payment fields. If `checkoutState.delivery` is undefined or null, the
component should navigate the user back to the delivery step rather than submit.

---

## Files to Create

| File Path | Purpose | Pattern Reference |
|---|---|---|
| `sprint5/API/database/migrations/<timestamp>_add_delivery_fields_to_orders.php` | Adds `delivery_method`, `delivery_base_cost_eur`, `delivery_oversized_surcharge_eur` as nullable columns to `orders` table | `sprint5/API/database/migrations/2026_04_20_181500_add_journey_id_to_invoices_table.php` |
| `sprint5/API/tests/Feature/OrderDeliveryPersistenceTest.php` | Integration tests for `POST /payment/check` delivery validation and persistence | `sprint5/API/tests/Feature/InvoiceTest.php` |

---

## Files to Modify

| File Path | Change Description | Target Function/Component |
|---|---|---|
| `sprint5/API/app/Models/Invoice.php` | Add `delivery_method`, `delivery_base_cost_eur`, `delivery_oversized_surcharge_eur` to `$fillable` array | `$fillable` property |
| `sprint5/API/app/Http/Controllers/PaymentController.php` | Add delivery field validation to `check()` request; persist delivery fields when creating Invoice | `check()` method |
| `sprint5/API/app/Http/Controllers/InvoiceController.php` | Verify `GET /invoices/{id}` response includes delivery fields; update API Resource or response array if fields are explicitly filtered | Invoice retrieval method (likely `show()`) |
| `sprint5/UI/src/app/checkout/payment/payment.component.ts` | Read `checkoutState.delivery` and append delivery fields to `POST /payment/check` payload; guard against missing delivery (redirect to Step 3) | `submitPayment()` or equivalent method |
| `sprint5/UI/src/app/checkout/order-overview/order-overview.component.ts` | Map `delivery_method`, `delivery_base_cost_eur`, `delivery_oversized_surcharge_eur` from invoice API response into component properties | `ngOnInit()` or invoice-loading method |
| `sprint5/UI/src/app/checkout/order-overview/order-overview.component.html` | Add delivery block: method + cost line, conditional surcharge line, updated total | Order summary section |

---

## Data and Contract Mappings

### `POST /payment/check` — extended request fields

The following fields are **added** to the existing request body (existing payment fields
remain unchanged):

| New Request Field | Type | Required | Source (Angular) |
|---|---|---|---|
| `delivery_method` | string | yes | `checkoutState.delivery.method` |
| `delivery_base_cost_eur` | numeric (float) | yes | `checkoutState.delivery.base_cost_eur` |
| `delivery_oversized_surcharge_eur` | numeric (float) | yes | `checkoutState.delivery.oversized_surcharge_eur` |

### `Invoice` model → `orders` table mapping

| PHP model field | DB column | Type | Notes |
|---|---|---|---|
| `delivery_method` | `delivery_method` | VARCHAR(20) | `'standard'`, `'zasilkova'`, or `'express'` |
| `delivery_base_cost_eur` | `delivery_base_cost_eur` | DECIMAL(8,2) | 0.00 when free shipping applies |
| `delivery_oversized_surcharge_eur` | `delivery_oversized_surcharge_eur` | DECIMAL(8,2) | 0.00 when no oversized items |

### `GET /invoices/{id}` — response additions

The invoice response (already returns order data) must include:

| Response Field | Type | Notes |
|---|---|---|
| `delivery_method` | string or null | null for legacy orders |
| `delivery_base_cost_eur` | float or null | null for legacy orders |
| `delivery_oversized_surcharge_eur` | float or null | null for legacy orders |

If `InvoiceController` returns all model attributes (default Eloquent serialization),
no controller change is needed — the new `$fillable` fields are automatically included.
If the controller uses an explicit field list or API Resource, add the three fields there.

### Confirmation page — method label mapping

| `delivery_method` value | Display label |
|---|---|
| `"standard"` | "Standard Delivery" |
| `"zasilkova"` | "Zásilkovna" |
| `"express"` | "Express Next-Day" |
| null (legacy) | — (hide delivery block) |

### `data-test` attributes for order-overview template additions

| Element | `data-test` value |
|---|---|
| Delivery method + cost line | `"order-delivery-method"` |
| Delivery cost amount | `"order-delivery-cost"` |
| Oversized surcharge line | `"order-oversized-surcharge"` |
| Order total | `"order-total"` |

---

## Key Patterns to Follow

| Pattern | Reference Implementation |
|---|---|
| Nullable column migration (`->nullable()->default(0)`) | `sprint5/API/database/migrations/2026_04_20_181500_add_journey_id_to_invoices_table.php` |
| PaymentController validation + Invoice creation | `sprint5/API/app/Http/Controllers/PaymentController.php` (`check()` method) |
| `Invoice.php` `$fillable` extension (Story 4.1 added `journey_id`) | `sprint5/API/app/Models/Invoice.php` |
| Feature test POST + assert DB persistence | `sprint5/API/tests/Feature/InvoiceTest.php` |
| Angular checkout component reading checkout state | `sprint5/UI/src/app/checkout/payment/payment.component.ts` |
| Order-overview template line-item pattern | `sprint5/UI/src/app/checkout/order-overview/order-overview.component.html` |
| Conditional display (`*ngIf`) in Angular templates | `sprint5/UI/src/app/checkout/payment/payment.component.html` |

---

## Existing Code to Reuse

| Existing Asset | Location | How to Reuse | Why Not Create New |
|---|---|---|---|
| `Invoice` model + `$fillable` | `sprint5/API/app/Models/Invoice.php` | Extend `$fillable` array; no new model | Model already maps to `orders` table; adding three fields is an additive change |
| `PaymentController::check()` | `sprint5/API/app/Http/Controllers/PaymentController.php` | Extend validation rules and Invoice creation call | Existing checkout submission handler; extending preserves existing payment flow |
| `InvoiceController` (GET response) | `sprint5/API/app/Http/Controllers/InvoiceController.php` | Verify fields are returned; extend API Resource if needed | Order retrieval already implemented; only response schema extension needed |
| Checkout state service (delivery object) | `sprint5/UI/src/app/_services/` (existing, written by Story 3) | Read in `payment.component.ts` to populate POST body | State management established by Story 3; payment component must not duplicate |
| `order-overview.component.ts` | `sprint5/UI/src/app/checkout/order-overview/order-overview.component.ts` | Add delivery field properties mapped from invoice response | Confirmation display already implemented; delivery lines are an additive extension |

---

## Verification Steps

### Integration Tests

All tests in `sprint5/API/tests/Feature/OrderDeliveryPersistenceTest.php`.
Each test must authenticate as a valid customer, populate the request with required
existing payment fields, and include/exclude/vary the delivery fields.

| Test Name (from plan) | What it verifies | BAC |
|---|---|---|
| `T1_orderPersistence_checkoutCompletedWithStandardDelivery_deliveryMethodAndCostSavedToOrder` | POST with `delivery_method: "standard"`, `delivery_base_cost_eur: 5.95`, `delivery_oversized_surcharge_eur: 0.00` → 200; `invoices` table row has those exact values | BAC-12 |
| `T2_orderPersistence_checkoutCompletedWithOversizedSurcharge_surchargeStoredSeparatelyOnOrder` | POST with `delivery_method: "standard"`, `delivery_base_cost_eur: 0.00`, `delivery_oversized_surcharge_eur: 20.00` → 200; `delivery_oversized_surcharge_eur` column = 20.00 (not merged into cost) | BAC-3, BAC-12 |
| `T3_orderPersistence_checkoutSubmittedWithoutDeliveryFields_returns422` | POST without `delivery_method` field → 422 with `errors.delivery_method` | BAC-12 |
| `T4_orderPersistence_checkoutSubmittedWithInvalidDeliveryMethod_returns422` | POST with `delivery_method: "drone"` (invalid) → 422 with `errors.delivery_method` | BAC-12 |

**Additional integration assertions (add to same test class):**
- `delivery_base_cost_eur: -1` → 422
- `delivery_method: "express"` + `delivery_base_cost_eur: 12.95` → 200; persisted correctly
- `delivery_base_cost_eur: 0.00` (free shipping) → 200; zero value stored, not null
- GET `/invoices/{id}` after creation → response contains `delivery_method`,
  `delivery_base_cost_eur`, `delivery_oversized_surcharge_eur`

### Unit / Component Tests

Add to (or create) `sprint5/UI/src/app/checkout/order-overview/order-overview.component.spec.ts`.
Mock the invoice API response with delivery fields populated.

| Test Name (from plan) | What it verifies | BAC |
|---|---|---|
| `T1_orderConfirmation_deliveryMethodCostAndSurchargeDisplayedAsSeparateLines` | When invoice response has `delivery_method: "standard"`, `delivery_base_cost_eur: 5.95`, `delivery_oversized_surcharge_eur: 20.00`, both `[data-test="order-delivery-method"]` and `[data-test="order-oversized-surcharge"]` are visible | BAC-12 |
| `T2_orderConfirmation_noOversizedSurcharge_surchargeLineNotDisplayed` | When `delivery_oversized_surcharge_eur: 0.00`, `[data-test="order-oversized-surcharge"]` is absent from DOM | BAC-12 |

**Additional component-test assertions (add to same spec file):**
- `delivery_method: "zasilkova"` → label shows "Zásilkovna" (not "zasilkova")
- `delivery_base_cost_eur: 0` → cost shows "Free" (not "€0.00")
- `delivery_method: null` (legacy order) → entire delivery block hidden; no error thrown
- `[data-test="order-total"]` shows correct sum: subtotal + base_cost + surcharge

### E2E / Manual Verification

1. Complete a full checkout with a CZ shipping address and a standard (≤25 kg) item.
2. At Step 3 (delivery), select Standard delivery (should show €5.95 for cart ≤€75).
3. Complete payment at Step 4.
4. On the order confirmation page (Step 5):
   - Verify "Standard Delivery €5.95" appears as a separate line.
   - Verify no "Oversized item surcharge" line appears.
   - Verify the order total equals product subtotal + €5.95.
5. Repeat with a 30 kg item in cart:
   - Step 3: Standard delivery free (if subtotal >€75) + €20 surcharge.
   - Step 5: Both delivery and surcharge lines appear.
6. Call `GET /invoices/{new_order_id}` and verify JSON contains all three delivery fields.

---

## Blast Radius

- **Files directly modified (backend):**
  - `sprint5/API/app/Models/Invoice.php` — `$fillable` extension
  - `sprint5/API/app/Http/Controllers/PaymentController.php` — validation + persistence
  - `sprint5/API/app/Http/Controllers/InvoiceController.php` — response schema (conditional)

- **Files directly modified (frontend):**
  - `sprint5/UI/src/app/checkout/payment/payment.component.ts` — payload extension
  - `sprint5/UI/src/app/checkout/order-overview/order-overview.component.ts` — field mapping
  - `sprint5/UI/src/app/checkout/order-overview/order-overview.component.html` — delivery block

- **Files created:**
  - Migration file; `OrderDeliveryPersistenceTest.php`

- **Dependent systems affected:**
  - Any integration test that calls `POST /payment/check` and asserts request shape will
    now require delivery fields — existing tests that omit them will fail with 422. Existing
    `InvoiceTest.php` (and any other feature tests posting to `/payment/check`) must be
    updated to include valid delivery fields.
  - Any API consumer of `GET /invoices/{id}` will receive three new fields — additive,
    no breaking change.

- **Regression risk:**
  - **High-impact**: Adding required delivery fields to `POST /payment/check` is a
    breaking change for existing tests. All existing feature tests calling this endpoint
    must be updated with valid delivery field values or they will fail.
  - **Medium**: `payment.component.ts` changes could disrupt existing E2E tests that
    simulate the payment step — they must now go through Step 3 first to populate
    checkout state delivery.
  - **Low**: The DB migration adds nullable columns — no data loss; existing orders
    remain intact with NULL delivery values.

---

## Threat Model Considerations

- **Input validation:** Three new fields on `POST /payment/check` — all validated server-
  side. `delivery_method` is enum-validated (no free-form string accepted). Numeric fields
  have `min:0` guard. No SQL injection risk (parameterised query builder).
- **Cost tampering:** A client could send `delivery_base_cost_eur: 0` for a cart that
  should pay €5.95. This is a known limitation of trusting client-submitted costs. The
  backend does NOT re-run `DeliveryCalculatorService` to validate the submitted costs.
  Mitigation options (out of scope for Sprint 7): server-side re-validation of delivery
  cost against recalculated value.
- **Data sensitivity:** Delivery method and cost are order metadata — not PII. Stored
  in the `orders` table with existing order data.

---

## Test Data Requirements

- **Seed data (integration tests):**
  - A valid authenticated user (customer) for `actingAs()`
  - A product in stock and in cart (existing test setup)
  - Valid existing `POST /payment/check` payload fields (payment_method, amount, etc.)
  - Three delivery field variants:
    ```
    Standard, paid:    { delivery_method: "standard", delivery_base_cost_eur: 5.95, delivery_oversized_surcharge_eur: 0.00 }
    Standard, free:    { delivery_method: "standard", delivery_base_cost_eur: 0.00, delivery_oversized_surcharge_eur: 0.00 }
    With surcharge:    { delivery_method: "standard", delivery_base_cost_eur: 0.00, delivery_oversized_surcharge_eur: 20.00 }
    Express:           { delivery_method: "express",  delivery_base_cost_eur: 12.95, delivery_oversized_surcharge_eur: 0.00 }
    Zásilkovna:        { delivery_method: "zasilkova", delivery_base_cost_eur: 5.95, delivery_oversized_surcharge_eur: 0.00 }
    Invalid method:    { delivery_method: "drone",    delivery_base_cost_eur: 5.95, delivery_oversized_surcharge_eur: 0.00 }
    ```

- **Angular spec mock data:**
  - Invoice API response objects with all three delivery field variants (surcharge 0 and 20)
  - Legacy response (all three delivery fields null) — must render without error

- **Edge case:** `delivery_oversized_surcharge_eur: 0.00` must produce `0.00` in DB
  (not null), to distinguish "free with no surcharge" from "legacy order (null)".

---

## Out of Scope

- **Delivery cost re-validation on the backend** — `PaymentController` trusts the client-
  submitted delivery cost. Server-side re-verification against `DeliveryCalculatorService`
  is a future hardening task, not in Sprint 7.
- **Admin delivery config endpoint** — Story 2 (`delivery-admin-config`)
- **Delivery step Angular UI** — Story 3 (`delivery-checkout-ui`)
- **Backend delivery calculation engine** — Story 1 (`delivery-calc-engine`)
- **Invoice history / PDF generation** — no changes to invoice PDF if one exists
- **Audit log** — Story 4 does not add audit records for delivery choices
- **Legacy order display** — null delivery fields render as hidden delivery block;
  no backfill migration is required
- **`order-overview.component.spec.ts` creation** — if the spec file does not exist,
  create it; if it exists, extend it with the two plan-defined tests
