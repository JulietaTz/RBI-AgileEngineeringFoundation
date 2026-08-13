# Test Plan — ToolShop Czech Market Entry — Sprint 6

## Change Log

| Date | Version | Status | Document Owner | Changed by | Change Reason | Comment | Reviewer | Approved by | Approval Date |
|---|---|---|---|---|---|---|---|---|---|
| 2026-08-12 | 1.0 | Draft | QA Engineer | GitHub Copilot | Initial version generated from plan.md | | | | |

---

## 1 Introduction

### 1.1 Summary (ITPF mandatory)

ToolShop is a self-developed, Angular 17 (frontend) + Laravel / PHP (backend) e-commerce webshop operated by RBI. Sprint 6 (HOLTES) delivers the Czech Market Entry initiative across seven stories:

- **Story 1** — Czech language & localisation (Angular + Laravel locale middleware)
- **Story 2** — Czech product content (DB columns, `Product::localise()` helper)
- **Story 3** — PayU Czech Republic payment integration (real gateway, HMAC webhook)
- **Story 4** — Weight-based delivery pricing (region × weight matrix, Zásilkovna option)
- **Story 5** — Alza.cz XML product feed (authenticated, scheduled export)
- **Story 6** — Mock PayU service (scenario-driven stubs, `APP_ENV` guard)
- **Story 7** — PayU TIP testing UI (English-only internal testing page)

The backend is PHP / Laravel, tested with PHPUnit. The frontend is Angular 17 with Transloco for i18n, tested with Jasmine / Karma. End-to-end flows are covered by Playwright (Chromium).

Source plan: `.sdlc/prds/toolshop-czech-market-sprint6/plan.md`  
Source test design: `.sdlc/prds/toolshop-czech-market-sprint6/test-design.md`

### 1.2 Timeline (ITPF not mandatory)

| Milestone | Date | Comment |
|---|---|---|
| Sprint kick-off / Test plan approved | 2026-08-12 | Stories 1 & 6 start in parallel |
| Stories 1 & 6 merged | TBD | Unlocks Stories 2, 3, 7 |
| Story 2 merged | TBD | Unlocks Stories 4 & 5 (parallel) |
| Stories 3, 4, 5, 7 merged | TBD | All stories on critical path complete |
| Test closure / Release gate | TBD | All 27 BACs green, 0 BLOCKER defects |

> ⚠️ Story 5 (Alza.cz Feed) is blocked externally until the Alza.cz partner portal XML schema is obtained. Story 3 integration tests require PayU CZ sandbox credentials to be provisioned.

### 1.3 Related Documents (ITPF not mandatory)

| Document | Link |
|---|---|
| Sprint 6 Plan | `.sdlc/prds/toolshop-czech-market-sprint6/plan.md` |
| Test Design (test situations catalogue) | `.sdlc/prds/toolshop-czech-market-sprint6/test-design.md` |
| ISO/IEC/IEEE 29119-3 Test Plan | `.sdlc/prds/toolshop-czech-market-sprint6/test-plan.md` |
| Delivery Pricing Matrix | `toolshop-v6/US4200-delivery-costs.md` |
| PayU Czech Republic Developer Docs | https://developers.payu.com/en/ |
| Alza.cz Partner Portal | https://www.alza.cz/partner |
| ITPF Test Plan Template | `plugins/rbi-sdlc-fundamentals/templates/Test-Plan-ITPF-Template.md` |

---

## 2.0 Test Approach (all ITPF mandatory)

### 2.1 Test Basis

#### 2.1.1 Test Assumptions

- **Self-developed software**  
  The ToolShop application is self-developed. All components introduced in Sprint 6 are new code written by the development team and require full functional testing at unit, integration, and end-to-end levels.

- **Product Dependencies**  
  The Angular frontend depends on `@freya/gds-web` for UI components and `@jsverse/transloco` for localisation. Both are assumed to be already functioning correctly. Tests for Story 1 validate only the integration of Transloco (locale switching, cookie persistence) rather than Transloco internals.  
  The Laravel backend depends on the existing `products` table schema. Stories 2 and 4 extend it via migrations; story sequencing enforces that Story 4 runs after Story 2 to prevent migration conflicts.

- **Third-party services**  
  PayU Czech Republic is a third-party payment gateway. Its hosted payment page and HMAC verification mechanism are assumed to function correctly. Testing focuses on ToolShop's integration contract (request construction, response mapping, webhook handling, data isolation). Integration tests use the Mock PayU service (Story 6) or the PayU CZ sandbox (Story 3).  
  Alza.cz is an external marketplace. The correctness of Alza.cz's schema parsing is out of scope; testing validates that ToolShop produces well-formed XML meeting the schema and that authentication works.

