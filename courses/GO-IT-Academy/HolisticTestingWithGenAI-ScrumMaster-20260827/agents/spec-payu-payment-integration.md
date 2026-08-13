---
id: spec-payu-payment-integration
status: specified
created: 2026-08-12
parent: ../../prds/toolshop-czech-market-sprint6/plan.md
story: Story 3
---

# Specification: PayU Payment Integration

## Context

ToolShop is expanding into the Czech market. Czech customers expect to pay with PayU Czech Republic — the dominant local payment method. This story introduces the real `PayUClient` service wrapping the PayU CZ REST API, a country-gated PayU option in the checkout UI, and a signed webhook endpoint for asynchronous payment status callbacks.

Story 6 (mock-payu-service) **must be completed first**. It establishes the `payu-cz` branch in `PaymentController@check`, the `CheckoutPaymentComponent` success/error alert UI, and the mock endpoint used by this story's integration tests. Story 3 replaces the mock delegation inside `PaymentController` with a call to `PayUClient`, adds the Angular radio button gated by CZ address, and wires the webhook callback that drives order status transitions.

The integration tests for this story continue to target the mock endpoint (via a configurable `PAYU_API_URL`) — real sandbox credentials are only needed for pre-production testing. See External Dependencies below.

## Dependencies

- **Blocked by:** `mock-payu-service` (Story 6) — must exist before this story starts. Story 6 provides: the `payu-cz` branch in `PaymentController@check`, `CheckoutPaymentComponent` success/error response handling, and `POST /mock/payu/orders` for integration tests.
- **Blocks:** nothing — Story 3 is on the PayU track's critical path but no subsequent story depends on it.

## Architecture Approach

```
Czech checkout (Angular)
  └─ PayU radio button (shown only when billingCountry=CZ OR shippingCountry=CZ)
        │
        ▼  POST /payment/check  { payment_method: "payu-cz", amount, currency, order_id }
  PaymentController@check
        │
        ▼  PayUClient::createOrder(amount, currency, orderId)
  PayU CZ REST API  (PAYU_API_URL from .env — points to mock in test, real API in prod)
        │
        ├─ success → return orderId + redirectUri to frontend → frontend redirects to PayU hosted page
        │
        └─ failure → return error to frontend (existing error alert from Story 6)

  PayU hosted page  (customer completes/cancels payment)
        │
        ▼  POST /payment/payu/callback  (signed webhook, PayU → ToolShop)
  PayUWebhookController
        │  verify HMAC-SHA256 signature first — reject if invalid
        ├─ status COMPLETED  → update order.status = payment_confirmed
        └─ status CANCELED   → update order.status = payment_cancelled (no order created)
```

**Key design decision — configurable base URL for test injection:**
`PayUClient` reads its API base URL from `PAYU_API_URL` in `.env`. In integration tests, this is set to the mock endpoint (`/mock/payu/orders`). In production, it is set to the real PayU CZ REST API base URL. This avoids any DI mock injection complexity and is consistent with how ToolShop handles other external service URLs.

**Country guard logic** (where it lives — `PaymentController`, not the Angular component):
The backend determines which payment methods are available based on the checkout address. When the frontend requests available payment methods, the backend response includes `payu-cz` only when `billingCountry === 'CZ'` OR `shippingCountry === 'CZ'`. The Angular component reads the payment methods list from the API response and renders available options — it does not implement country logic independently.

**No card data in DB:** `PayUClient` initiates orders via PayU's hosted-page model. Card capture occurs entirely on PayU's infrastructure. ToolShop stores only `transaction_id` (from PayU webhook) and `order_status` on the order record.

## Files to Create

