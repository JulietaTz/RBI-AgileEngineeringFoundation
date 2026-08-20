---
id: prd-toolshop-czech-market-entry
status: refined
created: 2026-08-13
updated: 2026-08-13
phase: plan-ready
author: "unknown"
---

# PRD: ToolShop Czech Market Entry — Localisation, Payments & Delivery

## Problem / Opportunity

After the planned China expansion was cancelled, ToolShop pivots to the Czech Republic as its next growth market. Currently the webshop has no Czech language support, no regional payment option (PayU is the dominant Czech payment gateway), and only a flat-rate delivery model with no Zásilkovna integration.

This combination prevents Czech customers from completing a natural purchase journey: they encounter an unfamiliar language, cannot pay with a trusted local method, and see shipping costs unrelated to their region or order weight. Commercial validation of the Czech market is blocked until these baseline capabilities exist.

## Value Propositions

### User Value

Czech customers can browse, understand, and purchase tools entirely in their language using PayU as a familiar local payment method and Zásilkovna as a preferred regional delivery option. Delivery pricing is transparent and proportional to destination and order weight rather than a fixed EU flat rate.

### Business Value

This release is the minimum viable foundation for the Czech market pilot. PayU and Zásilkovna are table-stakes for credibility with Czech buyers. Alza.cz marketplace integration provides an additional discovery and sales channel to validate commercial acceptance before scaling. The sprint generates the first real Czech transaction data needed for a go/no-go decision on broader Czech expansion.

### Technical Value

The i18n localisation architecture established here is reusable for future market expansions. The embedded mock PayU service decouples checkout testing from the real PayU API, improving test reliability and developer velocity. The weight-and-region-aware delivery pricing engine replaces a hardcoded flat rate, making shipping logic maintainable as new markets are added.

## Goals

1. Czech customers can complete the full purchase journey — browse, cart, checkout, payment, confirmation — entirely in Czech.
2. PayU CZK payment is available at checkout for customers with a Czech billing or shipping address.
3. Zásilkovna delivery appears for CZ and DACH shipping addresses; Standard delivery covers all regions with weight-tiered, region-specific pricing.
4. Czech-language product titles, descriptions, and specifications are available for all products in the Czech market scope.
5. ToolShop products are listed on Alza.cz as a commercial validation channel for the Czech market.
6. Developers and testers can exercise all PayU payment scenarios (success, decline, unavailability, timeout) via the embedded mock service and the in-app PayU TIP UI without calling the real PayU API.

## Constraints

- Sensitive payment information must not be stored in the webshop database (payment-security compliance requirement from US3100).
- PayU is shown only for customers whose billing or shipping address is in the Czech Republic.
- Currency display follows billing country: CZK when billing country = CZ, USD otherwise.
- The mock PayU service must be embedded in the Laravel API backend — no external stub server.
- Czech product content (translations) must be in scope before launch; the source and tooling for translations needs to be confirmed (see Open Decisions).

## Anti-Goals

- Security improvements (password hashing migration, xauth-X32 → X64 upgrade, ORMapper V1.2 → V2.0 upgrade) are explicitly out of scope — these are a separate epic.
- Guest-login-during-checkout continuity fix is not part of this PRD.
- PayU integration for non-Czech regions is not in scope.

## Selected Solution

### Approach: Centralized MarketContextService

A `MarketContextService` in Laravel — and a corresponding Angular service consuming it — provides the single source of truth for country-dependent feature availability. Given a billing or shipping country code, the service returns a market context DTO: available payment methods, available delivery options, display currency, and default language. All conditional features (PayU availability, Zásilkovna availability, CZK pricing) query this service rather than independently checking country codes.

**Scope:**

| | Details |
|---|---|
| **Reused** | Existing `PaymentController`, checkout address model, Angular checkout component and routing, Transloco i18n framework (already present), MariaDB product schema |
| **New work** | `App\Services\Market\MarketContextService` (Laravel), `MarketContextResource` (API DTO), Angular `MarketContextService`, `PayuMockController` + `PayuMockService`, `PaymentController` PayU branch, delivery pricing engine (weight + region matrix), Zásilkovna option in checkout delivery step, Transloco `cs-CZ.json` translation file, Czech product translations in DB seed, Alza.cz CSV feed generator, Angular PayU TIP page component |
| **Integration points** | `POST /payment/check`, checkout delivery options endpoint, Angular `CheckoutService`, Angular `PaymentComponent`, Laravel DB seeder |

