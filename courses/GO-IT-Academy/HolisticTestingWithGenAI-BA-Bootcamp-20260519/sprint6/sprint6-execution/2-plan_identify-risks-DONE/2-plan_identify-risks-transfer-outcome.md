# Risk-Based Test Planning With GenAI - Outcome

This outcome captures the verbatim response from running
`2-plan_identify-risks-transfer-prompt.md` against a GenAI model.

Add the Model B response under its own section when you run the
prompt a second time with a different model, then write your own
comparison and final decision.

---

# Model A Response

## Step 1 - Risk Analysis

| User Story | Business Impact | Technical Complexity | Integration Risk | Security Relevance | Regression Potential | Overall Risk |
|---|---|---|---|---|---|---|
| US2300 - Czech Language Support | Direct customer trust impact for Czech market entry; broken or missing translations block the sprint goal of "Go to Czech market" | Medium - language switcher, i18n keys across UI, validation messages, fallbacks for missing keys | Low - mostly UI/content layer, but coupled to every page template | Low - no auth or payment surface changed | Medium - touches global UI templates; risk of regressions on other locales | High |
| US2350 - Provide Product Articles in Czech Language | Affects purchase decisions for Czech customers, but limited to product detail content quality | Low - mainly content authoring and CMS load | Low - no new system integrations | Low | Low - additive content, isolated from core flows | Medium |
| US2450 - Delivery Costs | Wrong shipping price directly affects revenue and customer trust (chargebacks, complaints) | Low - rule engine / price matrix with clear thresholds | Low - internal pricing module | Low | Medium - shipping price changes can collide with promotions, vouchers, or checkout totals | Medium |
| US3100 - Add PayU Payment Support for Czech Customers | Core to the Czech market sprint goal; payment failures stop revenue and damage trust | High - external provider, callbacks, status sync, currency, region gating | Very High - external PSP integration with asynchronous status updates and refunds | High - handles money flow, PCI scope, and PII; misconfiguration can leak transaction data or accept fraudulent orders | High - touches checkout, order persistence, and refunds | Very High |
| US3206 - MD5 Hashed Password | Affects all customer accounts; weakly chosen algorithm but still better than plain text | Medium - hashing on register, change, login, forgot, plus migration of existing passwords | Medium - data migration of legacy plain-text passwords | High in concept (auth) but MD5 is itself a weak hash; US3409 addresses more impactful auth risk | High - changes login flow and credential storage, risk of locking users out during migration | High |
| US3409 - Fix Vulnerability in Auth-X32 | Closes a CVSS 8.1 authentication vulnerability with potential session hijack on customer-facing portal | High - library replacement with concurrency and token-lifecycle changes | High - touches auth service, session management, refresh tokens, persistent login | Very High - direct fix for ISEC-2499; failure path is account takeover | Very High - regression risk across login, logout, session timeout, password reset, multi-device | Very High |
| US4700 - Login During Checkout Workflow | Recovers abandoned checkouts; direct conversion impact and visible to every guest user | Medium - adds modal/dialog into checkout state machine | Medium - couples checkout flow with authentication and session restore | Medium - introduces a new auth entry point that must respect the same security rules as the standard login | High - touches the most revenue-critical flow in the system | High |
| US5003 - Replace ORMapper | Data layer foundation; failure has no isolated blast radius - it affects everything that persists data | High - ORMapper version jump, schema migration, denormalization of ACCESS, performance target on full table scan | High - cross-cuts every persistence path, including order, account, product, and pricing data | Medium - data integrity and access control may shift under denormalization | Very High - broad regression surface, including the historic mapping bug that wrote into wrong DB fields | High |

## Step 2 - MoSCoW Prioritization

| User Story | MoSCoW Priority | Reasoning |
|---|---|---|
| US3100 - Add PayU Payment Support for Czech Customers | MUST | Sprint goal depends on it; failure blocks Czech revenue and exposes payment integration risk - the highest combined business + integration risk in the sprint. |
| US3409 - Fix Vulnerability in Auth-X32 | MUST | CVSS 8.1 authentication vulnerability with concrete exploitation path (session reuse, token bypass); regression in auth touches every logged-in flow and cannot ship unverified. |
| US4700 - Login During Checkout Workflow | SHOULD | Modifies the most revenue-critical flow and creates a new authentication entry point; high regression and conversion impact, but more contained than payments or auth library replacement. |
| US2300 - Czech Language Support | COULD | Visible to every Czech customer and tied to the sprint goal, but core risk is content correctness and UI regression rather than business-critical failure; targeted UI and i18n checks are enough. |
| US5003 - Replace ORMapper | COULD | Broad regression surface and known historic mapping bug, but lower direct customer visibility this sprint; selective regression on order, account, and pricing reads is sufficient if time-boxed. |
| US2350 - Provide Product Articles in Czech Language | WON'T | Mostly content authoring with low system risk; can be reviewed editorially outside the test plan without endangering the sprint goal. |
| US3206 - MD5 Hashed Password | WON'T | MD5 itself is a weak hash and not a long-term security win; US3409 covers the higher-impact authentication risk this sprint, so deep test investment here is poorly aligned. |
| US2450 - Delivery Costs | WON'T | Bounded business-rule change with clear thresholds; can be covered by a small price-matrix smoke check inside the checkout regression for US4700 instead of a dedicated testing slot. |

## Step 3 - Testing Focus

