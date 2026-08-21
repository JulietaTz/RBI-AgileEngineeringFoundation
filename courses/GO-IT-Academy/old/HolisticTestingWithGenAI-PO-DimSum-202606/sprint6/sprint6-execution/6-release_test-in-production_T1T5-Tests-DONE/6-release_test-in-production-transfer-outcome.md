**Title**

T1–T5 test design for production validation of PayU on ToolShop

**Background**

US3100 integrates PayU Czech Republic into ToolShop checkout for Czech customers: local instruments, secure redirect, CZK where supported, payment status sync, order creation on success, clear failure handling, and no storage of sensitive payment data. Lower environments used a dummy PayU gateway, so real issuer behaviour, redirect edge cases, webhook latency, idempotency, fraud patterns, and production monitoring could not be fully exercised before release. This artefact applies risk-oriented T1–T5 test design for **safe production validation** and **post-deploy monitoring awareness**—test ideas and controls only, not unapproved live money experiments unless the facilitator defines an allowed sandbox.

**Reference**

- User story: `sprint6/sprint6-input/US3100-support-payment-type-PayU.md`
- T1T5 technique: `sprint6/sprint6-execution/3-understand_use-atdd_designT1T5-DONE/T1T5-testdesign-description.md`
- Transfer task: `6-release_test-in-production-task.md`

**Tasks**

### 1. Risks and production-specific unknowns

Dummy environments could not settle the following:

| Area | Risk / unknown | Why production matters |
|------|----------------|------------------------|
| Payment processing | Amount or currency mismatch (EUR vs CZK, rounding) | Live PayU contracts enforce cent-level rules (ACC-03, ACC-04). |
| Payment processing | Duplicate charge or duplicate order on retry | Real session and idempotency differ from stubs (ACC-06, ACC-07). |
| Checkout integration | PayU visible to non-Czech customers | Region gating across CDN, cache, and address validation (ACC-02). |
| Checkout integration | Broken redirect return URL or lost session after PayU | TLS, hostname, cookie domain, and mobile browser behaviour at scale (ACC-05, ACC-06). |
| Error handling | Generic or silent failure after cancel/decline | Customer trust and support load; must match ACC-07. |
| Error handling | Order stuck in pending when callback never arrives | Webhook and polling paths often prove out only live (ACC-08). |
| Security | Forged or replayed PayU callback marks order paid | Integration bugs or weak signature checks (ACC-08, ACC-09, ACC-10). |
| Security | Sensitive card/token data in shop DB or logs | Violates ACC-09; prod logging levels differ from lower envs. |
| Reliability | Timeouts and partial failures under peak load | Latency, queue backlog, and circuit-breaker behaviour differ from non-prod. |
| Production behaviour | Monitoring blind spots | Alerts must cover PayU initiation errors, callback failures, and order–payment mismatches. |

**Critical situations:** (1) Czech address → PayU visible and completes with correct CZK presentation; (2) non-Czech address → PayU hidden, other methods work (regression); (3) successful redirect out and back → confirmation and order created; (4) user cancels at PayU → clear message, consistent cart/order state; (5) decline or bank error → no false “paid”, safe retry; (6) late/duplicate webhooks → single paid transition; (7) browser closed mid-PayU → consistent state after sync; (8) amount tampering → session invalid or payment rejected; (9) redirect URL and TLS integrity; (10) observability for initiation, return, callbacks, and status mismatches.

**Production-safe validation framing:** smoke with minimal real charges or PayU test merchants if available; otherwise canary cohort plus read-only reconciliation (PayU dashboard vs orders); defined post-deploy window with rollback criteria; regression on non-PayU checkout in the same window; monitoring dashboards and alerts ready before go-live.

### 2. Prompt A (structured table, risk-first)

Used with a general-purpose chat model; explicit output shape and production constraints.

```
You are assisting a QA engineer designing production-safe validation for ToolShop (Practice Software Testing webshop).

Context — US3100 PayU Czech Republic:
- PayU only for Czech billing or shipping address (ACC-02).
- Redirect to PayU, CZK where supported, amount correct (ACC-03–05).
- On success: confirmation + order created (ACC-06).
- On fail/cancel: clear message, retry without duplicate charge (ACC-07).
- Status sync from PayU; no sensitive payment data stored (ACC-08–10).

Constraint: Lower env used a dummy gateway. Design TEST IDEAS for production validation only—controlled amounts, agreed rollback criteria, monitoring checks. Do NOT propose uncontrolled real-money experiments.

Using T1T5 naming syntax:
<Tx>_<Feature>_<RequirementOrCondition>_<ExpectedResult>

Produce a markdown table with columns:
| T1T5 test case title | Topic covered | Priority |

Requirements:
1. Exactly one row per type T1–T5 for checkout PayU.
2. T1 = standard happy path; T2 = alternative valid path to same goal; T3 = exception/recovery; T4 = negative/rejection; T5 = misuse/security.
3. Topics must span: payment processing, checkout integration, error handling, security, reliability, production behaviour.
4. Explicitly tag which rows support: acceptance of new integration, regression of impacted checkout, post-deployment validation.
5. Add a short “production safety note” column (one line per row): how to run safely in prod.

Trace each title to at least one US3100 ACC in a brief footnote list after the table.
```

### 3. Prompt B (exploratory, adversarial style)

Used with a different instruction style—security-tester persona, bullet output, no fixed table upfront.

