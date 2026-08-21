---
id: spec-delivery-checkout-ui
status: specified
created: 2026-08-20
parent: .sdlc/prds/sprint7-delivery-overhaul/plan.md
story: "Story 3"
---

# Specification: Angular Checkout Delivery Step

## Context

Story 1 (`delivery-calc-engine`) created the backend endpoint `POST /delivery/calculate`
that returns available delivery options and costs for a given cart and shipping address.
This story wires the Angular checkout delivery step (step 3 of 5) to that endpoint,
replacing whatever static or placeholder delivery display existed before with a fully
reactive, API-driven component.

The component must:
- Call the backend on load and display the returned delivery options as radio buttons
- Show the oversized surcharge as a separate line item when applicable
- Conditionally show/hide Express and Zásilkovna options based on the API response
- Disable the "Continue to Payment" button when no options are available
- Persist the customer's delivery selection to the shared checkout state so Step 4
  (Payment) can include it in the order submission payload

Story 4 (`delivery-order-persistence`) reads what this story writes to the checkout
state — the three fields `method`, `base_cost_eur`, and `oversized_surcharge_eur` — and
submits them to the backend during payment.

---

## Dependencies

- **Blocked by:** `delivery-calc-engine` (Story 1) — needs `POST /delivery/calculate`
  to exist and return the response schema specified in Story 1's spec.
- **Blocks:** `delivery-order-persistence` (Story 4) — Step 4 reads the delivery
  selection this component writes to the checkout state. Story 4 cannot be
  end-to-end tested until this story is complete. (Story 4 backend work is independent,
  but its Angular order-overview changes depend on this story.)

---

## Architecture Approach

The delivery component is updated to become API-driven:

1. On `ngOnInit()`, the component reads the current cart items and shipping country from
   the shared checkout state, then calls `DeliveryApiService.calculate()`.
2. The service POSTs to `/delivery/calculate` and returns a typed Observable.
3. The component subscribes, stores the response, and renders the delivery options.
4. When the customer selects a method, the component records the selection locally.
5. When the customer clicks "Continue to Payment", the selection is written to the
   shared checkout state and the router navigates to Step 4.
6. If the API returns zero options (unsupported destination), an error banner is shown
   and the Continue button is disabled.
7. `ngOnInit()` always re-fetches — so returning from the cart with modified items
   automatically triggers a fresh calculation.

```
ngOnInit()
  ↓ read from CheckoutStateService
  cartItems + shippingCountry
  ↓
DeliveryApiService.calculate(request)
  ↓ POST /delivery/calculate
  DeliveryOptionsResponse
  ↓ store in component
  render delivery_options[] as radio buttons
  render oversized_surcharge_eur line item (if > 0)
  render order summary totals

User selects option → selectedMethod updated
User clicks "Continue" → write to CheckoutStateService.delivery → navigate to Step 4
```

---

## Business Rules (complete — implement without reading the PRD)

### Delivery option display rules
- The `delivery_options` array from the API contains only **available** options.
  Do NOT filter client-side based on method name — render whatever the API returns.
- Standard delivery is always present (the API guarantees it). Render it first.
- Zásilkovna (`method: "zasilkova"`) appears only when the API includes it. Render it
  second if present. **Label on screen: "Zásilkovna"** (with háček), even though the
  API field value is `"zasilkova"` (without háček — legacy spelling from US4200).
- Express (`method: "express"`) appears only when the API includes it. Render it last.
- When the API returns zero options: show the error banner (BAC-15), hide the options
  list, and disable the Continue button.

### Cost display rules
- For each option: show `base_cost_eur`. If `base_cost_eur === 0`, show "Free" (not
  "€0.00").
- `oversized_surcharge_eur` from the response: show as a **separate** line item labelled
  "Oversized item surcharge" only if `oversized_surcharge_eur > 0`.
- Order summary must show three rows:
  1. Product subtotal — read from checkout state (`cartItems` sum of `price_eur × quantity`)
  2. Delivery (`base_cost_eur` of selected method, or "—" before selection)
  3. Oversized item surcharge (`oversized_surcharge_eur`, only if > 0)
  4. Total — sum of the above
- All monetary values formatted as `€X.XX` in EUR (BAC-12).

### "Continue to Payment" button state
- **Disabled** when:
  - API is loading (in-flight request)
  - `delivery_options` is empty (zero options returned)
  - No method has been selected yet
- **Enabled** once: options loaded AND at least one method selected.

