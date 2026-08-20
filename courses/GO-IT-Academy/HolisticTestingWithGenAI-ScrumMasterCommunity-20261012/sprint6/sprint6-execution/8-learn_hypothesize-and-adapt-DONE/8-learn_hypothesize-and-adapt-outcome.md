**Title**

Learn stage — Hypothesize and adapt the ToolShop test automation strategy

**Background**

Sprint 6 delivered Czech-market expansion (US2300, US2350, US3100 PayU, US3150 delivery), security hardening (US3206 password hashing, US3409 auth-library replacement), checkout UX (US4700 login during checkout), and data-layer migration (US5003 ORMapper). Evidence from exploratory tours (`list-of-bugs.md`), CI automation logs (`ta-*.log`), and production payment logs (`test_*.log`) now feeds the Learn stage: turn findings into automation priorities and an updated strategy for sprints 7–8 (United States, China, customer growth).

**Reference**

- Sprint narrative: `sprint6/sprint6-input/sprint6-content.md`, `sprint6/sprint6-input/sprint6-sprintGoal.md`
- User stories: `sprint6/sprint6-input/US*.md`
- Defect list: `sprint6/sprint6-input/list-of-bugs.md`
- Automation baseline: `sprint6/sprint6-input/testAutomationStrategy.md` (empty at sprint start)
- T1T5 design: `sprint6-execution/3-understand_use-atdd_designT1T5-DONE/T1T5-testdesign-description.md`
- CI logs: `sprint6-execution/5-deploy_run-automated-tests_analyzeLogfile-DONE/ta-*.log`, `5-deploy_run-automated-tests-outcome.md`
- Production logs: `sprint6-execution/7-observe_monitor-errors-warnings-DONE/test_*.log`, `7-observe_monitor-errors-warnings-outcome.md`
- Production T1T5 design: `sprint6-execution/6-release_test-in-production_T1T5-Tests-DONE/6-release_test-in-production-transfer-outcome.md`

**Tasks**

### 1. Evidence pack (grounded in Reference files)

| Category | Evidence | Source |
|----------|----------|--------|
| **Critical defects** | Checkout cart total `0,00` (bug 8); payment gateway `Error 304` (bug 12); profile password not hashed after change (bug 39); SQL injection on `users/login` (bug 79); BOLA on `users/{id}` (bug 71); all invoices returned regardless of token (bug 78); registration leaks password hint (bug 50) | `list-of-bugs.md` |
| **High-risk workflows** | Checkout → payment → order; login/session through checkout (US4700); PayU redirect/callback (US3100); password set/change/migration (US3206); auth-library session handling (US3409) | User stories + bugs 71–83 |
| **Frequently failing functionality** | EUR amounts ≥ ~51 EUR rejected (`PAY-VAL-051`, 1376/2500 failures); gateway timeouts (`PAY-TIMEOUT-504`, 784 failures); session/callback errors (`PAY-GEN-400`, 340 failures); daily collapse after 00:30 (0 % success after 01:00) | `7-observe_monitor-errors-warnings-outcome.md`, `test_*.log` |
| **Production risks** | 83.3 % failure rate on PayU production validation; T4 concurrent load worst at 12.1 % pass; dummy gateway in lower env hid real defects | `7-observe_monitor-errors-warnings-outcome.md` |
| **Regression-prone areas** | Checkout wizard (billing address swapped fields bugs 15–16); payment method display; localization (Czech language/products + region-gated PayU); ORMapper field mapping (US5003); auth migration side effects | Sprint stories + FedEx-style bugs |
| **Stable vs unstable** | **Relatively stable:** product browse/search (mostly cosmetic/typo defects). **Unstable:** payment gateway responses, checkout session handoff, EUR policy thresholds, auth under concurrency, ORMapper-migrated persistence | Bug list + log analysis |
| **Business-critical journeys** | Guest/logged-in checkout to paid order; Czech customer PayU path; account login/registration; admin/catalog API boundaries as traffic grows | Sprint goal + US3100/US4700 |
| **What worked** | T1T5 naming in production logs (traceable scenario, error code, amount, currency); risk-based exploratory tours (BadBoy, FedEx); structured log prompts surfaced reproducible EUR threshold and time-window defects | Task 3, 6, 7 outcomes |
| **What created risk** | Test-layer flakiness (~770 `FLAKY_PASS`, locator drift `data-testid` vs `data-qa`) masked product signal; no automation spine on revenue path; parallel major changes (auth, ORMapper, PayU, checkout UX) without narrow merge gate | `5-deploy_run-automated-tests-outcome.md` |