#### 2.1.2 Requirements

Requirements for this sprint are defined as Business Acceptance Criteria (BAC-1 through BAC-27) in `plan.md`. Each story block lists the BACs it satisfies and the test names that cover them. The BAC Coverage Matrix in `test-design.md` confirms that all 27 BACs are covered by at least one test situation (57 situations total).

#### 2.1.3 Solution Design and Architecture

ToolShop follows a two-tier architecture:

- **Frontend (Angular 17 SPA):** `LanguageSwitcherComponent`, `CookieLanguageService`, `CheckoutPaymentComponent`, `PayuTipComponent`, Angular routing module, Transloco i18n, Cookie service.
- **Backend (Laravel / PHP):** `Product` model + `localise()` helper, `ProductRepository`, `ProductController`, `SearchController`, `CartController`, `PayUClient`, `PaymentController`, `PayUWebhookController`, `DeliveryPricingService`, `DeliveryController`, `AlzaFeedGenerator`, `AlzaFeedController`, `MockPayUController`, `SetLocale` middleware, Laravel scheduler.
- **Database:** `products` table extended with `title_cs`, `description_cs`, `specs_cs` (Story 2) and `weight` (Story 4).

---

### 2.2 Test Objects

| ID | Name | Abbreviation | Story | Description |
|---|---|---|---|---|
| TO-1 | `LanguageSwitcherComponent` | LSC | Story 1 | Angular component rendering the locale switcher in the shared header; writes language selection to cookie via `CookieLanguageService` |
| TO-2 | `CookieLanguageService` | CLS | Story 1 | Angular service reading and writing the `lang` cookie; initialises Transloco active locale on app start |
| TO-3 | Czech translation file (`assets/i18n/cs.json`) | CS-JSON | Story 1 | Translation key file covering all UI strings; correctness of checkout and validation message keys |
| TO-4 | Laravel `SetLocale` middleware | SLM | Story 1 | Reads `lang` cookie on every request and sets the Laravel application locale |
| TO-5 | `Product` model (`localise()` helper) | PROD-L | Story 2 | PHP model method returning Czech content when `locale=cs` and English fallback when Czech fields are null/empty |
| TO-6 | `ProductController` / `ProductRepository` | PROD-CTRL | Story 2 | Passes resolved locale to product queries for listing and detail endpoints |
| TO-7 | `SearchController` | SRCH | Story 2 | Returns Czech product titles in search results when locale is `cs` |
| TO-8 | `CartController` (checkout overview) | CART | Story 2 | Returns Czech product titles in cart and checkout order summary |
| TO-9 | `PayUClient` | PAYU-C | Story 3 | PHP service wrapping the PayU CZ REST API; constructs order creation requests; handles redirect URL |
| TO-10 | `PaymentController` (payu-cz branch) | PAY-CTRL | Story 3 | Dispatches to `PayUClient` for `payment_method=payu-cz`; maps gateway response to checkout contract; enforces address country guard |
| TO-11 | `PayUWebhookController` | WEBHOOK | Story 3 | Receives PayU callback; verifies HMAC signature before writing order status |
| TO-12 | `CheckoutPaymentComponent` (country guard + PayU option) | CPC | Story 3 | Angular checkout step rendering PayU option only for CZ billing or shipping address |
| TO-13 | `DeliveryPricingService` | DPS | Story 4 | PHP service resolving shipping region, weight tier, currency, and available options (including Zásilkovna) per the US4200 matrix |
| TO-14 | `Product` model (weight attribute) | PROD-W | Story 4 | `weight` column added to `products` table; used by `DeliveryPricingService` to calculate cart total weight |
| TO-15 | `AlzaFeedGenerator` | AFG | Story 5 | PHP service producing Alza.cz-compliant XML from Czech-translated products |
| TO-16 | `AlzaFeedController` + bearer token guard | AFC | Story 5 | `GET /feed/alza` endpoint; validates `ALZA_FEED_TOKEN` before serving the cached XML file |
| TO-17 | `MockPayUController` + `APP_ENV` route guard | MOCK-PAYU | Story 6 | Scenario-driven mock endpoint; routes registered only when `APP_ENV != production` |
| TO-18 | `CheckoutPaymentComponent` (success/error display) | CPC-UI | Story 6 | Renders `payment-success-message` / `payment-error-message` alerts from mock PayU response body |
| TO-19 | `PayuTipComponent` | TIP | Story 7 | Angular form sending POST to `/mock/payu/orders` with `X-PayU-Mock-Scenario` header; displays response modal |
| TO-20 | Angular routing + header nav link | ROUTING | Story 7 | `data-test="nav-payu-tip"` link in header; `/payu-tip` route registration |