| File Path | Purpose | Pattern Reference |
|-----------|---------|-------------------|
| `API/app/Services/PayUClient.php` | Wraps PayU CZ REST API: `createOrder()` and `getOrderStatus()`. Reads base URL and credentials from `.env`. Returns a DTO/array with `redirectUri` (on success) or throws a `PayUException` (on failure). | Existing service pattern in `API/app/Services/` (see any existing HTTP client service) |
| `API/app/Http/Controllers/PayUWebhookController.php` | Handles `POST /payment/payu/callback`. Verifies HMAC-SHA256 signature against `PAYU_WEBHOOK_KEY`. Updates order status (`payment_confirmed` / `payment_cancelled`) on valid payload. Returns 200 immediately after DB write. | `API/app/Http/Controllers/PaymentController.php` |
| `API/app/Exceptions/PayUException.php` | Typed exception for PayU API failures, carrying HTTP status and error message from PayU response. | Existing exception class pattern in `API/app/Exceptions/` |

## Files to Modify

| File Path | Change Description | Target Function/Component |
|-----------|-------------------|---------------------------|
| `API/app/Http/Controllers/PaymentController.php` | Replace the direct internal HTTP call to `/mock/payu/orders` (added in Story 6) with a call to `PayUClient::createOrder()`. Map `PayUClient` success (redirectUri) and `PayUException` failure to the existing checkout response contract. | `check()` method, `payu-cz` branch only |
| `API/routes/api.php` | Add signed webhook route `POST /payment/payu/callback` guarded by a signature-verification middleware (not registered behind `APP_ENV` guard — this route must exist in production). Remove or separate from Story 6's `APP_ENV` guard block. | Route registration block |
| `UI/src/app/checkout/payment/payment.component.ts` | Read `availablePaymentMethods` from the payment-step API response. Render the PayU radio button only when `payu-cz` is present in the list (i.e., the backend has already applied the CZ country guard). | `PaymentComponent` class |
| `UI/src/app/checkout/payment/payment.component.html` | Add a PayU radio button alongside existing payment options. Conditionally render using `*ngIf="payuAvailable"` (or equivalent binding from `availablePaymentMethods`). | Payment options template block |
| `UI/src/environments/environment.ts` | No new constant needed if frontend always calls ToolShop's backend API (`environment.apiUrl`). If a separate PayU redirect return URL is configured client-side, add `payuReturnUrl` and `payuCancelUrl`. | Environment constants |
| `API/.env.example` | Add `PAYU_API_URL`, `PAYU_CLIENT_ID`, `PAYU_CLIENT_SECRET`, `PAYU_WEBHOOK_KEY`. | `.env.example` template |

> **Note:** `environment.ts` modification is only needed if the redirect return/cancel URLs are configured client-side. Confirm with the existing checkout redirect pattern before modifying.

## Data and Contract Mappings

### PayUClient::createOrder() — outbound request to PayU CZ REST API

| ToolShop Field | PayU API Field | Notes |
|---|---|---|
| `amount` (cart total in smallest currency unit, e.g. haléře) | `totalAmount` | PayU CZ expects amount in smallest unit (integer). Confirm unit with PayU docs at build time. |
| `currency` | `currencyCode` | `CZK` for Czech customers |
| `order_id` (ToolShop cart/session ID) | `extOrderId` | PayU uses this to correlate callbacks to ToolShop orders |
| `PAYU_CLIENT_ID` + `PAYU_CLIENT_SECRET` | OAuth2 bearer token (client credentials flow) | `PayUClient` must obtain and cache a short-lived access token before each API call |
| — | `notifyUrl` | Set to `POST /payment/payu/callback` (full URL). Required — PayU sends HMAC-signed webhook here. |
| — | `continueUrl` | Return URL after hosted-page completion (success/cancel). Configure in `.env.example` as `PAYU_RETURN_URL`. |

PayU CZ REST API response on order creation:
- `status.statusCode == "SUCCESS"` → `orderId`, `redirectUri` returned
- Any other status → treat as failure; surface error message to customer

### PayU Webhook Callback — inbound from PayU to `POST /payment/payu/callback`