### Reactive recalculation (BAC-13)
- The component calls `DeliveryApiService.calculate()` in `ngOnInit()` every time the
  component initialises. Angular's route-based navigation destroys and re-creates the
  component when the user navigates away and back — this is sufficient to trigger a
  fresh calculation. No explicit change-detection subscription is required.
- If a method was previously selected (stored in component state) and the user navigates
  back and cart changes, the component re-initialises with a clean state (no pre-selected
  method) — the user must re-select.

### Checkout state interface (delivery selection)
After the customer clicks "Continue", write the following object to the shared checkout
state:

| Field | Type | Source |
|---|---|---|
| `method` | string | `selectedOption.method` from API response |
| `base_cost_eur` | number | `selectedOption.base_cost_eur` from API response |
| `oversized_surcharge_eur` | number | `response.oversized_surcharge_eur` from API response |

This object is read by Story 4's `PaymentController` (backend) via the Angular checkout
submission payload.

### Cart data prerequisite
The delivery component needs the following fields per cart item from the checkout state:

| Field | Source |
|---|---|
| `product_id` | cart item |
| `weight_kg` | product weight field (added in Sprint 6 US4200) |
| `price_eur` | cart item unit price |
| `quantity` | cart item quantity |
| `in_stock` | product stock status |

**Assumption**: The checkout state (populated in Step 1 from the cart/products API)
already carries `weight_kg` and `in_stock` per item. If the checkout state does NOT
have these fields, the delivery component must fetch product details from the product
API to enrich the cart items before calling `DeliveryApiService.calculate()`. Verify
against the existing checkout state structure before building.

The shipping country comes from `checkoutState.shippingAddress.country` (set in Step 2).

---

## Files to Create

| File Path | Purpose | Pattern Reference |
|---|---|---|
| `sprint5/UI/src/app/_services/delivery-api.service.ts` | HTTP service: POSTs to `/delivery/calculate`, returns `Observable<DeliveryOptionsResponse>` | `sprint5/UI/src/app/_services/recommendation-api.service.ts` |
| `sprint5/UI/src/app/models/delivery-options-request.ts` | TypeScript interface for the `POST /delivery/calculate` request body | `sprint5/UI/src/app/models/recommendation-request.ts` |
| `sprint5/UI/src/app/models/delivery-options-response.ts` | TypeScript interfaces for the `POST /delivery/calculate` response | `sprint5/UI/src/app/models/recommendation-response.ts` |

---

## Files to Modify

| File Path | Change Description | Target Function/Component |
|---|---|---|
| `sprint5/UI/src/app/checkout/delivery/delivery.component.ts` | Inject `DeliveryApiService` and `CheckoutStateService`; call `calculate()` in `ngOnInit()`; handle option selection; persist selection on Continue | `DeliveryComponent` class |
| `sprint5/UI/src/app/checkout/delivery/delivery.component.html` | Replace placeholder delivery markup with: radio-button options list, cost summary rows, error banner, data-test attributes | `DeliveryComponent` template |
| `sprint5/UI/src/app/checkout/delivery/delivery.component.spec.ts` | Add/replace tests for all plan test cases using `HttpClientTestingModule` | `DeliveryComponent` spec |

---

## Data and Contract Mappings

### TypeScript interfaces

**`DeliveryCalculateRequest`** (file: `delivery-options-request.ts`):

| Field | Type | Required | Notes |
|---|---|---|---|
| `cart_items` | `CartItemDeliveryInfo[]` | yes | array, min 1 |
| `shipping_country` | `string` | yes | 2-char ISO code |

**`CartItemDeliveryInfo`** (nested interface in same file):

| Field | Type | Notes |
|---|---|---|
| `product_id` | `number` | |
| `weight_kg` | `number` | |
| `price_eur` | `number` | unit price, not total |
| `quantity` | `number` | |
| `in_stock` | `boolean` | |

**`DeliveryOptionsResponse`** (file: `delivery-options-response.ts`):

| Field | Type | Notes |
|---|---|---|
| `delivery_options` | `DeliveryOption[]` | may be empty (unsupported destination) |
| `oversized_surcharge_eur` | `number` | 0 when no oversized items |
| `non_oversized_total_eur` | `number` | for UI display in order summary |

**`DeliveryOption`** (nested interface in same file):

| Field | Type | Notes |
|---|---|---|
| `method` | `'standard' \| 'zasilkova' \| 'express'` | use as key; display via `label` field |
| `label` | `string` | e.g., "Standard Delivery", "Zásilkovna", "Express Next-Day" |
| `base_cost_eur` | `number` | 0 for free, positive otherwise |
| `available` | `boolean` | always `true` for returned options |

