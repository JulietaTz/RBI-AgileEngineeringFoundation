# Transfer Task Outcome: Learn Stage — Improve the Test Automation Strategy (ToolShop)

## Part 1 — Review of Previous Stages and Patterns

### Critical defects and production signals

Sprint 6 focused on Czech market entry (language, Alza.cz context, PayU), security hardening (password hashing, Auth-X32 replacement), checkout login (US4700), and ORMapper migration. Parallel evidence from `list-of-bugs.md` and production-style payment logs shows:

- **Checkout and payment**: Many defects cluster on checkout (cart totals, billing address, payment method list, gateway errors). Logs repeatedly show **PAY-VAL-051** (EUR transactions from 51 EUR upward), **PAY-TIMEOUT-504**, and **PAY-GEN-400** including **session validation failures** — aligning checkout, session, and gateway behavior as the highest-loss surface.
- **Authentication and account security**: Login validation gaps, aggressive lockout (1 failed attempt vs expected 3), password storage issues (US3206 / bug 39), and **OWASP-class API flaws** (SQL injection on login, BOLA, broken auth, excessive token lifetime, admin operations without admin role).
- **Localization and data integrity**: Czech expansion stories (US2300, US2350, US3100) increase regression risk wherever language, currency, and region-gated payment methods interact with checkout.
- **Data layer**: US5003 (ORMapper 1.2 to 2.0) raises risk of **wrong-field persistence** and performance regressions affecting orders and reference data.

### High-risk and business-critical workflows

- End-to-end **checkout to paid order** (including PayU where applicable).
- **Login and session continuity** through checkout (US4700) and after auth-library changes (US3409).
- **Payment authorization and callbacks** (gateway, currency, regional rules).
- **Account security** (registration, login, password change, API boundaries).

### Frequently failing or unstable functionality (from logs and bugs)

- **EUR high-amount** and **gateway timeout** paths dominate log failures; **evening hours** show concentrated stress in sample logs.
- Checkout **payment method discovery** and **invoice** inconsistencies (wrong method text, swapped address fields) indicate fragile integration and mapping.

### Stable vs unstable (relative)

- **Relatively stable for smoke-level automation**: Product browse, search, and filtering (many issues are cosmetic or typographic; still worth selective checks, not top automation priority vs revenue path).
- **Unstable or high-variance**: Payment gateway responses, checkout session handoff, auth under concurrency, new localization branches, anything touching migrated ORMapper schemas.

### What worked well

- Structured **T1–T5 style** tests in production logs made failure modes traceable to **test name, error code, amount, currency, and method** — a strong template for future monitoring and CI triage.
- **Risk-based and exploratory** tours surfaced deep issues (security, accessibility, FedEx-style data mismatches) that pure happy-path automation would miss.

### What created risks

- Relying on **dummy payment** in lower environments pushed discovery to production.
- **Tight coupling** of UI checkout to gateway and session state without enough **API-level guards**.
- **Parallel major changes** (auth lib, ORMapper, new payment method, checkout UX) without a single **narrowed automation spine** on the revenue path.

### Stronger automation support and highest-value automation

- **Automate first**: Revenue path (checkout, payment, order creation), **auth + session at checkout**, **security-negative API cases** that map to disclosed defects, **regional payment visibility** (e.g. PayU only where supported).
- **Highest value**: Tests that catch **EUR policy / amount thresholds**, **timeouts**, **session invalidation during payment**, and **callback integrity** — because logs prove they recur and directly affect conversion.

---

## Part 2 — Top 10 T1 and T5 Tests to Automate (Prioritized)

Each item is a **T1** (critical happy path / acceptance) or **T5** (negative, abuse, security) test. For each: **why automate**, **risk mitigated**, **business value protected**.


| Order | T1 / T5 | Test title                                                       | Why automate                                                                  | Risk mitigated                                    | Business value                              |
| ----- | ------- | ---------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------- | ------------------------------------------- |
| 1     | T1      | T1_Checkout_ValidPaymentCheckout_OrderProcessedSuccessfully      | Most-run customer journey; baseline for every release and gateway change      | Checkout breakage, misconfigured payment flow     | Revenue, conversion, trust                  |
| 2     | T1      | T1_Payment_ValidPaymentAuthorization_PaymentAccepted             | Isolates authorization from UI noise; matches log failure cluster             | False declines, provider mismatch, currency rules | Successful payment completion               |
| 3     | T1      | T1_OrderManagement_SuccessfulPayment_OrderCreatedSuccessfully    | Confirms persistence and order state after money path                         | “Paid but no order”, inconsistent totals          | Post-purchase fulfillment and support cost  |
| 4     | T1      | T1_Authentication_ValidCustomerLogin_LoginSuccessful             | Gateway to all personalized flows; flanked by auth library change             | Login regression, session establishment failure   | Access to account and checkout              |
| 5     | T1      | T1_Checkout_LoginDuringCheckout_ProceedToCheckoutContinuesFlow   | New US4700 behavior; high UX and abandonment sensitivity                      | Broken mid-checkout login, dead-end checkout      | Cart completion and Czech rollout readiness |
| 6     | T5      | T5_Payment_InvalidPaymentCallback_RequestRejected                | Logs show mixed callback and timeout semantics; fraud and reconciliation risk | Forged or duplicate callbacks, bad handshakes     | Financial integrity and chargeback exposure |
| 7     | T5      | T5_Checkout_EURHighAmountPolicy_EnforcedOrDocumentedConsistently | Directly targets PAY-VAL-051 style failures                                   | Silent policy mismatch across envs and regions    | Large-order revenue and provider compliance |
| 8     | T5      | T5_Authentication_SQLInjection_LoginRejected                     | Documented critical defect on `users/login`                                   | Account takeover, privilege breach                | Brand and legal exposure                    |
| 9     | T5      | T5_API_CrossUserResourceAccess_RequestForbidden                  | BOLA-style issues on `users/{id}`                                             | Horizontal privilege escalation                   | GDPR / privacy and trust                    |
| 10    | T5      | T5_AdminDestructiveOperation_WithoutAdminRole_Returns403         | Wrong status semantics and missing role checks on delete APIs                 | Unauthorized data destruction, weak authZ         | Catalog integrity and operational safety    |