| PayU Webhook Field | Action |
|---|---|
| `order.extOrderId` | Maps to ToolShop order/cart ID |
| `order.status` | `COMPLETED` → set `order.status = payment_confirmed`; `CANCELED` → set `order.status = payment_cancelled` |
| `order.orderId` | Store as `transaction_id` on the ToolShop order record |
| Signature header (`OpenPayU-Signature`) | Verify HMAC-SHA256 against `PAYU_WEBHOOK_KEY` before any DB write |

**HMAC verification pseudocode** (implement in `PayUWebhookController` or dedicated middleware):
```
incoming_sig = parse header OpenPayU-Signature → extract "signature" value
expected_sig = hash_hmac('sha256', raw_request_body, PAYU_WEBHOOK_KEY)
if not constant_time_equals(incoming_sig, expected_sig): return 401 Unauthorized
```
> Use `hash_equals()` (PHP constant-time comparison) to prevent timing attacks.

### PaymentController@check — updated payu-cz branch

Story 6 established:
```
POST /payment/check { payment_method: "payu-cz" }
→ direct call to /mock/payu/orders
→ 200 { message } or 422 { error }
```

Story 3 replaces the direct mock call:
```
POST /payment/check { payment_method: "payu-cz" }
→ PayUClient::createOrder(amount, currency, orderId)
→ success: return 200 { redirectUri }   ← NEW: frontend redirects customer to PayU hosted page
→ failure: return 422 { error }         ← unchanged from Story 6 contract
```

**Frontend contract change**: On `200` from `payment/check`, the `CheckoutPaymentComponent` now receives `{ redirectUri }` instead of `{ message }`. The component must redirect (`window.location.href = redirectUri`) rather than showing a success alert inline. The success alert (from Story 6) is shown ONLY if no `redirectUri` is present in the response (i.e., during test/mock scenarios where mock returns `{ message }`).

> **Design note**: This is a breaking change to the Story 6 frontend contract. Confirm with the team whether Story 3 should: (A) always redirect when `redirectUri` is present in the 200 response, showing the success alert only as a fallback; or (B) introduce a separate endpoint for PayU order creation. **Recommendation: Option A** — check `redirectUri` in response first; redirect if present, otherwise fall back to success alert. This keeps full backward compatibility with mock tests.

### Country Guard — `PaymentController` payment methods endpoint

The endpoint that returns available payment methods (or the checkout address endpoint response that feeds into the payment step) must include `payu-cz` in the list when:
- `billingCountry === 'CZ'` **OR** `shippingCountry === 'CZ'`

> `build-task` must locate the existing payment-methods resolver in `PaymentController` and apply the condition there. Do not duplicate this logic in Angular — the UI renders what the API says is available.

## Key Patterns to Follow

| Pattern | Reference Implementation |
|---------|------------------------|
| External HTTP client service (configurable base URL from `.env`) | Existing service in `API/app/Services/` — locate any HTTP-based service at build time |
| Controller with typed exception handling | `API/app/Http/Controllers/PaymentController.php` (Story 6 payu-cz branch) |
| Signed/webhook route registration | Existing webhook route in `API/routes/api.php` if present; otherwise use `Route::post(...)->withoutMiddleware([...])` pattern for non-CSRF routes |
| Angular conditional radio button | Existing payment method toggle in `UI/src/app/checkout/payment/payment.component.html` |
| HMAC constant-time comparison | PHP `hash_equals()` — standard library, no extra package |

## Existing Code to Reuse