### Component state model (internal to `DeliveryComponent`)

| Property | Type | Lifecycle |
|---|---|---|
| `deliveryOptions` | `DeliveryOption[]` | populated on API response |
| `oversizedSurcharge` | `number` | populated on API response |
| `nonOversizedTotal` | `number` | populated on API response |
| `selectedOption` | `DeliveryOption \| null` | set by user radio selection |
| `isLoading` | `boolean` | true while API call in flight |
| `hasError` | `boolean` | true if API call fails |
| `noOptionsAvailable` | `boolean` | true if `delivery_options.length === 0` |

### data-test attribute map

| Element | `data-test` value |
|---|---|
| Radio button — Standard | `"delivery-standard-option"` |
| Radio button — Zásilkovna | `"delivery-zasilkova-option"` |
| Radio button — Express | `"delivery-express-option"` |
| Oversized surcharge line | `"delivery-oversized-surcharge"` |
| Order total line | `"delivery-total-cost"` |
| "Continue to Payment" button | `"delivery-continue-button"` |
| Error banner (unsupported dest.) | `"delivery-error-unsupported-destination"` |
| Loading indicator | `"delivery-loading"` |

All attribute values use kebab-case (matching existing `payment-success-message` convention).

### Checkout state write (on Continue)

| Property written | Source |
|---|---|
| `checkoutState.delivery.method` | `this.selectedOption.method` |
| `checkoutState.delivery.base_cost_eur` | `this.selectedOption.base_cost_eur` |
| `checkoutState.delivery.oversized_surcharge_eur` | `this.oversizedSurcharge` |

---

## Key Patterns to Follow

| Pattern | Reference Implementation |
|---|---|
| Angular HTTP service (`@Injectable`, `HttpClient`, `Observable`) | `sprint5/UI/src/app/_services/recommendation-api.service.ts` |
| Checkout component structure (template + data binding) | `sprint5/UI/src/app/checkout/payment/payment.component.ts` |
| `data-test` attribute naming convention | `sprint5/UI/src/app/checkout/payment/payment.component.html` |
| TypeScript interface as DTO model | `sprint5/UI/src/app/models/recommendation-request.ts` |
| Component spec with `HttpClientTestingModule` | `sprint5/UI/src/app/header/header.component.spec.ts` |
| Checkout state service read/write | `sprint5/UI/src/app/_services/checkout-attribution.service.ts` (existing) |
| Loading + error state pattern | `sprint5/UI/src/app/checkout/payment/payment.component.html` (success/error alerts) |

---

## Existing Code to Reuse

| Existing Asset | Location | How to Reuse | Why Not Create New |
|---|---|---|---|
| `CheckoutStateService` (or equivalent) | `sprint5/UI/src/app/_services/` | Inject to read `cartItems` + `shippingCountry`, and write `delivery` selection | Checkout state is already owned by this service — delivery component must not manage its own copy |
| `HttpClient` (Angular built-in) | `@angular/common/http` | Inject in `DeliveryApiService` | Already registered in app module; no additional setup needed |
| `HttpClientTestingModule` | `@angular/common/http/testing` | Import in spec `TestBed` for mocking `POST /delivery/calculate` | Standard Angular testing pattern; avoids real HTTP in unit tests |
| Bootstrap radio button / form classes | Already imported in app | Use `form-check` + `form-check-input` + `form-check-label` classes for delivery option radios | Bootstrap 5 already the UI library; no additional CSS framework |
| Existing checkout route guard (if any) | `sprint5/UI/src/app/checkout/` | Ensure delivery step guard prevents advancement without selection | Reuse guard pattern to add "delivery selected" condition |

---

## Verification Steps

### Unit / Component Tests

All tests live in `sprint5/UI/src/app/checkout/delivery/delivery.component.spec.ts`.
Each test must configure `TestBed` with `HttpClientTestingModule`, create the component,
and use `HttpTestingController` to flush a mock `DeliveryOptionsResponse`.