**External schemas/contracts:** No Avro/Proto/OpenAPI external schemas are involved. The PayU CZ API is exercised via the embedded mock only; no external schema fetch is required for this sprint. The Alza.cz product feed format must be confirmed with Alza.cz seller support before the feed generator story begins.

**Rationale:**
- Centralising country-context logic eliminates duplicated `=== 'CZ'` guards across `PaymentController`, `DeliveryController`, and Angular templates — auditing Czech-specific behaviour becomes a single service inspection.
- A shared market DTO defined once ensures PayU visibility, Zásilkovna visibility, and CZK display all read from the same source of truth, preventing inconsistencies between payment and delivery conditional logic.
- The service accepts a country code as input, making a second market entry a new branch in the service config rather than a cross-codebase search.

**Estimated complexity:** L (one foundation story + six feature stories, approximately 12–15 files touched across backend and frontend)

---

## Business Acceptance Criteria

**BAC-1 — Czech Language Experience:**

Given a customer is on any page of the ToolShop,
When they select Czech (CS) from the language switcher,
Then all navigation elements, UI labels, buttons, headings, dialogs, error messages, and form validation messages switch to Czech.

Given a customer has Czech active and navigates to any other page,
When the page loads,
Then Czech remains the active language without requiring re-selection.

Given a customer submits a form with validation errors (registration, login, checkout, or profile update) while Czech is active,
Then all validation messages are displayed in Czech.

Given the checkout workflow is active with Czech selected,
When the customer proceeds through cart, delivery, payment, and confirmation steps,
Then all step labels, prompts, and confirmation messages are in Czech.

**BAC-2 — Czech Product Content:**

Given the webshop language is set to Czech,
When a customer views a product listing page,
Then Czech titles are displayed for all products that have Czech translations in the database; products without Czech translations display in the configured fallback language (English) without error.

Given the webshop language is set to Czech,
When a customer views a product detail page for a product with Czech content,
Then the product title, description, and technical specifications are all displayed in Czech.

Given the webshop language is set to Czech,
When a customer performs a product search,
Then search results display Czech product titles for products with available Czech content.

Given a customer with Czech active adds a product to cart and proceeds to checkout,
When the cart or checkout overview is displayed,
Then Czech product titles remain visible throughout the purchase journey.

Given Czech product content contains special characters (ě, š, č, ř, ž, ý, á, í, é),
When those characters are displayed in any product field,
Then they render correctly with no garbling or encoding artifacts.

**BAC-3 — PayU Payment for Czech Customers:**

Given a customer with a Czech billing OR Czech shipping address reaches the checkout payment step,
When payment options are shown,
Then PayU is listed as an available payment method.

Given a customer with a non-Czech billing AND non-Czech shipping address,
When payment options are shown,
Then PayU is NOT listed.

Given a Czech customer selects PayU and clicks Confirm,
When the payment step is submitted,
Then the customer is redirected to the PayU payment page.

Given the customer completes payment on the PayU page successfully,
When the payment result is received by the webshop,
Then a payment confirmation message is displayed and an order is created in the system.

Given the customer cancels or the PayU payment is declined,
When the payment result is received,
Then a clear error message is shown and the customer can retry without restarting checkout from the beginning.

Given any PayU payment interaction,
Then no credit card number, CVV, or payment credential is stored in the ToolShop database.

**BAC-4 — Regional Delivery Options and Pricing:**

Given a customer enters a Czech Republic (CZ) shipping address at the delivery step,
When delivery options are shown,
Then both Standard and Zásilkovna are available.

Given a customer enters a DACH shipping address (Germany, Austria, or Switzerland),
When delivery options are shown,
Then both Standard and Zásilkovna are available.

