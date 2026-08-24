# Test Plan — ToolShop Czech Market Entry — Sprint 6

Standard: ISO/IEC/IEEE 29119-3
Date: 2026-08-12
Version: 1.0
Status: Draft

---

## 1. Context

### 1.1 Test Plan Identifier

TP-plan-toolshop-czech-market-sprint6-v1.0

### 1.2 Scope of Testing

This test plan covers all seven stories in the ToolShop Czech Market Entry (Sprint 6 / HOLTES) initiative. The system under test is the ToolShop e-commerce application comprising a Laravel (PHP) backend and an Angular 17 frontend. The plan applies to three test levels: unit testing of all new service classes and UI components, integration testing of API flows and webhook handling, and end-to-end testing of critical checkout paths via Playwright.

Source plan: `.sdlc/prds/toolshop-czech-market-sprint6/plan.md`  
Source test design: `.sdlc/prds/toolshop-czech-market-sprint6/test-design.md`

**In scope:** Czech localisation (Stories 1–2), PayU payment integration and mock (Stories 3, 6), weight-based delivery pricing (Story 4), Alza.cz XML product feed (Story 5), PayU TIP testing UI (Story 7).

**Out of scope:** Admin portal, order fulfilment backend, mobile/native clients, non-Czech users' payment flows (except to verify PayU is not shown), translation of the PayU TIP page (English-only by design).

> ⚠️ Story 5 (Alza.cz XML Feed) cannot enter implementation until the Alza.cz partner portal XML schema is obtained. Test execution for Story 5 is blocked by this external dependency.

### 1.3 Assumptions and Constraints

- Stories 1 and 6 start in parallel at sprint kick-off; all other story dependencies follow the order defined in `plan.md`.
- Story 5 is externally blocked by procurement of the Alza.cz XML feed schema from the partner portal.
- Story 3 integration tests require PayU CZ sandbox credentials to be provisioned before execution.
- The `APP_ENV` guard preventing mock PayU routes from being registered in production must be enforced via deployment configuration before Story 6 goes live.
- All tests run against a local or CI environment (`APP_ENV=testing`) with PHP/Laravel and Angular test toolchains available.
- Test data is seeded via database factories and fixture helpers; no shared test database state is permitted between test runs.
- In-flight orders at deployment retain the original flat-rate delivery cost; weight-based pricing applies only to orders created after Story 4 is deployed.

### 1.4 Stakeholders

| Stakeholder | Role | Interest |
|---|---|---|
| Developer | Implementer | Test implementation, CI pipeline green |
| QA Engineer | Tester | Test design, execution, defect reporting |
| Product Owner | Decision-maker | BAC coverage, release approval |
| DevOps / Platform Team | Infrastructure | `APP_ENV=production` configuration (Story 6 go-live guard) |
| Business / Partnerships | External dependency owner | Alza.cz schema procurement; PayU CZ sandbox credential provisioning |

---

## 2. Communication

### 2.1 Progress Reporting

Test progress is reported per story at the point each story's branch is merged to the main line. A CI dashboard provides real-time pass/fail status for unit and integration suites. E2E results are reported after each e2e run. A consolidated test summary report is produced at sprint closure.

### 2.2 Test Plan Change Procedure

Changes to this document require:
1. A version bump in the header (e.g., `1.0` → `1.1`).
2. A description of the change in the revision history comment at the foot of the file.
3. Reviewer sign-off from the QA Engineer and Product Owner before the updated plan is baselined.

Unplanned changes to BACs or story scope that affect test situations must trigger a corresponding revision to `test-design.md` before implementation proceeds.

---

## 3. Risk Register

### 3.1 Product Risks