---

### 2.3 Test Strategy

#### 2.3.1 Test Levels / Approach / Test Types

| Used Test Levels | Applicable? | Comment | Used Test Approaches | Applicable? | Comment | Used Test Types | Applicable? | Comment |
|---|---|---|---|---|---|---|---|---|
| Component Testing (CT) | YES | PHPUnit unit, Jasmine unit | Requirements-Based Testing | YES | All 27 BACs map to named test situations | Functional Testing | YES | Primary test type for all 7 stories |
| System Testing (ST) | YES | Playwright e2e — Story 3 cancel/retry flow | Design-Based Testing | NO | | Non-Functional Testing | YES | Security (BAC-14, BAC-25), reliability (BAC-13) |
| System Integration Testing (SIT) | YES | Laravel integration tests — Stories 3, 5 | Risk-Based Testing | YES | Product and project risk register drives priority | Re-Testing | YES | After defect fixes |
| User Acceptance Testing (UAT) | NO | Developer-led sprint; PO reviews BAC coverage matrix only | Use Case-based Testing | YES | End-to-end checkout, locale switching, TIP form | Regression Testing | YES | CI pipeline on every PR merge |
| Business Process-Based Testing | NO | | Exploratory-Based Testing | YES | Supplementary exploratory session recommended after Story 3 merge | | | |

#### 2.3.2 Test Objectives (According to ISO-9126)

| Attribute | Sub-Attribute | Validation Needed? | Test Object(s) | What / How / Where | Test Levels | CT | ST | SIT | UAT |
|---|---|---|---|---|---|---|---|---|---|
| **Functionality** | Suitability | MUST | TO-1 – TO-20 (all) | 57 test situations covering all 27 BACs; executed per story sequencing defined in test-design.md | Y — see test-design.md | Y | Y | Y | — |
| **Functionality** | Accuracy | MUST | TO-13 (DPS), TO-5 (PROD-L) | Delivery prices match US4200 matrix for all 8 region × weight combinations (TS-15.4); Czech content resolved to correct DB field (TS-6.1, TS-6.2) | Y | Y | — | — | — |
| **Functionality** | Security | MUST | TO-11 (WEBHOOK), TO-16 (AFC), TO-17 (MOCK-PAYU) | HMAC signature verified before webhook writes order status; `GET /feed/alza` returns 401 without valid bearer token; `/mock/payu/orders` returns 404 in production (TS-14.1, TS-14.2, TS-20.3, TS-25.1) | Y | Y | — | Y | — |
| **Functionality** | Interoperability | MUST | TO-9 (PAYU-C), TO-15 (AFG) | PayU CZ REST API integration verified against sandbox; Alza.cz XML validated against schema (TS-12.1, TS-20.1–20.2) | Y | — | Y | — | — |
| **Reliability** | Fault Tolerance | MUST | TO-10 (PAY-CTRL), TO-18 (CPC-UI) | Failed or cancelled PayU payment does not create order; non-JSON response body handled gracefully with "Unknown error" fallback (TS-13.1, TS-13.2, TS-24.3) | Y | Y | Y | — | — |
| **Reliability** | Recovery | SHOULD | TO-10 (PAY-CTRL), TO-18 (CPC-UI) | Customer can retry PayU payment without restarting checkout after decline or cancellation (TS-13.1, TS-13.2) | Y | — | — | Y | — |
| **Usability** | Operability | MUST | TO-1 (LSC), TO-5 (PROD-L) | Language switcher visible on all customer-facing pages without scrolling (TS-1.1); untranslated products display English content, not blank (TS-7.1, TS-7.2) | Y | Y | — | — | — |
| **Usability** | Understandability | SHOULD | TO-18 (CPC-UI), TO-19 (TIP) | Payment success/error messages clearly visible in labelled alert elements; TIP response modal shows HTTP status code and body field (TS-24.1, TS-24.2, TS-27.2, TS-27.3) | Y | Y | — | — | — |
| **Maintainability** | Testability | SHOULD | TO-17 (MOCK-PAYU) | `X-PayU-Mock-Scenario` header enables deterministic scenario testing without external dependency; `APP_ENV` guard allows environment-specific route registration (TS-22.1, TS-23.1–23.3) | Y | Y | — | — | — |
| **Portability** | Adaptability | MUST | TO-2 (CLS), TO-17 (MOCK-PAYU) | Language preference persists via cookie across browser sessions including full restart (TS-3.3); mock routes absent in production environment (TS-25.1) | Y | Y | — | — | — |
| **Efficiency** | Time Behavior | LOW | TO-15 (AFG) | Alza.cz feed served from cached file (`storage/app/feed/alza.xml`); regeneration runs on 4-hour scheduler schedule. No explicit response-time SLA defined for Sprint 6. | — | — | — | — | — |
| **Support** | Support | N/A | — | ToolShop is self-developed; no vendor support contract applies | — | — | — | — | — |