---

### 2. Prompt A — evidence synthesis (structured, file-grounded)

Used with a general-purpose model; attached evidence-pack summary plus excerpts from `list-of-bugs.md` (security rows 71–83) and production log headline metrics from task 7 outcome.

```
You are a QA lead updating ToolShop's test automation strategy after Sprint 6.

System under test: ToolShop e-commerce webshop (Practice Software Testing).

Sprint 6 delivered:
- Czech market: language (US2300), products (US2350), PayU (US3100), delivery options (US3150)
- Security: MD5 password hashing (US3206), replace Auth-X32 with Auth-X64 (US3409)
- Checkout: login during checkout (US4700)
- Data: ORMapper 1.2 → 2.0 (US5003)

I will paste an EVIDENCE PACK (defects, log signals, stable/unstable areas). Ground every conclusion in that pack only.

Produce markdown with these sections:

1. **What worked** — testing activities and artefacts that reduced risk or improved feedback.
2. **What created risk** — gaps in automation, env fidelity, or coverage that let defects reach production.
3. **Automation value map** — table: area | automate first / automate later / keep manual-exploratory | rationale (business impact, regression risk, frequency, production stability, security).
4. **Top automation gaps** — max 5 bullets tied to specific evidence items (cite bug ID or log error code).
5. **Sprint 7–8 implications** — US + China expansion + customer growth: what must change in automation sequencing, monitoring, and manual charters (do not invent new user stories; extrapolate from Czech/PayU and security patterns only).

Rules:
- Label each claim FACT (from pack) vs INFERENCE.
- Do not recommend automating cosmetic UI typos unless tied to conversion or accessibility compliance.
- Prefer API-level T5 tests for OWASP-class defects already documented.
- End with 3 assumptions you made and what file would verify each.
```

---

### 3. Prompt B — executive prioritisation (different style)

Used with a stakeholder-oriented persona; no fixed section list upfront; stricter “show assumptions” constraint.

```
Act as a Head of Quality advising the ToolShop product team before Sprint 7 (US market) and Sprint 8 (China + customer growth).

Input: Sprint 6 evidence only — Czech launch, PayU production logs (83% failure, EUR≥51 defect, daily 00:30 collapse), 94 documented bugs including critical API auth flaws, and CI logs showing test-harness instability (FLAKY_PASS, locator drift).

Deliver:

1. **Executive summary** (≤6 sentences): automation posture today vs needed posture for multi-market growth.
2. **Ten-test shortlist** — exactly 10 T1 or T5 scenario titles (T1T5 naming syntax). For each: one line on business value protected and primary risk mitigated. Rank 1–10.
3. **Assumptions table** — assumption | why it matters | what would falsify it | artefact to check.
4. **What stays manual** — 5 areas with one-sentence justification each.
5. **Monitoring spine** — 5 metrics/alerts that must exist before US traffic (name error codes or log patterns where possible).

Constraints:
- Challenge any suggestion to “automate everything” or “automate all UI typos.”
- If you lack evidence for a claim, write INSUFFICIENT EVIDENCE — do not fill gaps with generic best practice.
- T5 must cover at least 3 security/abuse scenarios from the bug list.
```

---

### 4. Prioritised top 10 T1&T5 tests to automate next

| Order | Type | Test title | Why automate | Risk mitigated | Business value |
|-------|------|------------|--------------|----------------|----------------|
| 1 | T1 | T1_Checkout_ValidPaymentCheckout_OrderProcessedSuccessfully | Highest-frequency revenue journey; merge gate for every release | Broken checkout, misconfigured payment flow | Revenue, conversion, trust |
| 2 | T1 | T1_Payment_ValidPaymentAuthorization_PaymentAccepted | Isolates provider/currency rules from UI noise; matches log failure cluster | False declines, provider mismatch | Payment completion |
| 3 | T1 | T1_OrderManagement_SuccessfulPayment_OrderCreatedSuccessfully | Confirms persistence after money path | “Paid but no order”, wrong totals | Fulfillment, support cost |
| 4 | T1 | T1_Authentication_ValidCustomerLogin_LoginSuccessful | Prerequisite for growth features; flanked by US3409/US3206 | Login regression, session failure | Account access, checkout |
| 5 | T1 | T1_Checkout_LoginDuringCheckout_ProceedToCheckoutContinuesFlow | US4700; abandonment-sensitive mid-flow login | Dead-end checkout for guests | Cart completion, market rollout |
| 6 | T5 | T5_Payment_InvalidPaymentCallback_RequestRejected | Logs show callback/session failures; fraud/reconciliation risk | Forged or duplicate callbacks | Financial integrity |
| 7 | T5 | T5_Checkout_EURHighAmountPolicy_EnforcedOrDocumentedConsistently | Targets recurring PAY-VAL-051 before USD/CNY policies add complexity | Silent amount-policy mismatch | Large-order revenue |
| 8 | T5 | T5_Authentication_SQLInjection_LoginRejected | Documented critical defect (bug 79) | Account takeover | Brand, legal exposure |
| 9 | T5 | T5_API_CrossUserResourceAccess_RequestForbidden | BOLA on `users/{id}` (bug 71) | Horizontal privilege escalation | GDPR, privacy, trust |
| 10 | T5 | T5_AdminDestructiveOperation_WithoutAdminRole_Returns403 | Wrong 401 vs 403 and missing role checks (bugs 74, 76, 83) | Unauthorized catalog destruction | Operational safety |