| Risk ID | Risk | BAC(s) Affected | Probability | Impact | Test Coverage |
|---|---|---|---|---|---|
| PR-1 | Language switcher absent from one or more customer-facing pages | BAC-1 | Low | High | TS-1.1 |
| PR-2 | Czech locale activation triggers page reload, discarding in-progress state | BAC-2 | Low | High | TS-2.1, TS-2.2 |
| PR-3 | Language cookie is session-scoped rather than persistent; Czech not restored after full browser restart | BAC-3 | Medium | Medium | TS-3.1, TS-3.2, TS-3.3 |
| PR-4 | Checkout step labels or confirmation copy not resolved from Czech translation file | BAC-4 | Low | High | TS-4.1, TS-4.2 |
| PR-5 | Form validation error messages fall back to English when locale is Czech | BAC-5 | Low | Medium | TS-5.1, TS-5.2 |
| PR-6 | `Product::localise('cs')` returns English content instead of Czech when Czech fields are populated | BAC-6 | Medium | High | TS-6.1, TS-6.2 |
| PR-7 | Product with null Czech fields renders blank title or description instead of English fallback | BAC-7 | Low | High | TS-7.1, TS-7.2 |
| PR-8 | Search results return English titles for Czech-locale requests when Czech translations exist | BAC-8 | Low | Medium | TS-8.1 |
| PR-9 | Cart or checkout order summary shows English product title when locale is Czech | BAC-9 | Low | Medium | TS-9.1, TS-9.2 |
| PR-10 | PayU payment option displayed to customers with non-Czech billing and shipping addresses | BAC-10, BAC-11 | Low | High | TS-10.1, TS-10.2, TS-11.1 |
| PR-11 | Failed or cancelled PayU payment creates an order record in the database | BAC-12, BAC-13 | Low | High | TS-12.1, TS-13.1, TS-13.2 |
| PR-12 | Card number, CVV, or raw gateway credentials persisted in database or log entries | BAC-14 | Low | Critical | TS-14.1, TS-14.2 |
| PR-13 | Delivery price does not match the US4200 pricing matrix for one or more region × weight combinations | BAC-15 | Medium | High | TS-15.1, TS-15.2, TS-15.3, TS-15.4 |
| PR-14 | Zásilkovna delivery option displayed for EU (non-DACH) or US shipping addresses | BAC-16, BAC-17 | Low | Medium | TS-16.1, TS-16.2, TS-17.1, TS-17.2 |
| PR-15 | Incorrect weight tier selected at the 10 kg boundary, producing wrong delivery price | BAC-18 | Low | High | TS-18.1, TS-18.2 |
| PR-16 | Delivery price returned in USD for Czech billing customers, or CZK for non-Czech customers | BAC-19 | Low | High | TS-19.1, TS-19.2 |
| PR-17 | Alza.cz feed endpoint returns invalid XML, wrong Content-Type, or omits Czech-translated products | BAC-20 | Medium | High | TS-20.1, TS-20.2, TS-20.3 |
| PR-18 | Product price or stock changes not reflected in the Alza feed after the scheduled 4-hour export | BAC-21 | Low | High | TS-21.1, TS-21.2 |
| PR-19 | Mock PayU route (`POST /mock/payu/orders`) reachable in production environment | BAC-25 | Low | Critical | TS-25.1 |
| PR-20 | PayU TIP component sends request with missing fields or incorrect `X-PayU-Mock-Scenario` header | BAC-27 | Low | Medium | TS-27.1, TS-27.2, TS-27.3, TS-27.4 |

### 3.2 Project Risks

| Risk ID | Risk | Probability | Impact | Mitigation |
|---|---|---|---|---|
| TR-1 | Alza.cz XML feed schema not obtained before Story 5 is scheduled for implementation | High | High | Block `specify-task` for Story 5 until schema is received; assign external procurement action to Business/Partnerships immediately |
| TR-2 | PayU CZ sandbox credentials not provisioned before Story 3 integration tests are needed | Medium | High | Raise credential request with Business team at sprint kick-off; Story 3 unit tests can proceed without credentials; integration tests are gated |
| TR-3 | `APP_ENV=production` not configured in production deployment before Story 6 goes live | Low | Critical | DevOps to add `APP_ENV=production` to Helm chart / `.env` before Story 6 deployment; verified by TS-25.1 in CI |
| TR-4 | Merge conflict on `Product.php` between Stories 2 and 4 if sequencing is violated | Medium | Medium | Enforce strict story sequencing: Story 4 must not be merged before Story 2; verify migration order in CI |
| TR-5 | Merge conflict on `AppComponent`/`HeaderComponent` between Stories 1 and 7 | Low | Low | Enforce strict sequencing: Story 7 after Story 1 |
| TR-6 | Angular 17 / Transloco version incompatibility discovered mid-Story 1 | Low | High | Perform a brief technical spike at Story 1 kick-off to validate Transloco installation before full implementation |

---

## 4. Test Strategy

### 4.1 Test Sub-Plans

No separate level-specific sub-plans are maintained. This document covers unit, integration, and end-to-end test levels for all seven stories. Story-level test situation catalogues are maintained in `test-design.md`.

### 4.2 Test Design Techniques