| Test Name (from plan) | What it verifies | BAC |
|---|---|---|
| `T1_deliveryUI_nonOversizedCartAbove75EUR_freeShippingLabelDisplayed` | When API returns `base_cost_eur: 0.00` for Standard, the template shows "Free" (not "€0.00") | BAC-1 |
| `T2_deliveryUI_nonOversizedCartAt75EUR_5_95FlatRateDisplayed` | When API returns `base_cost_eur: 5.95`, template shows "€5.95" | BAC-2 |
| `T1_deliveryUI_cartContainsOversizedItem_20EURSurchargeShownAsSeparateLine` | When `oversized_surcharge_eur: 20.00`, `[data-test="delivery-oversized-surcharge"]` is visible | BAC-3 |
| `T2_deliveryUI_onlyOversizedItemInCart_flatRatePlusSurchargeTotal25_95EUR` | `[data-test="delivery-total-cost"]` shows €25.95 when base = €5.95 + surcharge = €20.00 | BAC-4 |
| `T3_deliveryUI_mixedCartWith80EURNonOversized_freeShippingPlusSurchargeTotal20EUR` | Total cost = €20.00 (free base + €20.00 surcharge) | BAC-5 |
| `T1_deliveryUI_allItemsInStockNoneOversized_expressOptionVisible` | `[data-test="delivery-express-option"]` present in DOM when API response includes express | BAC-6 |
| `T2_deliveryUI_cartContainsOutOfStockItem_expressOptionNotVisible` | `[data-test="delivery-express-option"]` absent when API response omits express | BAC-7 |
| `T3_deliveryUI_cartContainsOversizedItem_expressOptionNotVisibleRegardlessOfStock` | `[data-test="delivery-express-option"]` absent when API omits express (oversized gate) | BAC-8 |
| `T1_deliveryUI_shippingAddressCZ_zasilkovnaOptionDisplayed` | `[data-test="delivery-zasilkova-option"]` present when API response includes zasilkova | BAC-9 |
| `T2_deliveryUI_shippingAddressDACH_zasilkovnaOptionDisplayed` | Same assertion for DACH-country responses | BAC-9 |
| `T3_deliveryUI_shippingAddressOutsideCZandDACH_zasilkovnaOptionNotDisplayed` | `[data-test="delivery-zasilkova-option"]` absent when API omits zasilkova | BAC-10 |
| `T1_deliveryUI_zasilkovnaSelectedCartAbove75EUR_samePricingAsStandard` | Zásilkovna radio shows "Free" when `base_cost_eur: 0.00` — identical to Standard | BAC-11 |
| `T2_deliveryUI_zasilkovnaSelected_expressOptionNotAvailable` | When API response includes zasilkova but not express, express radio absent | BAC-11 |
| `T1_deliveryUI_deliveryMethodSelected_subtotalDeliveryCostAndTotalShownSeparately` | After selecting Standard, three cost lines (subtotal, delivery, total) visible; total = subtotal + delivery | BAC-12 |
| `T1_deliveryUI_userReturnsFromCartWithModifiedItems_deliveryCostRecalculated` | Component re-initialises (`ngOnInit` called again); `HttpTestingController.expectOne('/delivery/calculate')` verifies second API call was made | BAC-13 |
| `T5_deliveryUI_unsupportedShippingDestination_errorBannerDisplayedAndPaymentButtonDisabled` | API returns `delivery_options: []`; `[data-test="delivery-error-unsupported-destination"]` visible; `[data-test="delivery-continue-button"]` has `disabled` attribute | BAC-15 |

**Additional component-test assertions (add to same spec file):**
- Before any selection: Continue button is disabled
- After selecting any option: Continue button is enabled
- When API call is in flight (`isLoading: true`): Continue button is disabled and `[data-test="delivery-loading"]` visible
- `oversized_surcharge_eur: 0.00` → `[data-test="delivery-oversized-surcharge"]` is absent from DOM

### E2E Test

| Test Name (from plan) | What it verifies | BAC |
|---|---|---|
| `T5_deliveryCheckout_customerSelectsZasilkovnaForCZAddress_orderCompletesWithCorrectCost` | Full browser checkout: add item to cart → enter CZ address → step 3 shows Zásilkovna → select it → continue → complete payment → order confirmation shows Zásilkovna cost | BAC-9, BAC-11, BAC-12 |

This test requires Stories 1, 3, and 4 to all be deployed. It is an integration E2E
that exercises the complete checkout flow end-to-end.

### Manual Verification

1. Navigate to `http://localhost:4200` and add a product (≤25 kg, in stock) to cart.
2. Proceed through Step 1 (cart) → Step 2 (address, enter CZ country).
3. At Step 3 (delivery), verify:
   - Standard delivery radio is shown
   - Zásilkovna radio is shown (CZ address)
   - Express radio is shown (item in stock, not oversized)
   - Oversized surcharge line is absent