### US3100 - Add PayU Payment Support for Czech Customers (MUST)

- Main test objective: Confirm Czech customers can pay with PayU end-to-end, that payment status stays consistent between PayU and the webshop, and that the method is only offered in supported regions.
- Test ideas:
  - Successful PayU payment for a Czech customer in CZK on a desktop and mobile checkout.
  - Failed PayU payment (declined card / cancelled by user) and the order state in the webshop afterwards.
  - Asynchronous status callback delayed or duplicated by PayU - the order must not be paid twice or stuck in pending.
  - Region gating - non-Czech customer does not see PayU; Czech customer sees only supported PayU methods.
  - Refund or partial refund of a PayU order and reconciliation with order history.
- Test level: integration with a PayU sandbox account, plus an end-to-end checkout run; targeted security review on callback signature verification and storage of payment references.
- Data / environment: PayU sandbox credentials, Czech customer account, CZK-priced SKU, working webhook endpoint reachable from PayU sandbox, controlled clock for callback delay simulation.

### US3409 - Fix Vulnerability in Auth-X32 (MUST)

- Main test objective: Prove that xauth-X64 closes ISEC-2499, that no critical X2499 finding remains in the vulnerability scan, and that all authentication flows still behave correctly across concurrency and session refresh.
- Test ideas:
  - Re-run the vulnerability scan and confirm X2499 is not reported.
  - Concurrent login attempts from the same user on multiple devices - session state stays consistent and no token is reused across sessions.
  - Long-running session followed by refresh token rotation - old refresh token is invalidated and cannot be replayed.
  - Forced logout after deployment invalidates every existing session.
  - Negative test - attempt to reuse a partially invalidated session token (the previous ISEC-2499 vector) and confirm it is rejected.
- Test level: security regression plus integration tests on auth service; targeted exploratory testing around session lifecycle on macOS Tahoe clients.
- Data / environment: a system with xauth-X64 deployed, vulnerability scanner configured for X2499, at least one macOS Tahoe test client, multiple test accounts with persistent-login enabled.

### US4700 - Login During Checkout Workflow (SHOULD)

- Main test objective: Confirm that a guest user can authenticate inside the checkout flow and continue without losing cart context, while keeping the same security controls as the standard login.
- Test ideas:
  - Guest with items in cart clicks "Proceed to Checkout", logs in via the new dialog, and lands on the first checkout step with the cart intact.
  - Already-logged-in user clicks "Proceed to Checkout" and goes straight to the first checkout step without seeing the login dialog.
  - Failed login attempts in the checkout dialog enforce the same lockout / rate-limit rules as the main login page.
  - Login during checkout followed by US3100 (PayU payment) completes successfully - the new auth path does not break the payment integration.
  - Browser refresh or back navigation during the in-checkout login does not duplicate the order or lose the cart.
- Test level: end-to-end checkout test, plus a focused UI/UX exploratory pass; integration test on cart persistence across authentication.
- Data / environment: a guest cart with at least one in-stock SKU, an existing test account, a second account locked or with wrong credentials for negative cases, the standard checkout test environment.

## Step 4 - Critical Reflection

1. The hardest story to prioritize was US3206 (MD5 hashing). It is labeled as a security improvement but MD5 is itself cryptographically weak, so investing strong test effort there gives little long-term security value compared to US3409. It was placed in WON'T despite carrying the word "security" in the title - that is the kind of decision the test lead should consciously confirm.
2. The most likely underestimated risk is the regression surface of US5003 (Replace ORMapper). It is parked in COULD because customer visibility is lower, but the historic bug of writing into wrong database fields means a silent data-integrity regression here could only surface in production reporting, long after the sprint.
3. US3206 should be re-checked with stakeholders, specifically with the security officer. If compliance or audit reasons require MD5 to ship in this sprint, the priority must move at least to SHOULD, even though the algorithm itself is weak.
4. The biggest residual risk if only MUST and SHOULD stories receive strong test coverage is a silent data corruption from US5003 going to production with only ad-hoc checks - the ORMapper replacement combines a known historical mapping bug with a schema migration and denormalization of the ACCESS structure.

## Step 5 - Final Testing Recommendation

- Deep testing this sprint: US3100 (PayU Czech payments) and US3409 (Auth-X32 replacement). Both are sprint-blocking. Add focused end-to-end and regression coverage on US4700 (login during checkout) because it sits on the revenue path.
- Selective or lightweight testing: US2300 (Czech UI) with targeted i18n and visual checks, and US5003 (ORMapper replacement) with a curated regression set on order, account, and pricing reads plus the performance target on REF_ORG_Data.
- Out of testing scope for this sprint: US2350 (Czech product content - editorial review), US3206 (MD5 hashing - covered indirectly by US3409, revisit when migrating to a stronger algorithm), and US2450 (delivery costs - cover via a small price-matrix smoke check inside the US4700 checkout regression).
- Biggest remaining risk: a silent data-integrity regression introduced by US5003 reaching production because its broad blast radius was traded for tighter focus on payment and authentication this sprint.

---

# Model B Response

_Paste the verbatim response from your second GenAI model below this
line, then continue with the comparison section._

---

# Comparison And Final Decision

_After both model responses are captured, write:_

- _a comparison of MoSCoW choices, risk reasoning, and missed risks_
- _your own final testing recommendation_
- _one note on which model gave the more defensible reasoning and why_