| Technique | Applied To |
|---|---|
| Equivalence Partitioning | Locale (cs / en), billing/shipping country (CZ / non-CZ), `APP_ENV` (production / testing), Zásilkovna eligibility (CZ+DACH / other) |
| Boundary Value Analysis | Weight tier boundary (9.99 kg → Light, 10.00 kg → Heavy); empty vs. non-empty Czech product fields for fallback logic |
| Use Case Testing | End-to-end locale switching (BAC-2, BAC-3); full checkout flows with PayU (BAC-12, BAC-13); PayU TIP form submission and modal (BAC-27) |
| State Transition Testing | Language switcher (EN → CS → EN); payment state (initiated → confirmed / declined / cancelled) |
| Error Guessing | Language cookie unavailable or corrupted; PayU webhook with invalid HMAC signature; Alza feed endpoint with missing or invalid bearer token; non-JSON mock PayU response body |
| T1T5 Naming Convention | All Laravel test names follow `snake_case` verb-object pattern; Angular tests follow `it('description')` format consistent with Jasmine conventions |

### 4.3 Test Completion Criteria

| Level | Criterion |
|---|---|
| Story | All test situations for that story's BACs pass in CI; no BLOCKER defects open against that story |
| Sprint Release | All 27 BACs in the BAC Coverage Matrix are marked ✓; all e2e situations pass; 0 BLOCKER defects open; Story 5 test closure is conditional on Alza.cz schema procurement |

### 4.4 Metrics

| Metric | Target |
|---|---|
| BAC coverage | 100% (27 / 27) |
| Test situation pass rate (pre-release) | ≥ 95% |
| Open BLOCKER defects at release | 0 |
| Open HIGH defects at release | 0 (or documented risk accepted by Product Owner) |
| Test situations executed per story | 100% of situations linked to that story's BACs |

---

## 5. Test Activities

### 5.1 Test Planning

This document. Must be reviewed and approved by the QA Engineer and Product Owner before story implementation begins.

### 5.2 Test Design and Implementation

Test situations are derived from BACs in `test-design.md` (57 situations across 27 BACs). Each test situation maps to one or more test names listed in `plan.md`. Developers implement test code as part of each story's definition of done; QA reviews test situation coverage before marking a story as testable.

### 5.3 Test Execution

| Story | Test Level(s) | Situations | Execution Order | Prerequisite |
|---|---|---|---|---|
| Story 1: Czech Language & Localisation | Unit (Angular) | TS-1.1, TS-2.1–2.2, TS-3.1–3.3, TS-4.1–4.2, TS-5.1–5.2 | Sprint kick-off (parallel with Story 6) | None |
| Story 6: Mock PayU Service | Unit (Laravel), Unit (Angular) | TS-22.1, TS-23.1–23.3, TS-24.1–24.3, TS-25.1 | Sprint kick-off (parallel with Story 1) | None |
| Story 2: Czech Product Content | Unit (Laravel) | TS-6.1–6.2, TS-7.1–7.2, TS-8.1, TS-9.1–9.2 | After Story 1 | Story 1 merged |
| Story 4: Delivery Pricing | Unit (Laravel) | TS-15.1–15.4, TS-16.1–16.2, TS-17.1–17.2, TS-18.1–18.2, TS-19.1–19.2 | After Story 2 (parallel with Story 5) | Story 2 merged |
| Story 5: Alza.cz XML Feed | Unit + Integration (Laravel) | TS-20.1–20.3, TS-21.1–21.2 | After Story 2 (parallel with Story 4) | Story 2 merged; Alza.cz schema obtained |
| Story 3: PayU Payment Integration | Unit + Integration (Laravel), E2E (Playwright) | TS-10.1–10.2, TS-11.1, TS-12.1, TS-13.1–13.2, TS-14.1–14.2 | After Story 6 | Story 6 merged; PayU sandbox credentials provisioned |
| Story 7: PayU TIP Testing UI | Unit (Angular) | TS-26.1–26.2, TS-27.1–27.4 | After Stories 1 and 6 | Stories 1 and 6 merged |

### 5.4 Test Reporting

- A per-story test report is produced at story merge, summarising situations executed, pass/fail counts, and any open defects.
- CI pipeline results serve as the primary pass/fail record for unit and integration tests.
- E2E results (Story 3) include Playwright run reports with pass/fail per situation.
- A final sprint-level test summary report is produced at release gate, confirming the BAC Coverage Matrix is fully green.

### 5.5 Test Closure

