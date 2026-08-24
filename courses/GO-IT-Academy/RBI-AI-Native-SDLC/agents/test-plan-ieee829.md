# Test Plan — ToolShop Czech Market Entry — Sprint 6

Standard: IEEE 829
Date: 2026-08-12
Version: 1.0
Status: Draft

---

## 1. Test Plan Identifier

TP-plan-toolshop-czech-market-sprint6-IEEE829-v1.0

---

## 2. Introduction

This test plan covers all testing activities for the ToolShop Czech Market Entry initiative, Sprint 6 (HOLTES). The system under test is the ToolShop e-commerce application, a self-developed platform consisting of an Angular 17 single-page frontend and a Laravel (PHP) backend API. Sprint 6 introduces seven independently deployable stories: Czech language localisation, Czech product content, PayU CZ payment integration, weight-based delivery pricing, an Alza.cz XML product feed, a Mock PayU service, and a PayU TIP testing UI.

The purpose of testing is to verify that all 27 Business Acceptance Criteria (BAC-1 through BAC-27) defined in the sprint plan are met with no functional, security, or reliability deviations before stories are promoted to production.

Source plan: `.sdlc/prds/toolshop-czech-market-sprint6/plan.md`  
Source test design: `.sdlc/prds/toolshop-czech-market-sprint6/test-design.md`

---

## 3. Test Items

| Item ID | Component | Story | Version / Branch |
|---|---|---|---|
| TI-1 | `LanguageSwitcherComponent` + `CookieLanguageService` | Story 1: Czech Language & Localisation | `czech-language-localisation` |
| TI-2 | Czech translation file (`assets/i18n/cs.json`) + English baseline (`assets/i18n/en.json`) | Story 1 | `czech-language-localisation` |
| TI-3 | Laravel `SetLocale` middleware + `Kernel.php` registration | Story 1 | `czech-language-localisation` |
| TI-4 | `Product` model (`localise()` helper), `ProductRepository`, `ProductController`, `SearchController`, `CartController` | Story 2: Czech Product Content | `czech-product-content` |
| TI-5 | Migration `add_czech_fields_to_products_table` | Story 2 | `czech-product-content` |
| TI-6 | `PayUClient`, `PaymentController` (payu-cz branch), `PayUWebhookController`, webhook route (`POST /payment/payu/callback`) | Story 3: PayU Payment Integration | `payu-payment-integration` |
| TI-7 | `CheckoutPaymentComponent` (country guard + PayU radio option) | Story 3 | `payu-payment-integration` |
| TI-8 | `DeliveryPricingService`, `DeliveryController`, checkout delivery step component | Story 4: Delivery Pricing | `delivery-pricing` |
| TI-9 | Migration `add_weight_to_products_table`; `Product` model (`weight` attribute) | Story 4 | `delivery-pricing` |
| TI-10 | `AlzaFeedGenerator`, `AlzaFeedController`, `AlzaFeedTokenGuard`, scheduler entry, `resources/views/feed/alza.blade.php` | Story 5: Alza.cz XML Product Feed | `alza-xml-product-feed` |
| TI-11 | `MockPayUController`, `APP_ENV` route guard, `routes/api.php` (mock route) | Story 6: Mock PayU Service | `mock-payu-service` |
| TI-12 | `CheckoutPaymentComponent` (success/error alert display) | Story 6 | `mock-payu-service` |
| TI-13 | `PayuTipComponent` (template + class + spec), `/payu-tip` route, `data-test="nav-payu-tip"` header link | Story 7: PayU TIP Testing UI | `payu-tip-testing-ui` |

---

## 4. Features to Be Tested

