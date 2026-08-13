# Map Test Levels on Toolshop Checkout

## Reference to ISTQB Syllabus chapter
ISTQB FL – 2.2.1 Test Levels

## Link to the transfer task file
https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter%202/ISTQB-FL-2.2.1_Test_Levels_20260711.md

## Outcome

### Checkout Verification Points

1. Verify that the billing address saved in the user account is correctly loaded on the checkout page.
2. Verify that card payment can be completed successfully and that the completed order/receipt appears in the user's account.

### Checkout-related API Endpoints

For the selected checkout verification points, I found the following related endpoints in the Sprint 5 Swagger documentation:

1. `GET /users/me`  
   Returns the current user's information, including the saved address data used during checkout.

2. `POST /payment/check`  
   Checks the selected payment method and payment details during the checkout process.

### Test Levels Mapping

| Test Level | Test object on Toolshop | Test objective | Who typically performs it | One example test |
|------------|-------------------------|----------------|--------------------------|------------------|
| Component Testing | Billing address validation component  | Verify that required address fields are validated correctly in isolation | Developers | Verify that an empty required address field is rejected. |
| Component Integration Testing | Interaction between the user account/address data and checkout | Verify that address data is correctly transferred to the checkout component. | Developers / Testers | Verify that the billing address saved in the user account is correctly loaded at checkout. |
| System Testing | Complete Toolshop checkout flow  | Verify that the complete purchase process works correctly from cart to order confirmation. | An independent test team | Add a product, complete checkout, pay by card, and verify that the order receipt appears in the account. |
| System Integration Testing | Integration between Toolshop and an external or separately configured payment service | Verify that Toolshop exchanges payment data correctly with the external service and handles its response correctly. | Testers | Submit payment details through the configured external payment service and verify that a successful response allows checkout to continue. |
| Acceptance Testing | Complete Toolshop checkout from the user's perspective | Verify that the checkout process fulfills the user's needs and is ready for use. | Intended users | Verify that a customer can purchase a product successfully and receive an order confirmation. |

#### Note on Acceptance Testing

Acceptance testing can have different types and different objectives. In this table, I considered User Acceptance Testing (UAT). The objective is to verify that the complete checkout process meets the user's needs and works as expected from the user's point of view.

### Performed Test Level

In step 1, I performed end-to-end system testing of the checkout flow; the other test levels need different roles or environments because component testing is normally done by developers, component integration was not tested separately, system integration with the configured payment service requires a separate integration environment, and acceptance testing requires the user's or business perspective.

### Learning Summary

| Concept | One-line takeaway |
|---|---|
| Component Testing | Tests one component in isolation, usually by developers. |
| Component Integration Testing | Tests interactions between connected components. |
| System Testing | Tests the complete Toolshop checkout flow end to end. |
| System Integration Testing | Tests the interaction between Toolshop and another configured system or service. |
| Acceptance Testing | Confirms that the checkout flow meets user or business needs. |
| Test level selection | The same checkout feature can be tested at different levels depending on scope, objective, environment, and role. |