```
Act as a production-readiness reviewer for a payment integration team.

System: ToolShop checkout. New feature: PayU for Czech customers (US3100). Non-prod had a fake PayU—assume webhooks, bank declines, and redirect cookies behave differently in production.

List T1T5 scenario IDEAS as bullet groups (T1 … T5). For each bullet:
- Proposed scenario title (T1T5 syntax)
- Primary risk being mitigated
- One observable pass/fail signal (UI, order status, log/metric name)
- “Safe in prod?” — yes/no + one guardrail (e.g. minimal amount, read-only callback replay in staging mirror, alert threshold only)

Also answer:
1. What regression on NON-PayU checkout must run in the same release window?
2. What monitoring/alerts should exist BEFORE the first prod PayU payment?
3. Name two scenarios Prompt A-style tables often MISS for payment integrations.

Do not write step-by-step click scripts. Focus on risk depth and production observability.
```

### 4. Consolidated design scope

The table below supports:

- **Acceptance:** T1, T2 — first prod smoke and breadth of valid PayU instruments.
- **Regression:** T4 — non-Czech gating plus implicit check that other payment methods still work in the same checkout flow during the release window.
- **Post-deployment validation:** T1 (immediate smoke), T3 (failure/recovery under live gateway), T5 (callback security), with monitoring signals called out in safety notes.

### 5. T1–T5 test set (full range, balanced mix)

Naming: `<Tx>_<Feature>_<RequirementOrCondition>_<ExpectedResult>`

| T1T5 test case title | Topic covered | Priority |
|----------------------|---------------|----------|
| T1_checkoutPayU_czechCustomerSelectsPayUWithCZKAmount_secureRedirectAndReturn_paymentConfirmed_orderCreated | Payment processing; checkout integration; production behaviour | P0 — acceptance / first prod smoke |
| T2_checkoutPayU_czechCustomerUsesAlternativePayULocalInstrument_samePurchaseGoal_paymentCompletesAndStatusSyncs | Payment processing; checkout integration | P1 — acceptance breadth |
| T3_checkoutPayU_paymentDeclinedOrUserCancelsThenRetriesWithSameCart_singlePaidOrder_clearUserMessaging | Error handling; reliability; payment processing | P0 — post-deploy validation |
| T4_checkoutPayU_nonCzechBillingAndShippingAddress_payUNotOffered_checkoutCompletesWithExistingPaymentMethod | Checkout integration; negative / gating; regression | P0 — acceptance + regression |
| T5_checkoutPayU_replayedOrTamperedPayUCallbackWithoutValidSignature_callbackRejected_orderNotMarkedPaid_securityEventLogged | Security; misuse; reliability | P0 — security validation |

| Title prefix | US3100 ACC |
|--------------|------------|
| T1 | ACC-01, ACC-03, ACC-04, ACC-05, ACC-06 |
| T2 | ACC-01, ACC-04 |
| T3 | ACC-07, ACC-08 |
| T4 | ACC-02; regression with other checkout payment methods |
| T5 | ACC-08, ACC-09, ACC-10 |

**Production safety notes (from Prompt A follow-up):**

| Title | Safe execution guardrail |
|-------|--------------------------|
| T1 | Single agreed test order; minimal CZK amount; verify in PayU merchant portal + order DB read-only. |
| T2 | Same as T1 with different instrument; one order per instrument per window. |
| T3 | Use PayU decline/cancel test path or facilitator-approved sandbox; confirm no duplicate order IDs in logs. |
| T4 | Non-Czech address only; complete purchase with card/invoice—no PayU redirect. |
| T5 | Replay captured callback in isolated staging mirror OR negative test with invalid signature in prod (no payment state change); audit log review only. |

**Monitoring before first prod PayU (from Prompt B):** alert on PayU redirect failures, callback 4xx/5xx rate, order–payment status mismatch count, spike in checkout abandonment on payment step; dashboard panel for PayU initiation vs completion ratio.

**Regression in same window (from Prompt B):** guest and logged-in checkout with card/invoice for DE/AT addresses; cart persistence through payment step; order confirmation email for non-PayU path.

### 6. Reflection — Prompt A vs Prompt B

| Aspect | Prompt A (structured table) | Prompt B (exploratory adversarial) |
|--------|----------------------------|-------------------------------------|
| **Risk depth** | Good ACC traceability and T1–T5 balance quickly | Surfaces monitoring prerequisites and “table misses” (e.g. callback replay, non-PayU regression) |
| **Production safety** | Explicit safety-note column forced guardrails per scenario | “Safe in prod?” per bullet led to clearer staging-vs-prod split for T5 |
| **Duplication** | Clean one-row-per-type set | Some T3/T5 ideas overlapped; needed dedup when merging |
| **Format** | Import-ready table; weak on observability names until column added | Bullets named metrics/alerts; manual merge into table |
| **Gaps** | Initially light on monitoring and regression pairing | Less consistent T1T5 naming until converted to table |
| **Next time** | Add “observable signal” and “regression pair” columns to Prompt A upfront | End Prompt B with “emit final markdown table with columns X/Y/Z” to avoid merge work |

**Takeaway:** Structured prompts deliver complete T1–T5 coverage and naming compliance fast; adversarial prompts improve production safety, monitoring readiness, and security/reliability angles. Running both and merging yields a release-ready design without treating production as a full duplicate of lower-env happy-path testing.