Given a customer enters an EU (non-DACH) shipping address,
When delivery options are shown,
Then only Standard delivery is available and Zásilkovna is not shown.

Given a customer enters a US or Other (non-EU, non-US) shipping address,
When delivery options are shown,
Then only Standard delivery is available.

Given the total cart weight is below 10 kg (Light tier),
When a delivery cost is displayed for any available method,
Then the price reflects the Light weight tier from the published pricing matrix for that method, region, and billing currency.

Given the total cart weight is 10 kg or more (Heavy tier),
When a delivery cost is displayed for any available method,
Then the price reflects the Heavy weight tier from the published pricing matrix for that method, region, and billing currency.

Given a customer's billing country is Czech Republic,
When any delivery cost is shown,
Then the amount is displayed in CZK (Kč).

Given a customer's billing country is not Czech Republic,
When any delivery cost is shown,
Then the amount is displayed in USD ($).

**BAC-5 — Alza.cz Marketplace Listing:**

Given the Czech market launch preparation,
When the product CSV/XLSX export is generated for the Czech market scope,
Then it contains all seeded Czech-market products with at minimum: Czech product title, Czech description, product category, price in CZK, and a product image reference.

Given the generated feed file is uploaded to the Alza.cz seller portal,
When a prospective Czech customer searches Alza.cz for tool categories covered by ToolShop,
Then at least one ToolShop product is discoverable in the search results.

**BAC-6 — PayU Testing Without Real API:**

Given `POST /mock/payu/orders` is called with header `X-PayU-Mock-Scenario: success`,
Then the endpoint returns HTTP 200 with body `{ "status": "SUCCESS", "transaction_id": "<generated>", "message": "Payment was successful" }` without calling the real PayU API.

Given `POST /mock/payu/orders` is called with header `X-PayU-Mock-Scenario: declined`,
Then the endpoint returns HTTP 422 with body `{ "error": "Payment declined" }`.

Given `POST /mock/payu/orders` is called with header `X-PayU-Mock-Scenario: unavailable`,
Then the endpoint returns HTTP 503 with body `{ "error": "PayU service unavailable" }`.

Given `POST /mock/payu/orders` is called with header `X-PayU-Mock-Scenario: timeout`,
Then the endpoint returns 502 Bad Gateway (or equivalent configured delay) without calling the real PayU API.

Given no `X-PayU-Mock-Scenario` header is sent and `POST /mock/payu/orders` is called with valid input,
Then the default response is success (HTTP 200 with SUCCESS body).

Given a tester is on the PayU TIP page (`/payu-tip`),
When they fill in amount, currency, order_id, select a scenario, and click Send request,
Then a modal appears showing the HTTP status code and the `message` or `error` field from the response body.

Given a tester changes the site language to Czech or any other language,
When they open the PayU TIP page,
Then all UI copy on the page and its response modal remains in English.

## Dependencies

### Upstream

| Dependency | Owner | Required Before |
|---|---|---|
| PayU Czech Republic live API credentials (merchant ID, API key, notification secret) | Business / PayU CZ partner onboarding | PayU production deployment |
| Alza.cz seller account registration and current product feed format specification | Business / Alza.cz partner onboarding | Alza feed generator story |
| Czech product translations for all seeded DB products | Development team (manual seeding) | Czech product content story |
| Confirmation that `weight` field (or equivalent `ProductSpec` record) exists for all seeded products | Backend team | Delivery engine story — **verify before implementation begins** |

### Downstream

No external consumers of the ToolShop API are affected. All new endpoints and behaviour are additive.

### Cross-Repo / Deployment

| Change | Location | What to add | Owner |
|---|---|---|---|
| Environment variables | `API/.env.example` | `PAYU_MOCK_DEFAULT_SCENARIO`, `PAYU_API_KEY`, `PAYU_MERCHANT_ID`, `PAYU_NOTIFICATION_SECRET` | Backend team |
| No Kubernetes / Helm / Terraform changes needed | — | All features ship within the existing Docker Compose stack | — |