*Verification note:* Prompt B initially ranked PayU-specific T1 above generic checkout; after cross-checking production logs and bug list, order 1–3 were kept as market-agnostic revenue spine; PayU Czech coverage is embedded in orders 1–2 via parametrized payment method and region fixtures (aligned with Prompt A automation-value map).

---

### 5. Updated test automation strategy (sprints 7–8: US, China, customer growth)

#### 5.1 Purpose and scope

ToolShop automation shall protect **revenue, security, and multi-market correctness** with a **small fast API-heavy spine** (checkout, payment, order, auth) as the default merge gate; UI automation widens only where localization and checkout layout historically hurt conversion.

#### 5.2 Automation sequencing

| Phase | Focus | Timing |
|-------|-------|--------|
| **Phase 1 — Spine** | Orders 1–5 + T5 payment callback and SQL injection (6, 8) | Immediate merge gate |
| **Phase 2 — Policy & authZ** | EUR/USD/CNY amount-policy T5 (7); BOLA and admin-role T5 (9–10) | Before US launch (Sprint 7) |
| **Phase 3 — Market matrix** | Parametrized T1 per locale: language, currency, tax display, payment-method visibility (PayU CZ-only; US card/wallet; China local methods TBD) | Sprint 7–8 cadence |
| **Phase 4 — Scale signals** | Scheduled T4 load job (not every PR); feed timeout/concurrency learnings into T1/T5 boundary tests | Customer-growth milestones |

#### 5.3 What remains manual or exploratory

- First-time **UX, copy, and localization quality** per market (US2300-style issues are not regression-automation targets).
- **Accessibility** and broad cosmetic UI (contrast, typos on home — sample via charters).
- **New third-party integrations** until sandbox contracts are trustworthy (China payment providers).
- **Chartered sessions** after auth-library, ORMapper, or major schema migrations.
- **Visual/layout** browser-specific defects (SuperModel tour findings).

#### 5.4 Monitoring and feedback loops

Continuous production monitoring (aligned with automated test identifiers):

| Signal | Alert trigger | Feedback into tests |
|--------|---------------|---------------------|
| `PAY-VAL-051` / USD/CNY policy codes | Spike by currency, region | Extend T5 amount-policy scenarios |
| `PAY-TIMEOUT-504` | Rate > baseline by hour/method | Add timeout/retry T3/T5 cases |
| `PAY-GEN-400` session/callback | Burst after deploy | T5 callback rejection; session-at-checkout T1 |
| Auth error rate | Post US3409/US3206 deploy | T1 login + T5 injection suite |
| Order–payment reconciliation | Mismatch count > 0 | T1 order-after-payment |

CI reliability policy: treat **`FLAKY_PASS` as failure** for merge; fix locator contract (`data-qa` map) before expanding UI coverage.

#### 5.5 Sprint 7–8 market specifics

- **United States (Sprint 7):** Parametrize checkout spine for USD, US shipping rules (extend US2450-style thresholds), no PayU; add T5 for wrong-region payment visibility.
- **China (Sprint 8):** Locale/language matrix smoke T1; payment and compliance unknowns stay exploratory until integration stable; monitor currency and redirect behaviour like PayU learnings.
- **Customer growth:** Promos, concurrent checkout, catalog scale — T4 on schedule; concurrency failures upgrade T1 session-handoff cases.

---

### 6. Strategy delta (vs baseline `testAutomationStrategy.md`)