| Feature ID | BAC | Story | Description |
|---|---|---|---|
| FT-1 | BAC-1 | Story 1 | Language switcher control visible and interactive on all customer-facing pages |
| FT-2 | BAC-2 | Story 1 | Selecting Czech activates translated UI text immediately without page reload; in-progress state (cart, filters) preserved |
| FT-3 | BAC-3 | Story 1 | Language preference written to persistent `lang` cookie on selection and restored on subsequent visits including after full browser restart |
| FT-4 | BAC-4 | Story 1 | All checkout step labels, form field labels, buttons, and confirmation copy rendered in Czech when locale is `cs` |
| FT-5 | BAC-5 | Story 1 | All form validation error messages (required, format, length, mismatch) rendered in Czech when locale is `cs` |
| FT-6 | BAC-6 | Story 2 | Czech product title, description, and specifications displayed on listing and detail pages when locale is `cs` and Czech fields are populated |
| FT-7 | BAC-7 | Story 2 | Products without Czech fields fall back to English title and description — no blank or broken content |
| FT-8 | BAC-8 | Story 2 | Search results show Czech titles for translated products when locale is `cs` |
| FT-9 | BAC-9 | Story 2 | Czech product titles visible in the cart and checkout order summary when locale is `cs` |
| FT-10 | BAC-10 | Story 3 | PayU payment option displayed when billing address country = CZ or shipping address country = CZ (either condition is sufficient) |
| FT-11 | BAC-11 | Story 3 | PayU payment option absent when both billing and shipping address countries are non-CZ |
| FT-12 | BAC-12 | Story 3 | Successful PayU gateway response creates an order with `payment_confirmed` status |
| FT-13 | BAC-13 | Story 3 | Declined or cancelled PayU payment shows error, does not create an order, and allows the customer to retry without restarting checkout |
| FT-14 | BAC-14 | Story 3 | No card number, CVV, or raw gateway credentials stored in any database record or log entry after a PayU payment cycle |
| FT-15 | BAC-15 | Story 4 | Standard delivery option available for CZ, DACH, EU, US, and Others shipping regions with prices matching the US4200 matrix for all 8 region × weight combinations |
| FT-16 | BAC-16 | Story 4 | Zásilkovna delivery option available for CZ and DACH shipping addresses |
| FT-17 | BAC-17 | Story 4 | Zásilkovna delivery option absent for EU (non-DACH) and US shipping addresses |
| FT-18 | BAC-18 | Story 4 | Light tier (< 10 kg) and Heavy tier (≥ 10 kg) applied correctly; boundary at 10.00 kg |
| FT-19 | BAC-19 | Story 4 | Delivery price returned in CZK for Czech billing country; USD for non-Czech billing country |
| FT-20 | BAC-20 | Story 5 | `GET /feed/alza` returns HTTP 200 with valid XML and `Content-Type: application/xml` for authenticated requests; HTTP 401 for missing or invalid bearer token; XML contains Czech-translated products |
| FT-21 | BAC-21 | Story 5 | Product price and stock level changes reflected in the Alza feed after the next scheduled export run |
| FT-22 | BAC-22 | Story 6 | `POST /mock/payu/orders` with no `X-PayU-Mock-Scenario` header returns HTTP 200 with `status=SUCCESS` and a `transaction_id` |
| FT-23 | BAC-23 | Story 6 | Scenario header `declined` → HTTP 422; `unavailable` → HTTP 503; `timeout` → HTTP 502 |
| FT-24 | BAC-24 | Story 6 | `CheckoutPaymentComponent` renders `message` in green `data-test="payment-success-message"` on HTTP 200; `error` in red `data-test="payment-error-message"` on HTTP 422; "Unknown error" fallback for non-JSON responses |
| FT-25 | BAC-25 | Story 6 | `POST /mock/payu/orders` route not registered when `APP_ENV=production`; returns HTTP 404 |
| FT-26 | BAC-26 | Story 7 | `data-test="nav-payu-tip"` link present in header with text "PayU TIP"; link navigates to `/payu-tip` route |
| FT-27 | BAC-27 | Story 7 | TIP form POSTs to `/mock/payu/orders` with form values and optional `X-PayU-Mock-Scenario` header; response modal shows HTTP status and `message`/`error` field; "Request failed" for network errors |

---

## 5. Features Not to Be Tested