## Risk Analysis

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| `weight` field absent from product records — delivery pricing cannot be calculated | H | H | Verify before implementation begins; if missing, add a DB migration for `weight` column and include in seeder data as part of the delivery engine story |
| PayU live API contract differs from mock (field names, redirect URL format, callback signature) | M | H | Validate mock against PayU CZ documentation before go-live; test in staging with PayU sandbox credentials |
| Czech special characters (ě, š, č, ř, ž, ý, á, í, é) corrupted in DB or UI | M | M | Confirm MariaDB charset is `utf8mb4`; add a character encoding acceptance test in the Czech content story |
| Alza.cz product feed format undocumented or subject to change | M | M | Contact Alza.cz seller support early to obtain the current feed spec before building the CSV generator |
| `MarketContextService` design too narrow to extend to a second market | L | M | Design service to accept country code as input and return a market DTO — avoid hardcoding `"CZ"` in service internals |
| Transloco `cs-CZ.json` translation file incomplete at launch (missing keys shown as raw key strings) | M | L | Automated test scans all Transloco keys used in app against the cs-CZ file and fails on missing keys |

## Open Decisions

- [x] **Alza.cz integration mechanism** — Decision: CSV/XLSX product feed generated from the database, uploaded manually to the Alza.cz seller portal. Appropriate for a pilot validation sprint; direct API integration deferred until commercial viability is confirmed.
- [x] **Czech product content scope and sourcing** — Decision: All products currently in the seeded database receive Czech translations, added manually via DB seed scripts by the development team.
- [x] **PayU redirect flow implementation** — Decision: `POST /payment/check` returns `{ "redirect_url": "..." }` (or direct mock result) as JSON to Angular. Angular `PaymentComponent` performs `window.location.href` redirect client-side. Server-side `302` would break the Angular SPA session state.
- [x] **Mock scenario selection mechanism** — Decision: `X-PayU-Mock-Scenario` request header, consistent across US9100 (mock service) and US9200 (TIP UI). Headers are not cached, not logged by default, and do not pollute URLs.
- [x] **CZK payment handling at PayU gateway** — Decision: PayU Czech Republic natively processes CZK transactions. The webshop passes CZK amounts; no webshop-level FX conversion is required. If a specific transaction type cannot be processed in CZK, PayU handles the fallback on their side.

## References

- [Sprint 6 Agile Vision Statement](../../../toolshop-v6/sprint6-agile-vision-statement.md)
- [US2300 — Czech Language Support](../../../toolshop-v6/US2300-support-czech-language.md)
- [US2350 — Czech Product Content](../../../toolshop-v6/US2350-czech-products.md)
- [US3100 — PayU Payment Integration](../../../toolshop-v6/US3100-support-payment-type-PayU.md)
- [US4200 — Delivery Costs and Options](../../../toolshop-v6/US4200-delivery-costs.md)
- [US9100 — Mock PayU Payment Service](../../../toolshop-v6/US9100-Mock-PayU-Payment-Service.md)
- [US9200 — PayU TIP Testing UI](../../../toolshop-v6/US9200-PayU-TIP-Testing-UI.md)
- [Architecture Overview](../../../toolshop-v6/architecture.md)
- [Feature Matrix](../../../toolshop-v6/features.md)

## Consistency Notes

- **Security items excluded**: The Sprint 6 vision bundles security improvements (password hashing, xauth-X32 → X64, ORMapper V1.2 → V2.0) alongside Czech market work. These are independent concerns and should be a separate PRD. ISEC-2499 (xauth vulnerability) should be tracked in that PRD.
- **Guest checkout**: The vision mentions fixing guest login during checkout without restarting the purchase journey. No dedicated story was provided and the user confirmed this is out of scope for this PRD. Consider a separate story or PRD if this is needed before Czech launch.
- **Alza.cz story gap**: The vision positions Alza.cz as a key validation channel, but no implementation story was provided in the source material. This PRD includes BAC-5 and flags it as an open decision. A story will need to be created during `refine-prd`.
- **US9100/US9200 dependency**: US9200 (PayU TIP UI) depends on US9100 (mock service) and should be sequenced after it. US3100 also depends on US9100 being available in the test environment before checkout PayU testing can proceed.