#### 2.3.3 Test Strategy Specials

- **Story 5 is externally blocked.** `specify-task` for Story 5 must not be started until the Alza.cz XML schema is obtained and the Field Mapping Table in `plan.md` is completed. Test situations TS-20.x and TS-21.x cannot be finalised until then.
- **PayU e2e test (TS-13.2)** requires a Playwright scenario that initiates a PayU redirect then cancels on the hosted page. This test depends on the PayU CZ sandbox being available and is classified as a separate e2e run outside the standard CI unit suite.
- **Security review** of `PayUWebhookController` HMAC verification and `AlzaFeedController` bearer token guard is mandatory before Story 3 and Story 5 are promoted to production. Non-functional test situations TS-14.1 and TS-14.2 verify the absence of sensitive data at the data layer.

---

### 2.4 Test Automation Strategy (ITPF mandatory)

| Layer | Tool | Scope | CI Trigger |
|---|---|---|---|
| Laravel unit tests | PHPUnit | Service classes, models, controllers, route guards (Stories 2–6) | On every PR merge to main |
| Angular unit tests | Jasmine / Karma | Angular components and services (Stories 1, 6, 7) | On every PR merge to main |
| Laravel integration tests | PHPUnit (feature tests) | API flows: PayU checkout (Story 3), Alza feed endpoint (Story 5) | On every PR merge to main |
| End-to-end tests | Playwright (Chromium) | PayU cancel/retry flow (Story 3, TS-13.2) | On-demand after Story 3 merge; requires sandbox credentials |

**Automation coverage target:** 100% of named test situations in `test-design.md` are automated at unit or integration level. The single e2e situation (TS-13.2) is automated via Playwright. No manual-only test situations are planned.

**Test isolation:** Each test is responsible for its own state setup and teardown. No shared mutable state between tests. Laravel tests use database transactions rolled back per test. Angular tests use `TestBed.resetTestingModule()` after each spec.

---

### 2.5 Definition of Ready / Definition of Done (ITPF mandatory)

**Definition of Ready (Test Entry Criteria)**

- BACs for the story are defined and agreed with the Product Owner
- Test situations in `test-design.md` for the story's BACs are reviewed by QA
- Story dependencies (see plan.md dependency graph) are fulfilled (predecessor story merged)
- For Story 3: PayU CZ sandbox credentials are available in `.env`
- For Story 5: Alza.cz XML schema is obtained and the Field Mapping Table in `plan.md` is completed
- Test environment is available and `APP_ENV=testing` is set

**Definition of Done (Test Exit Criteria)**

- All test situations for the story's BACs pass in CI with no failures
- 0 BLOCKER defects open against the story
- All HIGH defects resolved or explicitly risk-accepted by the Product Owner
- BAC Coverage Matrix in `test-design.md` updated to ✓ for all BACs covered by the story
- Story-level test report produced and attached to the story ticket
- No sensitive data (card numbers, credentials) present in any test artefact or log

---

## 3 Documentation (all ITPF mandatory)

| Document | Description | Where to find |
|---|---|---|
| Test Plan (this document) | ITPF-format sprint test plan | `.sdlc/prds/toolshop-czech-market-sprint6/test-plan-itpf.md` |
| ISO/IEC/IEEE 29119-3 Test Plan | Complementary standards-conformant test plan | `.sdlc/prds/toolshop-czech-market-sprint6/test-plan.md` |
| Test Situations Catalogue | 57 test situations across 27 BACs | `.sdlc/prds/toolshop-czech-market-sprint6/test-design.md` |
| Test Specifications (automated) | PHPUnit and Jasmine test files per story | Story implementation branches / `spec/` and `tests/` directories |
| Defects / Bugs | Raised during test execution | Jira project (sprint backlog) |
| Test Summary Report | Produced at sprint closure | To be created after all stories merge |

