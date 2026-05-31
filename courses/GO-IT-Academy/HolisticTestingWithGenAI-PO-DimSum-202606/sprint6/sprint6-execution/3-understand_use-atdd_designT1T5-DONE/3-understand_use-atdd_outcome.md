# Outcome — T1T5 Test Design for Business Use Cases (Sprint 6, ToolShop)

**Sources:** `sprint6/sprint6-input/sprint6-content.md`, `sprint6/sprint6-input/sprint6-sprintGoal.md`, individual `US*.md` files, `T1T5-testdesign-description.md`, `toolshop-official-feature-list.md`.

**T1T5 definitions used:**


| Type | Meaning                                                |
| ---- | ------------------------------------------------------ |
| T1   | Standard case — main happy path                        |
| T2   | Alternative case — different valid path to same goal   |
| T3   | Exception case — deviation handled with recovery       |
| T4   | Negative case — invalid input/state rejected           |
| T5   | Misuse case — deliberate manipulation / security probe |


---

## Candidate business use cases (from official feature list)


| Business use case                 | Feature-list anchor                                                                |
| --------------------------------- | ---------------------------------------------------------------------------------- |
| Authentication and account access | Authentication → Login, Register, Forgot Password, 2FA, Social Login, Lock account |
| Checkout and payment processing   | Checkout Flow → quantity, address, payment options (basic/advanced)                |
| Customer account self-service     | Customer Account → profile, password, invoices, favorites                          |
| Product discovery (overview)      | Product Overview → pagination, filter, search, sort, price-range                   |
| Product detail and comparison     | Product Detail, Product Comparison                                                 |
| Localized shopping experience     | Multi-Language                                                                     |
| Delivery and fulfilment selection | Checkout Flow → Address details                                                    |
| Discount and geo-based pricing    | Discount → Geo-location                                                            |


---

## Five business use cases influenced by Sprint 6 stories


| #   | Business use case                                                    | Sprint stories             | Why influenced                                                                                                                                 |
| --- | -------------------------------------------------------------------- | -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **Checkout with login during checkout flow**                         | **US4700**, US3409, US3206 | US4700 adds a login step for guests at checkout; US3409 changes session handling (handoff risk); US3206 affects login after password migration |
| 2   | **Checkout with PayU payment for Czech customers**                   | **US3100**                 | US3100 adds PayU with region gating and payment callbacks                                                                                      |
| 3   | **Product discovery with Czech language and localized content**      | **US2300**, **US2350**     | US2300 adds Czech UI; US2350 adds Czech product copy on overview and PDP                                                                       |
| 4   | **Customer password change with strength policy and secure storage** | **US3206**                 | US3206 introduces MD5 storage, password policy, and strength indicator on change-password flows                                                |
| 5   | **Checkout delivery selection and delivery-cost calculation**        | **US2450**, **US3150**     | US2450 defines free/flat/surcharge/express rules; US3150 adds Zásilkovna as a delivery option in CZ journeys                                   |


**T1T5 distribution:** Use case 1 includes a full **T1–T5** set (one test per type). Use cases 2–5 add journey-specific scenarios; every type **T1–T5** appears at least once across the five use cases (see coverage check at end).

*Note:* US5003 (ORM replacement) is treated as a technical enabler—acceptance tests for the five customer-facing use cases above remain traceable without a separate ORM use case.

---

## Consolidated T1T5 test design