| Feature | Reason Excluded |
|---|---|
| Admin portal | Out of sprint scope; no sprint 6 changes made to admin surfaces |
| Order fulfilment backend | No changes in sprint 6; assumed to be functioning correctly |
| Mobile / native clients | ToolShop is web-only; no mobile client exists |
| PayU hosted payment page internals | Third-party responsibility; ToolShop only tests the integration contract |
| PayU HMAC signature algorithm correctness | PayU library responsibility; ToolShop tests that the check is invoked and blocks writes on failure |
| Alza.cz XML schema validation on the Alza side | Alza's parser is out of scope; ToolShop validates it produces well-formed XML matching the schema |
| Translation of PayU TIP page | English-only by design — no translation keys are defined for this page |
| Existing (non-Czech) payment methods | Not modified in sprint 6; regression is covered by existing test suite |
| In-flight orders at Story 4 deployment | Retain original flat-rate delivery cost by design; no test required |
| Transloco library internals | Standard library; tested only at the integration point (locale switching behaviour) |

---

## 6. Approach

**Test levels and tools:**

| Level | Tool | Stories |
|---|---|---|
| Unit — Angular | Jasmine / Karma | 1, 6, 7 |
| Unit — Laravel | PHPUnit | 2, 3, 4, 5, 6 |
| Integration — Laravel | PHPUnit feature tests | 3, 5 |
| End-to-end | Playwright (Chromium) | 3 |

**Test design techniques:**

- **Equivalence Partitioning:** locale (`cs` / `en`); billing/shipping country (CZ / non-CZ); `APP_ENV` (`production` / `testing`); Zásilkovna eligibility (CZ+DACH / other); mock PayU scenario (none / `declined` / `unavailable` / `timeout`).
- **Boundary Value Analysis:** weight tier boundary at 10.00 kg (9.99 kg → Light, 10.00 kg → Heavy); empty vs. non-empty Czech product fields for fallback logic.
- **Use Case Testing:** end-to-end locale switching flow; full checkout with PayU (select → confirm → success/decline/cancel); cart through checkout with Czech product content; TIP form submission and modal display.
- **State Transition Testing:** language switcher (EN → CS → EN); payment state machine (initiated → confirmed / declined / cancelled → retry).
- **Error Guessing:** missing `lang` cookie on init; null and empty-string Czech product fields; PayU webhook with invalid HMAC signature; Alza feed endpoint without or with expired bearer token; non-JSON mock PayU response body.

Test situations are documented in `test-design.md` (57 situations across 27 BACs). Each situation is mapped to a named, automated test. All unit and integration tests run automatically in CI on every PR merge. The single Playwright e2e situation (TS-13.2 — cancelled PayU checkout) runs on demand after Story 3 merge and requires PayU CZ sandbox access.

---

## 7. Item Pass/Fail Criteria

**Pass:** All assertions within a test situation produce the expected outcome as specified in `test-design.md`. No unexpected exceptions are thrown. The test situation's BAC statement is satisfied with no deviation.

**Fail:** Any of the following:
- An assertion deviates from the expected outcome documented in the test situation.
- An unexpected exception or error is thrown during execution.
- A feature does not satisfy the BAC statement it was derived from.
- Sensitive data (card number, CVV, raw credentials) is found in any database record or log (FT-14, FT-25 — automatic BLOCKER).
- The mock PayU route returns a non-404 response under `APP_ENV=production` (FT-25 — automatic BLOCKER).

---

## 8. Suspension Criteria and Resumption Requirements

**Suspension criteria:**
- More than 30% of test situations within a story are BLOCKED by a build error, unresolved dependency, or unavailable environment.
- A BLOCKER defect is open against any security-critical BAC (BAC-14: sensitive data storage; BAC-25: mock route in production).
- The CI pipeline is broken for more than one working day (non-story-specific build failure).
- Story 5 implementation is attempted before the Alza.cz XML schema is obtained (explicit plan blocker).

**Resumption requirements:**
- The blocking defect is resolved and a clean CI build is available.
- For Story 5: the Alza.cz schema is obtained and the Field Mapping Table in `plan.md` is completed.
- For Story 3 integration tests: PayU CZ sandbox credentials are provisioned and validated in the `.env` of the integration environment.
- A new test execution cycle begins from the first situation affected by the suspension.

---

## 9. Test Deliverables