4. Select Standard → verify Continue button is enabled.
5. Now add a product weighing 30 kg (oversized) to cart, navigate back to Step 3:
   - Oversized surcharge line shows €20.00
   - Express radio is absent
6. Change shipping country to "US" (Step 2) → return to Step 3:
   - Zásilkovna radio is absent

---

## Blast Radius

- **Files directly modified:**
  - `sprint5/UI/src/app/checkout/delivery/delivery.component.ts`
  - `sprint5/UI/src/app/checkout/delivery/delivery.component.html`
  - `sprint5/UI/src/app/checkout/delivery/delivery.component.spec.ts`

- **Files created (net new):**
  - `delivery-api.service.ts`, `delivery-options-request.ts`, `delivery-options-response.ts`

- **Dependent systems affected:**
  - `delivery-order-persistence` (Story 4): reads `checkoutState.delivery` written by this
    story's Continue action — Story 4 cannot submit valid orders until this story exists
  - `CheckoutStateService`: extended to include a `delivery` slot; any other component that
    reads the full checkout state object may be affected if delivery is now a required field

- **Regression risk:**
  - The payment component (Step 4) must be able to handle the case where `checkoutState.delivery`
    is undefined (user skipped directly to payment) — add a guard or null-check there
  - If `CheckoutStateService` is shared across all steps, adding a `delivery` field is
    additive and safe; verify the checkout reset flow (on new cart) clears `delivery` too
  - Existing delivery component tests (if any) will be replaced/updated by this story's spec file

---

## Threat Model Considerations

- **New attack surface:** `DeliveryApiService` calls `POST /delivery/calculate` — this is
  a public backend endpoint, so no auth token is attached. The request payload (cart items,
  shipping country) contains no PII.
- **Data sensitivity:** `weight_kg` and `in_stock` are product metadata, not user PII.
  No credentials or financial data are sent to or from this endpoint.
- **Input validation:** The backend (Story 1) validates all fields. The Angular service
  must only send the required fields — no injection vector exists since the payload is
  constructed from typed interfaces.
- **State tampering:** The `checkoutState.delivery.base_cost_eur` written by this component
  is re-validated by the backend in Story 4 — so a client-side tamper of the cost does not
  affect the final order. The backend is the source of truth.

---

## Test Data Requirements

- **Fixtures:** No separate fixture files needed — test data is constructed inline in each
  spec as typed `DeliveryOptionsResponse` objects.
- **Mock API responses for spec tests:**
  ```
  Free shipping response:
    { delivery_options: [{method:"standard", label:"Standard Delivery", base_cost_eur:0, available:true}],
      oversized_surcharge_eur: 0, non_oversized_total_eur: 80 }

  Flat rate + Zásilkovna + Express:
    { delivery_options: [
        {method:"standard",  label:"Standard Delivery",  base_cost_eur:5.95, available:true},
        {method:"zasilkova", label:"Zásilkovna",          base_cost_eur:5.95, available:true},
        {method:"express",   label:"Express Next-Day",    base_cost_eur:12.95, available:true}
      ],
      oversized_surcharge_eur: 0, non_oversized_total_eur: 60 }

  Oversized — no Express:
    { delivery_options: [
        {method:"standard",  label:"Standard Delivery", base_cost_eur:5.95, available:true}
      ],
      oversized_surcharge_eur: 20, non_oversized_total_eur: 0 }

  Unsupported destination:
    { delivery_options: [], oversized_surcharge_eur: 0, non_oversized_total_eur: 0 }
  ```

- **Edge case data:**
  - `base_cost_eur === 0` → display "Free", not "€0.00"
  - `oversized_surcharge_eur === 0` → surcharge line absent from DOM
  - All three options returned → all three radios visible; Express is last

---

## Out of Scope

- **Backend delivery calculation logic** — specified and built in Story 1 (`delivery-calc-engine`)
- **Admin config update** — Story 2 (`delivery-admin-config`)
- **Order persistence** (saving delivery choice to DB) — Story 4 (`delivery-order-persistence`)
- **Order confirmation page delivery display** — Story 4 updates the order-overview component
- **Zásilkovna pickup-point map or parcel-shop selection** — not in any Sprint 7 story
- **Multi-currency display** — Sprint 7 uses EUR only; no CZK/USD conversion in the UI
- **Angular SSR / server-side rendering** of the delivery step — not in scope for Sprint 7
- **i18n/l10n of delivery labels** — labels come from the API `label` field; no translation
  keys needed in Sprint 7 (Czech localisation is a separate Sprint 6 story)