| Use case                                                         | T1T5 | Test idea                                                                    | Expected observable outcome                                                                 | Feature list                                       | User story     |
| ---------------------------------------------------------------- | ---- | ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | -------------------------------------------------- | -------------- |
| Checkout with login during checkout flow                         | T1   | Guest proceeds to checkout, logs in with valid credentials at new login step | Login dialog → valid credentials → first checkout step (address) visible; cart preserved    | Checkout Flow, Authentication → Login              | US4700         |
| Checkout with login during checkout flow                         | T2   | Guest with valid session proceeds to checkout                                | No login dialog → first checkout step displayed immediately                                 | Checkout Flow, Authentication → Login              | US4700, US3409 |
| Checkout with login during checkout flow                         | T3   | Guest fails login once, succeeds on retry                                    | Error message after wrong password → correct password → first checkout step; cart unchanged | Checkout Flow, Authentication → Lock account       | US4700, US3206 |
| Checkout with login during checkout flow                         | T4   | Guest enters invalid credentials at checkout login                           | Error message; user remains on login step; checkout steps not accessible                    | Checkout Flow, Authentication → Login              | US4700         |
| Checkout with login during checkout flow                         | T5   | SQL injection payload in email at checkout login                             | Validation/error message; checkout does not progress; no unsafe execution                   | Checkout Flow, Authentication → Login              | US4700, US3409 |
| Checkout with PayU payment for Czech customers                   | T1   | CZ customer selects PayU on payment step                                     | PayU visible → select PayU → payment flow starts → order pending payment                    | Checkout Flow → Payment options (advanced)         | US3100         |
| Checkout with PayU payment for Czech customers                   | T2   | CZ customer completes PayU via bank-transfer option                          | Return to shop → order confirmed paid                                                       | Checkout Flow → Payment options (advanced)         | US3100         |
| Checkout with PayU payment for Czech customers                   | T3   | PayU auth succeeds but callback is delayed                                   | UI shows pending payment; refresh does not duplicate charge                                 | Checkout Flow → Payment options (advanced)         | US3100         |
| Checkout with PayU payment for Czech customers                   | T4   | Non-CZ customer opens payment step                                           | PayU option absent; other payment methods available                                         | Checkout Flow → Payment options (advanced)         | US3100         |
| Product discovery with Czech language and localized content      | T1   | Visitor switches site language to Czech                                      | Nav labels and product titles on overview in Czech                                          | Multi-Language, Product Overview                   | US2300, US2350 |
| Product discovery with Czech language and localized content      | T2   | Czech locale active; filter by category                                      | Result cards show Czech names/descriptions where translated                                 | Product Overview → Filter, Multi-Language          | US2300, US2350 |
| Product discovery with Czech language and localized content      | T4   | Invalid locale code in URL                                                   | Default locale applied; page loads without server error                                     | Multi-Language                                     | US2300         |
| Customer password change with strength policy and secure storage | T1   | Logged-in customer changes password to policy-compliant value                | Success message; subsequent login works with new password                                   | Customer Account → Change password, Authentication | US3206         |
| Customer password change with strength policy and secure storage | T2   | Customer changes password during forgot-password workflow                    | Reset completes; login with new password succeeds                                           | Authentication → Forgot Password, Customer Account | US3206         |
| Customer password change with strength policy and secure storage | T4   | Customer submits password failing policy (e.g. no special char)              | Creation/change blocked; message names violated rule; strength indicator shows Weak         | Customer Account → Change password                 | US3206         |
| Customer password change with strength policy and secure storage | T5   | Attempt to view password via API response or UI field                        | Password not returned in plain text in API/UI (observable via devtools/network)             | Customer Account → Change password, Authentication | US3206         |
| Checkout delivery selection and delivery-cost calculation        | T1   | Cart over €75, standard delivery, no oversized items                         | Free standard shipping shown; order summary separates product and delivery totals           | Checkout Flow → Address details                    | US2450         |
| Checkout delivery selection and delivery-cost calculation        | T2   | CZ customer selects Zásilkovna pickup                                        | Zásilkovna option selectable; delivery line updates; checkout can continue                  | Checkout Flow → Address details                    | US3150, US2450 |
| Checkout delivery selection and delivery-cost calculation        | T3   | Customer adds oversized item after free shipping applied                     | Delivery cost recalculates; surcharge visible before order confirm                          | Checkout Flow → Address details                    | US2450         |
| Checkout delivery selection and delivery-cost calculation        | T4   | Shipping address in unsupported country                                      | Clear error; customer cannot complete unsupported destination                               | Checkout Flow → Address details                    | US2450         |
| Checkout with PayU payment for Czech customers                   | T5   | Tampered PayU webhook signature                                              | Order not marked paid; customer sees unpaid/pending state                                   | Checkout Flow → Payment options (advanced)         | US3100         |


**Test naming (T1T5 syntax, for automation backlog):**

- `T1_checkout_notLoggedIn_validCredentialsAtLoginStep_firstCheckoutStepIsDisplayed`
- `T1_checkout_czechCustomer_selectPayU_paymentFlowInitiatedAndOrderPending`
- `T1_productOverview_switchToCzech_navigationAndTitlesInCzech`
- `T1_customerAccount_changePassword_policyCompliant_successAndReLoginWorks`
- `T1_checkout_cartOver75_standardDelivery_freeShippingDisplayedInSummary`

**Coverage check:** T1 ✓ (×5 use cases) · T2 ✓ · T3 ✓ · T4 ✓ · T5 ✓ (T5 split: checkout login SQLi, PayU webhook tampering, password exposure probe).

**Czech golden path (chains use cases 3 → 5 → 1 → 2):**

1. Switch site to Czech (US2300).
2. Browse/filter; verify Czech product copy (US2350).
3. Add to cart → checkout → select Zásilkovna; verify delivery totals (US3150, US2450).
4. Login at checkout step (US4700).
5. Select PayU; complete payment (US3100).
6. Order confirmation in Czech.

---

## Optional live check — ToolShop v5 ([https://practicesoftwaretesting.com/](https://practicesoftwaretesting.com/))


| Test sampled                           | Expected (Sprint 6 design)     | Observed (live v5 demo, May 2026)                    | Mismatch note                                                         |
| -------------------------------------- | ------------------------------ | ---------------------------------------------------- | --------------------------------------------------------------------- |
| T1 product overview (English baseline) | Filter, search, sort available | Confirmed on public demo                             | Czech-specific tests not exercisable—no Czech switcher on public demo |
| T1 checkout delivery (free over €75)   | Free shipping when rules met   | Partially observable—totals depend on demo catalogue | Validate against sprint branch with US2450 data                       |
| T4 PayU region gating                  | PayU only for CZ               | PayU not on public demo                              | Expected—US3100 not in baseline v5 matrix                             |
| T4 checkout guest login step           | Login dialog at checkout       | Not verified on live site                            | US4700 is sprint work-in-progress                                     |


**Conclusion:** Baseline v5 supports product-discovery and generic checkout structure. Sprint 6 behaviour (Czech locale, PayU, checkout login, Zásilkovna, password policy) must be validated on the **sprint deployment**, not only the public training demo.

---

## Brief reflection (prompting and ATDD)

- **Critique-first** (before generate-first) reduced misclassified T2 scenarios (e.g. social login vs session restore at checkout).
- Reject **non-observable** rows (auth-library swap, MD5 in DB) unless tied to a customer-visible outcome (login, password change, session handoff).
- Next tightening: add **Given/When/Then** under each row and map each test to **one acceptance-criterion bullet** per user story before ATDD automation.

