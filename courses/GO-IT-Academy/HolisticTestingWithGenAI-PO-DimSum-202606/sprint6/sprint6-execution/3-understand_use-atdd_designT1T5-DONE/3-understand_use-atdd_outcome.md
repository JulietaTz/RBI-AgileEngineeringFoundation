## Title

T1T5 Test Design for Business Use Cases in ToolShop (Sprint 6)

## Learning Objective

Apply the T1T5 test design technique to define scenario-based acceptance tests for business use cases that are impacted by implemented Sprint 6 user stories.

## Background

The ToolShop team prepares Sprint 6 acceptance testing for Czech market enablement and checkout improvements.

Derived business use cases from the official ToolShop feature list:
- Authentication and account access
- Checkout and payment processing
- Product overview, filtering, and search

Three use cases influenced by implemented Sprint 6 user stories:
- Checkout with login during checkout flow (`US4700`)
- Checkout with PayU payment option for Czech customers (`US3100`)
- Product discovery with Czech language and localized product content (`US2300`, `US2350`)

## Reference

- `sprint6-input/sprint6-content.md`
- `sprint6-input/sprint6-sprintGoal.md`
- `sprint6-input/US4700-new-step-login-in-checkout-flow.md`
- `sprint6-input/US3100-support-payment-type-PayU.md`
- `sprint6-input/US2300-support-czech-language.md`
- `sprint6-input/US2350-czech-products.md`
- `3-understand_use-atdd/T1T5-testdesign-description.md`
- `3-understand_use-atdd/toolshop-official-feature-list.md`

## Tasks

Selected use case: Checkout with login during checkout flow (`US4700`)

- `T1_checkout_notLoggedIn_validCredentialsAtLoginStep_firstCheckoutStepIsDisplayed`
- `T2_checkout_notLoggedIn_existingSessionRestored_firstCheckoutStepIsDisplayed`
- `T3_checkout_notLoggedIn_firstLoginAttemptFails_secondAttemptSucceeds_firstCheckoutStepIsDisplayed`
- `T4_checkout_notLoggedIn_invalidCredentials_errorMessageIsDisplayedAndCheckoutDoesNotContinue`
- `T5_checkout_loginStep_SQLInjectionPayloadInEmail_errorMessageIsDisplayedAndCheckoutDoesNotContinue`