| Deliverable | Description | Produced By |
|---|---|---|
| Test plan (this document) | IEEE 829 sprint test plan | QA Engineer |
| ISO/IEC/IEEE 29119-3 test plan | Complementary standards-conformant plan | QA Engineer |
| ITPF test plan | Internal RBI ITPF-format plan | QA Engineer |
| `test-design.md` | 57 test situations across 27 BACs | QA Engineer |
| Automated unit + integration test code | PHPUnit and Jasmine specs per story | Developer |
| Playwright e2e spec | Story 3 cancel/retry flow (TS-13.2) | Developer / QA |
| Per-story test reports | Pass/fail summary and BAC coverage status per story | QA Engineer |
| Sprint Test Summary Report | Final report confirming BAC Coverage Matrix fully green | QA Engineer |
| Defect log | All defects raised during execution, linked to BAC and story | QA Engineer |

---

## 10. Testing Tasks

| # | Task | Story | Depends On | Responsibility |
|---|---|---|---|---|
| 1 | Review and approve test plan | All | — | QA Engineer, Product Owner |
| 2 | Validate Transloco integration (technical spike) | Story 1 | 1 | Developer |
| 3 | Implement and execute Story 1 unit tests (Angular) | Story 1 | 2 | Developer |
| 4 | Implement and execute Story 6 unit tests (Laravel + Angular) | Story 6 | 1 | Developer |
| 5 | Review Story 1 & 6 test results; update BAC Coverage Matrix | Stories 1, 6 | 3, 4 | QA Engineer |
| 6 | Implement and execute Story 2 unit tests (Laravel) | Story 2 | 3 (Story 1 merged) | Developer |
| 7 | Review Story 2 test results; update BAC Coverage Matrix | Story 2 | 6 | QA Engineer |
| 8 | Implement and execute Story 4 unit tests (Laravel) | Story 4 | 6 (Story 2 merged) | Developer |
| 9 | Implement and execute Story 5 unit + integration tests (Laravel) | Story 5 | 6 + Alza schema obtained | Developer |
| 10 | Implement and execute Story 3 unit tests (Laravel + Angular) | Story 3 | 4 (Story 6 merged) | Developer |
| 11 | Execute Story 3 integration tests with PayU CZ sandbox | Story 3 | 10 + PayU sandbox provisioned | Developer / QA |
| 12 | Execute Story 3 Playwright e2e (TS-13.2 cancel/retry) | Story 3 | 11 | QA Engineer |
| 13 | Implement and execute Story 7 unit tests (Angular) | Story 7 | 3, 4 (Stories 1 & 6 merged) | Developer |
| 14 | Review Stories 3, 4, 5, 7 test results; update BAC Coverage Matrix | Stories 3–7 | 8–13 | QA Engineer |
| 15 | Produce Sprint Test Summary Report | All | 14 | QA Engineer |
| 16 | Product Owner sign-off on Test Summary Report | All | 15 | Product Owner |

---

## 11. Environmental Needs

| Attribute | Value |
|---|---|
| Backend runtime | PHP (Laravel) |
| Backend test framework | PHPUnit |
| Frontend runtime | Angular 17 |
| Frontend test framework | Jasmine / Karma |
| E2E browser | Chromium via Playwright |
| `APP_ENV` | `testing` for all automated test runs |
| Mock PayU routes | Active when `APP_ENV != production` |
| PayU CZ sandbox | Required for Story 3 integration tests; credentials stored in `.env` only (never committed) |
| `ALZA_FEED_TOKEN` | Required for Story 5 feed endpoint tests; stored in `.env` only |
| Database | Test database seeded per-test via Laravel database factories; transactions rolled back after each test |
| CI pipeline | Executes unit + integration suites on every PR merge; Playwright suite on demand |
| `APP_ENV=production` simulation | PHPUnit `env` override for TS-25.1 (verifies mock route absent) |

---

## 12. Responsibilities