Test closure is declared when:
1. All 27 BACs in the BAC Coverage Matrix are marked ✓.
2. The final sprint test summary report is reviewed and signed off by the Product Owner.
3. 0 BLOCKER defects remain open.
4. Story 5 closure is separately gated on Alza.cz schema procurement and partner portal configuration — if blocked at release, Story 5 is explicitly noted as deferred.

---

## 6. Test Environment

| Attribute | Value |
|---|---|
| Backend runtime | PHP (Laravel) |
| Backend test framework | PHPUnit |
| Frontend runtime | Angular 17 |
| Frontend test framework | Jasmine / Karma |
| E2E browser | Chromium via Playwright |
| `APP_ENV` | `testing` for all test runs |
| Mock PayU routes | Active in `testing` environment (gated by `APP_ENV != production`) |
| Test data seeding | Database factories for Laravel; fixture helpers for Angular component tests |
| PayU sandbox | Required for Story 3 integration tests; provided via `.env` (never committed) |
| Alza.cz schema | Required before Story 5 test execution |
| CI pipeline | Executes unit and integration suites on every PR merge; E2E suite on demand |

---

## 7. Test Data Requirements

| Story | Data Required | Seeding Mechanism | Notes |
|---|---|---|---|
| Story 1 | Browser with `lang=cs` cookie pre-set; browser without cookie | Angular test fixture / `document.cookie` mock | Test TS-3.2 and TS-3.3 require cookie state simulation |
| Story 2 | Products with `title_cs`, `description_cs`, `specs_cs` populated; products with all Czech fields null | Laravel database factory | Two product variants: `translated` and `untranslated` |
| Story 3 | CZ and non-CZ billing/shipping address combinations; mock PayU configured for success, declined, unavailable, timeout scenarios | PayU sandbox credentials; `X-PayU-Mock-Scenario` header in integration tests | E2E test requires Playwright to simulate payment cancellation on redirect |
| Story 4 | Carts with total weight < 10 kg (Light tier) and ≥ 10 kg (Heavy tier); addresses in each of CZ, DACH, EU (non-DACH), US, and Others regions; CZ and non-CZ billing countries | Laravel factory / request fixture | All 8 region × weight combinations must be covered for TS-15.4 |
| Story 5 | Products with `title_cs` populated (Czech feed candidates); a product with updated price and updated stock level after initial feed generation | Laravel database seeder | Requires Story 2 DB columns to exist |
| Story 6 | No-header request (default scenario); requests with `X-PayU-Mock-Scenario: declined`, `unavailable`, `timeout`; `APP_ENV=production` environment simulation | PHPUnit `env` override; Angular `HttpClientTestingModule` | TS-25.1 requires `APP_ENV=production` without registering routes |
| Story 7 | Form field values (amount, currency, order_id, scenario); network error simulation | Angular `HttpClientTestingModule`; mock HTTP response stubs | |

---

## 8. Responsibilities

| Role | Activities |
|---|---|
| Developer | Implement unit and integration tests per story; maintain CI pipeline; ensure all test situations for their story pass before raising PR |
| QA Engineer | Review test situations in `test-design.md`; verify BAC coverage; execute and record e2e test results; raise defects; author sprint test summary report |
| Product Owner | Review BAC coverage matrix per story; approve test plan; sign off test closure report |
| DevOps / Platform Team | Configure `APP_ENV=production` in production deployment before Story 6 goes live; provide CI environment with required test toolchains |
| Business / Partnerships | Procure Alza.cz XML schema and partner portal access; provision PayU CZ sandbox credentials |

---

## 9. Schedule

| Milestone | Entry Condition | Target |
|---|---|---|
| Test plan approved | — | Before sprint implementation begins |
| Stories 1 & 6 tests passing | Stories 1 & 6 merged to main | After sprint kick-off parallel phase |
| Story 2 tests passing | Story 2 merged | After Story 1 completion |
| Stories 4 & 5 tests passing | Stories 4 & 5 merged (Story 5 conditional on schema) | After Story 2, parallel phase |
| Story 3 tests passing (unit + integration) | Story 6 merged; PayU sandbox credentials provisioned | After Story 6 completion |
| Story 3 e2e tests passing | Story 3 merged; staging environment available | After Story 3 merge |
| Story 7 tests passing | Stories 1 & 6 merged | After Stories 1 & 6 completion |
| All unit & integration tests green | All 7 stories merged | Sprint closure |
| Test summary report finalised | All tests executed; 0 BLOCKER defects | Release gate |
| Test closure | Product Owner sign-off; BAC Coverage Matrix fully green | Before production deployment |