| Existing Asset | Location | How to Reuse | Why Not Create New |
|---|---|---|---|
| `PaymentController@check` `payu-cz` branch | `API/app/Http/Controllers/PaymentController.php` (added in Story 6) | Extend — replace the mock HTTP call with a `PayUClient` injection. Keep the 422 error mapping unchanged. | Story 6 already established the branch structure; creating new would duplicate controller logic |
| `CheckoutPaymentComponent` success/error alert handling | `UI/src/app/checkout/payment/payment.component.ts` (added in Story 6) | Extend — add `redirectUri` handling on 200 response; keep existing `message`/`error` mapping | Success/error alert template already rendered from Story 6; only the 200-with-redirectUri case is new |
| Existing payment method list rendering | `UI/src/app/checkout/payment/payment.component.html` | Add PayU radio button to existing options list block | Same HTML structure and binding pattern used by all other payment options |
| `.env.example` | `API/.env.example` | Append PayU credential keys | Standard credentials-template pattern already in use |

## Verification Steps

### Unit Tests

| Test Name | What it verifies | BAC |
|---|---|---|
| `test_payu_option_displayed_when_billing_country_is_cz` | Payment methods endpoint returns `payu-cz` in available options when `billingCountry=CZ` | BAC-10 |
| `test_payu_option_displayed_when_shipping_country_is_cz` | Payment methods endpoint returns `payu-cz` when `shippingCountry=CZ` even if `billingCountry≠CZ` | BAC-10 |
| `test_payu_option_not_displayed_for_non_czech_billing_and_shipping_address` | `payu-cz` absent from payment options when both billing and shipping country are non-CZ (e.g. `AT`, `US`) | BAC-11 |
| `test_payu_order_record_contains_no_card_number_cvv_or_raw_credentials` | After a completed PayU webhook callback, the orders table row contains only `transaction_id` and `order_status`; no card-data columns exist or are populated | BAC-14 |

### Integration Tests

| Test Name | What it verifies | BAC |
|---|---|---|
| `test_successful_payu_gateway_response_creates_order_with_payment_confirmed_status` | `PaymentController@check` with `payment_method=payu-cz` calls `PayUClient` (pointed at mock via `PAYU_API_URL`), receives mock 200 success, creates order with `status=payment_confirmed` | BAC-12 |
| `test_declined_payu_gateway_response_returns_error_and_does_not_create_order` | `PaymentController@check` receives mock 422 decline from `PayUClient`, returns 422 error to frontend, and no order row is written to the database | BAC-13 |

### E2E / Manual Verification

**E2E test (Playwright):**

`test_cancelled_payu_checkout_allows_customer_to_retry_without_restarting` — BAC-13
1. Navigate to ToolShop checkout with a CZ billing or shipping address in the cart.
2. Reach the payment step — confirm PayU radio button is visible.
3. Select PayU and click Confirm.
4. Simulate a failed/cancelled payment (use mock scenario `declined`).
5. Assert: red error alert is shown with the error text.
6. Assert: Confirm button is still active (not replaced by Buy now).
7. Assert: PayU radio button can be re-selected.
8. Click Confirm again — assert the request is sent without requiring checkout restart.

**Manual verification (sandbox credentials required):**
- Set `PAYU_API_URL` to PayU CZ sandbox URL and configure sandbox credentials in `.env`.
- Complete a checkout with a CZ address, select PayU, confirm.
- Confirm redirect to PayU hosted payment page occurs.
- Complete payment on sandbox page; confirm return to ToolShop with `payment_confirmed` status.
- Repeat with cancellation; confirm order is NOT created and retry is possible.

## Blast Radius

- **Files directly modified:** `PaymentController.php` (payu-cz branch logic), `routes/api.php` (new webhook route), `payment.component.ts` + `.html` (PayU radio + redirect handling), `.env.example`
- **Files created:** `PayUClient.php`, `PayUWebhookController.php`, `PayUException.php`
- **Dependent systems affected:**
  - `CheckoutPaymentComponent` response contract changes from `{ message }` on 200 to `{ redirectUri }` — this must remain backward-compatible with mock test scenarios (see Data Mappings above)
  - Story 6 mock route (`POST /mock/payu/orders`) is not modified — it remains the integration test target
  - Existing payment methods (bank transfer, credit card, etc.) are **not touched** — the `payu-cz` branch is isolated
