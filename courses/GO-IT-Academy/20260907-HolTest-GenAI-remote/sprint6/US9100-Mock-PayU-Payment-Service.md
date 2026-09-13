# US9100 - Backend mock PayU payment service for checkout testing

Background: Toolshop offers PayU Czech Republic for customers with country CZ. Real PayU calls are unsuitable for training and automated checkout tests (credentials, rate limits, flakiness). The backend should expose an embedded mock PayU service that accepts payment requests and returns configurable HTTP responses, so testers can validate success and failure paths in the checkout payment step without external stub servers.

## User Story: Backend mock PayU payment service

As a tester (or developer),
I want the backend to provide a mock PayU payment service with configurable HTTP responses,
so that I can test the checkout flow with PayU without calling the real PayU API and verify that the checkout UI displays the correct success or error message for each scenario.

## Acceptance Criteria:

### UC1: Mock PayU endpoint accepts payment requests

Given the mock PayU service is running as part of the Laravel API
When  a client sends `POST /mock/payu/orders` with a JSON body containing at least `amount`, `currency`, and `order_id`
Then  the mock responds with JSON and an HTTP status code determined by the active scenario

### UC2: Default scenario returns success

Given no scenario override is configured
When  `POST /mock/payu/orders` is called with valid input
Then  the mock returns `200 OK` with a body such as `{ "status": "SUCCESS", "transaction_id": "<generated>", "message": "Payment was successful" }`

### UC3: Scenario-based error responses

Given a scenario is selected (via request header `X-PayU-Mock-Scenario`, query parameter, or env default — pick one mechanism in implementation)
When  the mock receives a payment request
Then  it returns the configured response, supporting at minimum:

| Scenario     | HTTP status | Response body (example)                          |
| ------------ | ----------- | ------------------------------------------------ |
| `success`    | 200 OK      | `{ "status": "SUCCESS", "transaction_id": "…", "message": "Payment was successful" }` |
| `declined`   | 422         | `{ "error": "Payment declined" }`                |
| `unavailable`| 503         | `{ "error": "PayU service unavailable" }`        |
| `timeout`    | —           | Delayed response or gateway timeout mapped to `502 Bad Gateway` when called from `PaymentController` |

### UC4: PaymentController delegates PayU to the mock

Given a checkout payment validation request with `payment_method: "payu-cz"`
When  `POST /payment/check` is handled
Then  the backend forwards the relevant payment data to the embedded mock PayU service (not the real PayU API)
And   maps the mock response back to the existing checkout contract: `200 { "message": "..." }` on success, or `422` with `{ "error": "..." }` on failure

### UC5: Checkout UI reads payment response and displays outcome

Given a CZ user is on the checkout payment step (step 4) with PayU selected
When  the user clicks **Confirm** and `POST /payment/check` returns `200 OK` with `{ "message": "..." }`
Then  the UI reads the `message` field from the response body
And   displays it in the green success alert (`data-test="payment-success-message"`)
And   clears any previously shown payment error message
And   sets `paymentConfirmed` so the button label changes to **Buy now**
And   disables the payment form fields

Given a CZ user is on the checkout payment step with PayU selected
When  the user clicks **Confirm** and `POST /payment/check` returns an error status (e.g. `422`) with `{ "error": "..." }`
Then  the UI reads the `error` field from the response body
And   displays it in the red error alert (`data-test="payment-error-message"`)
And   clears any previously shown payment success message
And   keeps the button label as **Confirm** (`paymentConfirmed` remains false)
And   leaves the payment form enabled so the user can retry

Given a CZ user is on the checkout payment step with PayU selected
When  payment validation fails with a non-JSON or unexpected error response
Then  the UI displays a fallback error message (e.g. `Unknown error`)
And   does not enable **Buy now**

### UC6: Non-PayU methods unchanged

Given a payment validation request for any method other than `payu-cz`
When  `POST /payment/check` is handled
Then  existing validation behaviour is unchanged

## Mock API contract

### Mock PayU service (backend internal)

| Field / endpoint                    | Direction          | Notes                                                      |
| ----------------------------------- | ------------------ | ---------------------------------------------------------- |
| `POST /mock/payu/orders`            | inbound            | Mock PayU order/payment creation                           |
| `amount`, `currency`, `order_id`    | request            | Minimum fields; `currency` expected `CZK` for CZ checkout  |
| `status`, `transaction_id`, `message` | response (success) |                                                          |
| `error`                             | response (failure) | Mapped by `PaymentController` to checkout response       |

### Checkout UI contract (`POST /payment/check`)

The payment step UI calls `POST /payment/check` and must act on the HTTP status and JSON body:

| HTTP status | Response body              | UI behaviour                                                                 |
| ----------- | -------------------------- | ---------------------------------------------------------------------------- |
| 200         | `{ "message": "..." }`     | Show success alert with `message`; enable **Buy now**                        |
| 4xx / 5xx   | `{ "error": "..." }`       | Show error alert with `error`; keep **Confirm**; allow retry                 |
| 4xx / 5xx   | (missing or invalid body)  | Show fallback error message; keep **Confirm**                                |

## Checkout flow (PayU)

```mermaid
sequenceDiagram
    participant UI as PaymentComponent
    participant Check as POST /payment/check
    participant PayU as Mock PayU Service
    participant Inv as POST /invoices

    UI->>Check: payment_method payu-cz
    Check->>PayU: POST /mock/payu/orders
    PayU-->>Check: 200 or error status
    Check-->>UI: 200 message or error status
    Note over UI: Read response; show success or error alert
    Note over UI: On success, Buy now enabled
    UI->>Inv: create invoice
```

## Affected files

**Backend (implementation targets):**

- `API/app/Http/Controllers/PaymentController.php` — add `payu-cz` branch
- New `PayuMockController` or mock routes in `API/routes/api.php`
- New `App\Services\Payu\PayuMockService` (or similar)
- `API/.env.example` — optional `PAYU_MOCK_DEFAULT_SCENARIO`

**Frontend (response handling must work for PayU mock scenarios):**

- `UI/src/app/checkout/payment/payment.component.ts` — read `message` / `error` from `/payment/check` response
- `UI/src/app/checkout/payment/payment.component.html` — success (`data-test="payment-success-message"`) and error (`data-test="payment-error-message"`) alerts

## Out of scope

- Real PayU SDK/API integration, redirect URLs, webhooks
- Frontend `PAYMENT_ENDPOINT` localStorage override changes
- Chat-widget checkout PayU support
- Invoice/PDF changes beyond existing `payu-cz` persistence
- Changes outside `practice-software-testing/sprint5-holtesting/`

# Alternatives:

# Errors:
