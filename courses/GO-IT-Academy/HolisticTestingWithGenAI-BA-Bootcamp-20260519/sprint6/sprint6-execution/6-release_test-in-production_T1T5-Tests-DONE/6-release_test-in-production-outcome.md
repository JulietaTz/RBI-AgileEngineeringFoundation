**Title**

T1–T5 test design for production validation of PayU (ToolShop)

**Learning Objective**

After working through this transfer task, you can analyse payment-integration risks that were only partially observable outside production, name critical checkout and PayU situations, define production-safe validation scenarios, and document a structured T1–T5 scenario set aligned with US3100 and RBI T1T5 naming rules.

**Background**

US3100 adds PayU Czech Republic for Czech customers: local instruments, secure processing, status sync, CZK where supported, redirect to PayU, order creation on success, clear failure handling, and no storage of sensitive payment data in the webshop. In lower environments a dummy gateway hid real redirects, bank-side behaviour, webhooks, latency, and fraud patterns. Production validation must therefore combine risk-based scope, controlled amounts or sandbox-like instruments where available, monitoring of errors and payment callbacks, and regression around checkout paths that still use other payment methods.

**Reference**

- Transfer task: `sprint6-execution/6-release_test-in-production/6-release_test-in-production-task.md`
- User story: `sprint6-input/US3100-support-payment-type-PayU.md`
- T1T5 method: `sprint6-execution/3-understand_use-atdd_designT1T5-DONE/T1T5-testdesign-description.md`

**Tasks**

### Part 1 — Risks, critical situations, and production validation focus

**Key risks (payment, checkout, errors, security, reliability, production behaviour)**

| Risk area | Risk | Why it matters in production |
|-----------|------|------------------------------|
| Payment processing | Wrong amount or currency sent to PayU | Dummy env may not enforce CZK and cent rounding like live contracts (ACC-03, ACC-04). |
| Payment processing | Double capture or duplicate order on retries | Real idempotency and PayU session handling differ from stubs (ACC-06, ACC-07). |
| Checkout integration | PayU shown to non-Czech customers | Region gating is business-critical and easy to misconfigure across caches and CDN (ACC-02). |
| Checkout integration | Broken redirect return or deep-link after PayU | TLS, hostnames, and session cookies behave differently at scale (ACC-05, ACC-06). |
| Error handling | Silent failure or generic page after cancel/fail | Trust and support load; must match ACC-07. |
| Error handling | Stuck “pending” when PayU status never arrives | Webhook and polling paths often only prove out live (ACC-08). |
| Security | Forged or replayed PayU callbacks | Misuse and integration bugs can mark orders paid without money movement (ACC-08, ACC-09, ACC-10). |
| Security | Sensitive card or token data persisted in shop DB | Violates ACC-09; logging must be checked in prod-like config. |
| Reliability | Timeouts and partial failures under load | Latency and queue backlogs differ from non-prod (ACC-08, operations). |
| Production behaviour | Monitoring blind spots | Alerts must cover PayU errors, callback failures, and order–payment mismatches. |

**Critical test situations (condensed)**

1. Czech billing/shipping: PayU visible, selectable, and completes end-to-end with correct CZK presentation.
2. Non-Czech address: PayU not offered; checkout still completes with another method (regression).
3. Successful PayU: redirect out, return, confirmation page, order persisted with expected status.
4. User cancels at PayU: returns to shop with clear message and cart/order state consistent with policy.
5. PayU declines or bank error: message, no false “paid”, retry path works without duplicate charge risk.
6. Late or duplicate webhooks: order transitions to paid once; idempotent handling verified via logs or read-only checks.
7. Session/browser closed during PayU: reopening shop shows consistent payment and order state after sync.
8. Amount tampering (cart changed server-side or client manipulation attempts): PayU session invalid or payment rejected.
9. TLS and redirect URL integrity: only expected hosts; no mixed content; secrets not in URLs.
10. Observability: metrics/logs for PayU initiation, return, callback errors, and order status mismatches.

**Production-relevant validation strategies (design-level)**

- Prefer **smoke plus sample instruments** on real PayU with **minimal real charges** or PayU-provided test merchants if available; otherwise **canary cohort** and **read-only reconciliation** (compare PayU dashboard to orders) after first live traffic.
- Run **after deployment** in a defined window with **rollback criteria** (spike in payment errors, callback failure rate, or order–payment divergence).
- Coordinate **monitoring dashboards and alerts** before go-live; include **regression** on non-PayU checkout in the same window.

**Team split (from task)** — each team owns one scenario type: Team A → T1, Team B → T2, Team C → T3, Team D → T4, Team E → T5. The table below gives one **exemplar row per type** so all types are covered in one reference outcome.

### Part 2 — T1–T5 test set for PayU in production

Naming follows T1T5 syntax: `<Tx>_<Feature>_<RequirementOrCondition>_<ExpectedResult>` (readable segments, requirement traceable to US3100 ACCs).

| T1T5 test case title | Topic | Priority |
|----------------------|-------|----------|
| T1_checkoutPayU_czechCustomerValidPayUFlowWithCZK_redirectSecure_paymentConfirmed_orderCreated | Payment processing; checkout integration; production behaviour | P0 — acceptance / first prod smoke |
| T2_checkoutPayU_czechCustomerAlternativePayULocalInstrument_sameOrderGoal_paymentCompletesAndSyncs | Payment processing; checkout integration | P1 — acceptance breadth |
| T3_checkoutPayU_paymentFailsOrUserCancelsThenRetriesSuccessfully_singlePaidOrder_clearUserMessaging | Error handling; reliability; payment processing | P0 — prod validation |
| T4_checkoutPayU_nonCzechBillingAddress_payUNotDisplayed_checkoutStillValidWithOtherMethods | Checkout integration; negative / gating | P0 — acceptance + regression |
| T5_checkoutPayU_forgedOrReplayedPayUCallback_payloadRejected_orderNotMarkedPaid_auditLogged | Security; misuse; reliability | P0 — security validation |

**Traceability (abbreviated)**

| US3100 ACC | Covered by (title prefix) |
|------------|---------------------------|
| ACC-01, ACC-05, ACC-06 | T1 |
| ACC-01, ACC-04 (breadth of instruments) | T2 |
| ACC-07, ACC-08 | T3 |
| ACC-02, regression with other methods | T4 |
| ACC-08, ACC-09, ACC-10 | T5 |

**Note:** Execute only with agreed **production test policy**, **data minimisation**, and **rollback/monitoring** readiness; this document is **test design only** for the ToolShop PayU integration.