### 3.1 Defect Management

Defects discovered during test execution are raised as Jira bugs in the sprint backlog, linked to the story and the failing BAC. Severity levels:

| Severity | Definition | Release Gate |
|---|---|---|
| BLOCKER | Prevents a BAC from being verified at all | Must be fixed before story Done |
| HIGH | BAC verified but with a significant deviation | Must be fixed before release |
| MEDIUM | Minor deviation; workaround exists | Fixed in current or next sprint |
| LOW | Cosmetic or trivial | Backlog for prioritisation |

### 3.2 Test Reporting

Test progress is reported as follows:
- **Daily:** CI pipeline status visible to all team members on the CI dashboard.
- **Per story:** Story-level test report summarising pass/fail counts, open defects, and BAC coverage status — produced when the story PR is raised.
- **Sprint closure:** Sprint Test Summary Report summarising all 27 BACs, overall pass rate, defect metrics, and any deferred items (notably Story 5 if schema is still pending).

---

## 4 Test Environments and Test Data (ITPF mandatory)

### 4.1 Test Environments & Test Data Management

| Environment | Purpose | `APP_ENV` | Mock PayU routes | Notes |
|---|---|---|---|---|
| Local developer | Development and unit test execution | `local` / `testing` | Active | Developer-managed |
| CI pipeline | Automated unit + integration tests on every PR | `testing` | Active | No PayU sandbox credentials; Stories 1, 2, 4, 6, 7 fully covered |
| Integration / staging | Story 3 integration and e2e tests | `testing` / `staging` | Active | Requires PayU CZ sandbox credentials in `.env` |
| Production | Live environment | `production` | **INACTIVE** (route not registered) | `APP_ENV=production` must be set by DevOps before Story 6 go-live |

**Test data per story:**

| Story | Test Data Required | Seeding Mechanism |
|---|---|---|
| Story 1 | Browser with `lang=cs` cookie; browser without cookie | Angular fixture / `document.cookie` mock in Jasmine |
| Story 2 | Products with and without Czech fields (`title_cs`, `description_cs`, `specs_cs`) | Laravel database factory — `translated` and `untranslated` states |
| Story 3 | CZ and non-CZ billing/shipping address pairs; mock PayU scenarios (success, declined, unavailable, timeout); payment cancel simulation | `X-PayU-Mock-Scenario` header; PayU sandbox; Playwright navigation |
| Story 4 | Carts with weight < 10 kg and ≥ 10 kg; addresses in CZ, DACH, EU (non-DACH), US, Others; CZ and non-CZ billing countries | Laravel request fixtures; all 8 region × weight combinations |
| Story 5 | Products with `title_cs` populated; one product updated for price and one for stock after initial feed run | Laravel database seeder; requires Story 2 migrations |
| Story 6 | `X-PayU-Mock-Scenario` header values: none, `declined`, `unavailable`, `timeout`; `APP_ENV=production` simulation | PHPUnit env override; Angular `HttpClientTestingModule` |
| Story 7 | Form field values; 200 and 422 mock responses; network error simulation | Angular HTTP mock stubs |

Credentials (PayU sandbox key/secret, `ALZA_FEED_TOKEN`) are stored only in `.env` and never committed to version control.

---

## 5 Release Management (ITPF mandatory)

### 5.1 Deployment of Test Objects

Each story is independently deployable. Deployment is via the existing CI/CD pipeline (GitHub Actions or equivalent) on merge to main. Environment-specific `.env` values control feature availability:

- `APP_ENV=production` disables the mock PayU routes (Story 6 guard).
- `ALZA_FEED_TOKEN` must be set before the Alza feed endpoint is live (Story 5).
- PayU credentials (`PAYU_API_KEY`, `PAYU_API_SECRET`) must be set before Story 3 processes real payments.
- In-flight orders at deployment retain their original flat-rate delivery cost; weight-based pricing applies only to new orders created after Story 4 is deployed.

### 5.2 Release Management

Stories are released to production individually as they pass the Definition of Done. The overall sprint release is declared when all 7 stories are deployed and the Test Summary Report is signed off by the Product Owner. Story 5 may be deferred to a post-sprint release if the Alza.cz schema is not obtained during the sprint.