---

## Part 3 — Adjusted Test Automation Strategy (Including Sprints 7–8: US, China, Customer Growth)

### Strategic shifts

- **Automation spine**: Maintain a **small, fast, API-heavy** suite for **checkout, payment, order, and auth** as the default **merge gate**; widen UI coverage only where localization and layout regressions historically hurt conversion (checkout wizard, not every typo on home).
- **Sprints 7–8 (United States, China, customer growth)**:
  - **Parametrize** automation by **market**: language, currency, tax display assumptions, **payment method matrix** (PayU, cards, future local methods), and **shipping rules** (US2450-style thresholds remain relevant as catalogs grow).
  - Add **synthetic probes** for **new regions** before campaigns: smoke T1 per locale plus T5 for **region-only payment** visibility (no PayU where unsupported; valid fallbacks).
  - **Customer growth** implies **scale and promos**: keep **T4-style load** as a scheduled job (not necessarily in every PR), but **feed learnings** from concurrency and peak-hour timeouts back into **T1/T5** boundary tests (session + timeout + retry semantics).
- **Manual / exploratory (keep)**:
  - First-time **UX** and **copy quality** per locale, **accessibility**, and **visual** issues from the bug list.
  - **New third-party integrations** until contracts and sandboxes are trustworthy.
  - **Chartered sessions** after auth ORMapper or major schema migrations.
- **Monitoring and feedback loops**:
  - Dashboards on **PAY-VAL-051**, **504 timeouts**, **session validation** rates by **currency, method, hour, region**.
  - Alert thresholds tied to the **same identifiers** used in automated test reports for faster correlation.

### What to automate first vs later

- **First**: Rows 1–5 in Part 2 (revenue and auth spine), plus **T5 payment and auth injection** (rows 6–9).
- **Next**: Expand **regional matrices** and **admin/API authZ** (row 10) as catalog and staff tooling grow with US/China entry.
- **Remain manual / exploratory**: Broad cosmetic UI, one-off typos, pure visual alignment, and **subjective localization quality** — sample with charters, not full regression automation.

### Risks to monitor continuously

- Payment **timeout rate** and **EUR (and future USD/CNY) amount policy** rejections.
- **Session invalidation** during long checkout and **callback** error spikes.
- **Authentication** error bursts after releases touching **xauth** migration or password flows.
- **Order–payment consistency** (counts of authorized payments vs created orders).

---

## Outcome Table (Required)


| Name of use case / feature covered by automation | T1T5 test title                                                  | Implementation order (1–10) | Reason                                                                                  |
| ------------------------------------------------ | ---------------------------------------------------------------- | --------------------------- | --------------------------------------------------------------------------------------- |
| Single-product checkout to confirmed payment     | T1_Checkout_ValidPaymentCheckout_OrderProcessedSuccessfully      | 1                           | Core ToolShop revenue path; anchors every regional and gateway change                   |
| Payment provider authorization                   | T1_Payment_ValidPaymentAuthorization_PaymentAccepted             | 2                           | Log-backed failure hotspot; isolates provider and currency rules from UI                |
| Order creation after successful payment          | T1_OrderManagement_SuccessfulPayment_OrderCreatedSuccessfully    | 3                           | Protects fulfillment and support from “paid but not recorded” defects                   |
| Customer login                                   | T1_Authentication_ValidCustomerLogin_LoginSuccessful             | 4                           | Prerequisite for growth features; flanked by auth library and hashing work              |
| Login step inside checkout (guest to logged-in)  | T1_Checkout_LoginDuringCheckout_ProceedToCheckoutContinuesFlow   | 5                           | US4700; reduces abandonment on Czech path and future markets                            |
| Payment callback handling                        | T5_Payment_InvalidPaymentCallback_RequestRejected                | 6                           | Security and reconciliation; correlates with invalid callback and timeout noise in logs |
| EUR high-amount payment rules                    | T5_Checkout_EURHighAmountPolicy_EnforcedOrDocumentedConsistently | 7                           | Targets recurring PAY-VAL-051 class failures before USD/CNY policies add complexity     |
| SQL injection on authentication API              | T5_Authentication_SQLInjection_LoginRejected                     | 8                           | Critical OWASP-class defect already found; must stay locked with CI                     |
| API broken object level authorization            | T5_API_CrossUserResourceAccess_RequestForbidden                  | 9                           | Prevents cross-customer data exposure as traffic and markets grow                       |
| Admin-only destructive API operations            | T5_AdminDestructiveOperation_WithoutAdminRole_Returns403         | 10                          | Protects catalog and operations as tooling and integrations expand                      |