- **Regression risk:** The `check()` method in `PaymentController` handles multiple payment methods. Introducing the `payu-cz` branch must not affect the `else` branch used by all other methods. Guard with a unit test that asserts non-PayU payment methods return unchanged behaviour.

## Threat Model Considerations

- **New attack surface:** `POST /payment/payu/callback` — publicly accessible webhook endpoint. Must verify PayU HMAC-SHA256 signature on every request before any database operation. Unauthenticated requests must return 401 and log the attempt.
- **Data sensitivity:** PayU API credentials (`PAYU_CLIENT_ID`, `PAYU_CLIENT_SECRET`) are high-sensitivity secrets. Must be read from `.env` only — never hardcoded, never logged, never returned in API responses.
- **HMAC timing attack:** Use PHP `hash_equals()` (constant-time comparison) when verifying the webhook signature. Do not use `===` or `strcmp`.
- **Mass assignment / order status injection:** `PayUWebhookController` must only accept `COMPLETED` and `CANCELED` as valid status transitions. Any other `order.status` value from PayU must be rejected (log and return 200 to prevent PayU retries, but do not update order).
- **No card data in persistence layer:** `PayUClient` uses PayU's hosted-page model — card data is captured by PayU, never transmitted to or through ToolShop. DB schema must not have card-data columns on the orders table. Verify at build time.
- **Auth/authz changes:** New webhook route must be excluded from CSRF middleware (it is called by PayU, not the browser) but must use the HMAC guard as its authentication mechanism. Do not add it to `auth:sanctum` or `auth:jwt` middleware groups.
- **Input validation:** Webhook body fields (`extOrderId`, `orderId`, `status`) must be validated as non-empty strings before DB lookup. Reject with 422 if malformed.

## Test Data Requirements

- **Fixtures needed:**
  - A test order in `payment_pending` status with a known `extOrderId` (for webhook integration tests)
  - A CZ-country customer address fixture (for BAC-10/11 payment method tests)
  - A non-CZ customer address fixture (AT or US, for BAC-11)
- **Seed data:** One product in the cart with a known price and weight (for checkout integration tests)
- **Edge case data:**
  - Webhook body with `status = COMPLETED` and valid HMAC → order updates
  - Webhook body with `status = COMPLETED` and invalid HMAC → rejected, order unchanged
  - Webhook body with unknown `status` value (e.g. `PENDING`) → no DB write, 200 returned
  - `billingCountry = CZ`, `shippingCountry = DE` → PayU shown (BAC-10)
  - `billingCountry = DE`, `shippingCountry = CZ` → PayU shown (BAC-10)
  - `billingCountry = DE`, `shippingCountry = AT` → PayU not shown (BAC-11)

## Out of Scope

- **Real PayU redirect UI flow end-to-end testing** — sandbox testing requires credentials provisioned by Business/DevOps (listed as an External Action Item in the plan). The integration tests use the mock endpoint.
- **PayU CZ sandbox credential provisioning** — this is an external action item in the plan (Business/DevOps owner).
- **Status polling** — `PayUClient` must expose `getOrderStatus()` per the plan description, but active polling is not required for Sprint 6. The webhook callback drives status updates. `getOrderStatus()` is scaffolded for completeness.
- **PayU payment in CZK currency routing** — the checkout currently uses EUR/USD. Currency selection for PayU payments (CZK) is dictated by the billing country logic established in Story 4 (Delivery Pricing). Story 3 passes `currency` as provided by the checkout step — it does not independently set currency.
- **Czech translation of PayU checkout copy** — PayU's hosted payment page is served by PayU, not ToolShop. ToolShop's translation layer (Story 1) does not apply.
- **Mobile app (React Native)** — this story targets the Angular web app only.
- **GraphQL API** — no PayU mutation is added to the GraphQL schema; REST only.
- **Invoice/PDF generation changes** — the existing invoice generation handles `payu-cz` orders identically to other payment methods.
- **Chat widget** — not impacted.