| Role | Responsibility |
|---|---|
| Developer | Implement unit and integration tests as part of story Definition of Done; maintain CI pipeline; ensure all named test situations pass before raising PR |
| QA Engineer | Review test situations and BAC coverage; execute e2e tests; raise and triage defects; produce per-story and sprint test reports; maintain BAC Coverage Matrix |
| Product Owner | Review BAC coverage matrix per story; approve test plan (Section 16); sign off Sprint Test Summary Report |
| DevOps / Platform Team | Set `APP_ENV=production` in Helm chart / `.env` before Story 6 production deployment; provide CI environment with required test toolchains |
| Business / Partnerships | Procure Alza.cz XML schema and partner portal access; provision PayU CZ sandbox credentials |

---

## 13. Staffing and Training Needs

| Need | Story | Action |
|---|---|---|
| Transloco locale-change lifecycle (Angular) | Story 1 | Technical spike recommended at kick-off to confirm `translocoService.setActiveLang()` behaviour before full implementation. Estimated: 0.5 days. |
| PayU CZ REST API and HMAC webhook verification | Story 3 | Developer must review PayU Czech Republic Developer Docs (`https://developers.payu.com/en/`) before writing `PayUClient`. Estimated: 0.5 days. |
| Playwright e2e setup | Story 3 | If the project has not previously used Playwright, initial setup (install, Chromium download, first spec scaffold) should be scheduled as a task before Story 3 e2e execution. Estimated: 0.5 days. |
| Alza.cz XML feed schema | Story 5 | Developer must review the Alza.cz partner portal schema specification before writing `AlzaFeedGenerator`. Blocked on schema procurement (see Section 15). |

---

## 14. Schedule

| Milestone | Entry Condition | Target |
|---|---|---|
| Test plan approved | — | Before sprint implementation begins |
| Stories 1 & 6 unit tests complete | Stories 1 & 6 merged to main | After sprint kick-off parallel phase |
| Story 2 unit tests complete | Story 2 merged | After Story 1 completion |
| Story 4 unit tests complete | Story 4 merged | After Story 2 completion (parallel with Story 5) |
| Story 5 unit + integration tests complete | Story 5 merged; Alza schema obtained | After Story 2 completion (parallel with Story 4) |
| Story 3 unit tests complete | Story 3 merged | After Story 6 completion |
| Story 3 integration + e2e tests complete | PayU sandbox credentials provisioned; staging available | After Story 3 unit tests pass |
| Story 7 unit tests complete | Story 7 merged | After Stories 1 & 6 completion |
| All unit & integration tests green in CI | All 7 stories merged | Sprint closure gate |
| Sprint Test Summary Report approved | Product Owner sign-off; BAC Coverage Matrix fully green; 0 BLOCKER defects | Before production deployment |

---

## 15. Risks and Contingencies

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| Alza.cz XML schema not obtained during sprint | High | High | Block Story 5 `specify-task` until schema is received; assign procurement to Business/Partnerships at sprint kick-off; Story 5 may be deferred to a post-sprint release without blocking the other six stories |
| PayU CZ sandbox credentials not provisioned before Story 3 integration tests | Medium | High | Raise credential request at sprint kick-off; Story 3 unit tests (PHP + Angular) proceed without sandbox; integration and e2e tests are explicitly gated on credential availability |
| `APP_ENV=production` not set in production deployment (mock routes exposed) | Low | Critical | DevOps to verify `APP_ENV` in Helm chart before any Story 6 production deployment; TS-25.1 verifies this independently in CI; classified as BLOCKER if failing |
| Merge conflict on `Product.php` between Stories 2 and 4 | Medium | Medium | Enforce branch sequencing: Story 4 branch is cut from Story 2 merged; CI branch protection enforces dependency order |
| Merge conflict on `AppComponent`/`HeaderComponent` between Stories 1 and 7 | Low | Low | Story 7 branch is cut after Story 1 merges; conflict surface is limited to a single nav link addition |
| Transloco locale-change API differs from expected behaviour | Low | High | Conduct technical spike before Story 1 full implementation; spike result documented and shared with team |
| Card or credential data inadvertently logged by a third-party middleware | Low | Critical | TS-14.2 explicitly inspects log output after a PayU payment cycle; classified as automatic BLOCKER if any credentials found |

---

## 16. Approvals

| Role | Name | Signature | Date |
|---|---|---|---|
| Test Manager / QA Engineer | | | |
| Project Lead / Tech Lead | | | |
| Product Owner | | | |