---

## 6 Team / Test Responsibilities (all ITPF mandatory)

According to the RBI Whole-Team-Approach, every team member is included in test activities.

| What | Who |
|---|---|
| Implementing unit and integration tests per story | Developer |
| Reviewing test situations and BAC coverage before story implementation | QA Engineer |
| Executing e2e tests (Story 3 Playwright run) | QA Engineer |
| Raising and triaging defects | QA Engineer |
| Reviewing BAC Coverage Matrix per story | Product Owner |
| Approving product release (story Done and sprint closure) | Product Owner |
| Configuring `APP_ENV=production` in production deployment | DevOps / Platform Team |
| Procuring Alza.cz XML schema and PayU CZ sandbox credentials | Business / Partnerships |
| Producing sprint Test Summary Report | QA Engineer |

---

## 7 Risks (all ITPF mandatory)

### 7.1 Product Risks

| Risk ID | Risk Impact | Probability | Mitigation |
|---|---|---|---|
| PR-1 | Language switcher absent on one or more customer-facing pages → customers cannot switch to Czech → reduced sales conversion (BAC-1) | Low | `test_language_switcher_renders_on_all_customer_facing_pages` explicitly iterates all customer-facing routes |
| PR-2 | Czech locale activation triggers page reload, discarding cart state or applied filters (BAC-2) | Low | TS-2.2 explicitly verifies in-progress state is preserved during locale switch |
| PR-3 | Language cookie is session-scoped; Czech not restored after full browser restart (BAC-3) | Medium | TS-3.3 verifies cookie survives full browser close via persistent cookie attribute |
| PR-4 | Checkout steps or confirmation page display English strings when locale is Czech (BAC-4, BAC-5) | Low | TS-4.1, TS-4.2, TS-5.1, TS-5.2 enumerate all checkout steps and validation error categories |
| PR-5 | `Product::localise('cs')` returns blank or broken content instead of English fallback for untranslated products (BAC-7) | Low | TS-7.1, TS-7.2 use null and empty-string Czech field variants |
| PR-6 | Delivery price does not match US4200 matrix for one or more region × weight combination (BAC-15) | Medium | TS-15.4 covers all 8 combinations parametrically |
| PR-7 | Zásilkovna option displayed for EU (non-DACH) or US shipping addresses (BAC-17) | Low | TS-17.1, TS-17.2 verify exclusion for both non-eligible regions |
| PR-8 | Card number, CVV, or raw gateway credentials persisted in database or log entries after PayU payment (BAC-14) | Low | TS-14.1 inspects order record columns; TS-14.2 inspects logs — both are MUST |
| PR-9 | Mock PayU route (`POST /mock/payu/orders`) reachable in production, allowing simulated payment bypass | Low | TS-25.1 verifies 404 with `APP_ENV=production`; classified Critical impact |
| PR-10 | Alza.cz feed endpoint returns 200 without valid bearer token, exposing product pricing and stock data | Low | TS-20.3 verifies 401 for missing/invalid token |

### 7.2 Organizational Risks

| Risk ID | Risk Impact | Probability | Mitigation |
|---|---|---|---|
| OR-1 | Alza.cz XML schema not obtained during the sprint → Story 5 blocked → Czech products not discoverable on Alza.cz at release | High | Assign external procurement action to Business/Partnerships at sprint kick-off; Story 5 `specify-task` is explicitly blocked until schema is received |
| OR-2 | PayU CZ sandbox credentials not provisioned before Story 3 integration tests → integration test suite cannot be executed → Story 3 cannot reach Done | Medium | Raise credential request with Business team at sprint kick-off; Story 3 unit tests (PHPUnit, Jasmine) can proceed without sandbox credentials |
| OR-3 | `APP_ENV=production` not set in production deployment before Story 6 goes live → mock routes exposed in production | Low (Critical) | DevOps to verify `APP_ENV` in Helm chart / `.env` before any Story 6 deployment; verified independently by TS-25.1 |
| OR-4 | Merge conflict on `Product.php` between Stories 2 and 4 if sequencing is violated | Medium | Story 4 is explicitly blocked on Story 2 merge in the dependency graph; CI branch protection enforces merge order |
| OR-5 | Development team unfamiliar with Transloco locale switch lifecycle → incorrect implementation of BAC-2 (no-reload requirement) | Low | Brief technical spike recommended at Story 1 kick-off to validate Transloco locale change API before full implementation |
