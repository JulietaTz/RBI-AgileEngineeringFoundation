# Outcome — Sprint 6 T1&T5 (per `4-build:test-features-stories-prompt.md`)

**Source:** `sprint6/sprint6-input/sprint6-content.md`  
**Note:** No live environment was used; analysis and test titles only.

---

## Part 1 — Analysis

### Functionality that requires acceptance testing

- **US2300** — Language switcher; Czech UI, navigation, product text where applicable; Czech validation and error messages.
- **US2350** — Czech-language product articles for selected items/categories.
- **US2450** — Shipping logic: free standard over €75 (excluding oversized), €5.95 under threshold, €20 heavy/oversized surcharge, €12.95 express for in-stock only.
- **US3100** — PayU selectable in checkout for supported regions; secure processing; payment status sync; CZK where applicable; no sensitive payment storage in shop (NFR).
- **US3206** — Passwords stored as MD5 hashes; hashing on register / profile / login password change / forgot password; bulk conversion of existing plaintext passwords.
- **US3409** — Auth library migration (xauth-X32 → xauth-X64); scan no longer reports critical X2499 for old library (per story ACC).
- **US4700** — Guest: “Proceed to Checkout” opens login; after successful login, first checkout step shown. Logged-in user: proceeds directly to first checkout step.
- **US5003** — ORMapper 2.0: full migration to new schema; ACCESS denormalized; REF_ORG_Data full scan under 1.5s in PERF-ENV-Cloud-098.

### Existing functionality that requires regression testing

- Login, logout, session timeout, refresh, multi-device sessions (**US3409**, **US3206**).
- Registration, profile, password reset and change flows (**US3206**).
- Cart persistence, catalog browse/search/detail (localization and **US5003** can affect data shown).
- Non-PayU payment methods and non-Czech regions (**US3100** must not break others).
- Order confirmation, order history, admin or reporting reads that depend on migrated schema (**US5003**).
- Logged-in checkout path and any saved addresses/payment preferences tied to auth.

### Most critical business processes

1. **End-to-end purchase** — cart → checkout (incl. **US4700**) → shipping (**US2450**) → PayU (**US3100**) → confirmed order.
2. **Reliable authentication** — returning customers after password migration and new auth stack (**US3206**, **US3409**).
3. **Trustworthy catalog and totals** — Czech content (**US2300**, **US2350**) and correct persisted data (**US5003**).

### Prioritized risks

| Level | Rationale |
|-------|-----------|
| **High** | Checkout blocked or cart lost for guests after **US4700**; PayU/order state desync (**US3100**); session hijack or auth instability after **US3409**; wrong financial or customer data after **US5003**. |
| **Medium** | Incomplete Czech strings or wrong regional display of PayU; edge cases on migrated passwords (**US3206**); shipping rule mistakes (**US2450**). |
| **Low** | Cosmetic localization issues; minor copy or layout glitches. |

---

## Part 2 — Selected use-case

A **Czech-region customer** adds items to the cart, chooses **PayU** at payment time, and completes checkout. The path must honor **US3100** (region-gated PayU, secure flow, status sync), **US2450** (shipping options and surcharges), and **US2300/US2350** (Czech UI and product copy). **US4700** applies if the shopper starts as a guest and must authenticate before payment.

---

## Part 2 — T1&T5 test set

| Scope | Risk | Covered User Story | T1&T5 Title Syntax |
|------|------|---------------------|---------------------|
| Acceptance Testing | High | US3100, US2450 | T1_Checkout_CzechRegionPayUSelected_OrderCompletesWithSynchronizedPaymentStatus |
| Acceptance Testing | High | US2300, US2350, US3100 | T1_Checkout_CzechLocaleAndLocalizedProduct_PayUFlowShowsCzechContent |
| Regression Testing | High | US3100 | T2_Checkout_NonCzechRegion_PayUNotOfferedExistingCardFlowStillCompletes |
| Regression Testing | Medium | US2450, US5003 | T2_Checkout_OrderOver75EuroExcludingOversized_FreeStandardShippingApplied |
| Regression Testing | Medium | US3100 | T3_Checkout_PayUProviderTimeout_UserSeesRecoverableErrorAndOrderNotMarkedPaid |
| Regression Testing | Medium | US4700, US3100 | T3_Checkout_GuestLogsInMidFlow_PayURemainsSelectableAndCartIntact |
| Acceptance Testing | Medium | US3100 | T4_Checkout_UnsupportedRegionPayURetry_PaymentRejectedOrHidden |
| Regression Testing | High | US3100 | T5_Checkout_ForgedPayUCallbackWithoutValidSignature_OrderNotMarkedPaid |