| Baseline state | Change | Why (evidence) |
|----------------|--------|----------------|
| **Empty document** — no stated scope, layers, or gates | Defined **automation spine** (checkout → payment → order → auth) as merge gate | Production PayU logs: 83 % failure on revenue path; checkout bugs cluster (8, 12, 15–16) |
| No distinction test-layer vs product defects | **CI reliability policy**: fail on retry-only pass; locator contract ownership | `ta-*.log`: ~770 FLAKY_PASS; 115× mini-cart locator drift |
| No production monitoring linkage | **Monitoring spine** tied to error codes used in T1T5 logs | PAY-VAL-051, PAY-TIMEOUT-504, PAY-GEN-400 reproducible across 5 days |
| No market expansion plan | **Phased market matrix** (Czech proven patterns → US → China) | Sprint goal pivot; US3100 region gating; sprints 7–8 direction in task brief |
| No security automation priority | **T5 OWASP suite** in Phase 1–2 (SQLi, BOLA, admin authZ) | Bugs 71–83 documented in exploratory tours |
| Implicit “automate UI broadly” | **Explicit manual/exploratory reserve** for cosmetic, a11y, first-locale UX | 94 bugs: majority cosmetic/FedEx; revenue defects concentrated in checkout/payment/API |
| No feedback from production to test design | **Closed loop**: alert thresholds → new/updated T5 scenarios | EUR threshold found only in production logs, not dummy gateway |

---

### 7. Outcome table (required)

| Feature or use case to cover | T1T5 test title | Implementation order (1–10) | Reason |
|------------------------------|-----------------|-----------------------------|--------|
| Single-product checkout to confirmed payment | T1_Checkout_ValidPaymentCheckout_OrderProcessedSuccessfully | 1 | Core revenue path; anchors every regional and gateway change |
| Payment provider authorization | T1_Payment_ValidPaymentAuthorization_PaymentAccepted | 2 | Log-backed failure hotspot; isolates provider and currency rules |
| Order creation after successful payment | T1_OrderManagement_SuccessfulPayment_OrderCreatedSuccessfully | 3 | Protects fulfillment from “paid but not recorded” |
| Customer login | T1_Authentication_ValidCustomerLogin_LoginSuccessful | 4 | Prerequisite for growth; flanked by auth library and hashing work |
| Login step inside checkout (guest → logged-in) | T1_Checkout_LoginDuringCheckout_ProceedToCheckoutContinuesFlow | 5 | US4700; reduces abandonment on Czech path and future markets |
| Payment callback handling | T5_Payment_InvalidPaymentCallback_RequestRejected | 6 | Security and reconciliation; correlates with callback/session log noise |
| EUR high-amount payment rules | T5_Checkout_EURHighAmountPolicy_EnforcedOrDocumentedConsistently | 7 | Targets PAY-VAL-051 before USD/CNY policies add complexity |
| SQL injection on authentication API | T5_Authentication_SQLInjection_LoginRejected | 8 | Critical OWASP defect (bug 79); must stay locked in CI |
| API broken object level authorization | T5_API_CrossUserResourceAccess_RequestForbidden | 9 | Prevents cross-customer data exposure as traffic grows |
| Admin-only destructive API operations | T5_AdminDestructiveOperation_WithoutAdminRole_Returns403 | 10 | Protects catalog as staff tooling and integrations expand |

---

### 8. Reflection — prompts, model errors, next time

| Aspect | Prompt A (structured synthesis) | Prompt B (executive prioritisation) |
|--------|--------------------------------|-------------------------------------|
| **Strength** | Forced FACT vs INFERENCE and automation-value map prevented skipping security/API gaps | Assumptions table surfaced implicit US/China extrapolations for explicit verification |
| **Weakness** | Tendency to over-list “automate later” UI items without tying to conversion | Initially placed PayU-only T1 at rank 1 without noting EUR defect affects all EUR checkout, not PayU alone |
| **Unsafe without file checks** | Suggested automating “Czech translation completeness” as P1 — reclassified to manual charter after checking bug list (mostly typos, not missing AC) | Proposed “automate all invoice pagination” (bug 13) — deprioritised after confirming lower business impact vs payment spine |
| **Next time** | Paste bug IDs 71–83 and log error-code counts inline; add “cite source row” column | Require model to map each of 10 tests to a bug ID or log pattern before accepting rank order |

**Takeaway:** Structured synthesis (A) produces defensible strategy sections; executive prioritisation (B) stress-tests ranking but needs guardrails against market-specific bias and generic “automate everything” drift. Both outputs were merged only after verification against `list-of-bugs.md` and task 7 log metrics — the EUR ≥51 defect and daily 00:30 collapse were the decisive signals for payment-spine-first sequencing and production monitoring hooks.
