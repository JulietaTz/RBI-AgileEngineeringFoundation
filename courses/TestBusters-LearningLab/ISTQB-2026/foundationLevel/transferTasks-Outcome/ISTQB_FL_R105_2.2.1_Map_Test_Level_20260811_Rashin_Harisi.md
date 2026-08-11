**Two things to test on the checkout page:**

1. Verify that the billing address saved in the user account is correctly loaded on the checkout page.
2. Verify that card payment can be completed successfully and that the completed order/receipt appears in the user's account.

For the second test, I found this endpoint in the Sprint 5 Swagger documentation: `POST /payment/check`.
This endpoint checks the selected payment method and payment details during the checkout process.

---

| Test Level | Test object on Toolshop | Test objective | Who typically performs it | One example test |
|------------|-------------------------|----------------|--------------------------|------------------|
| Component Testing | billing address validation component  | verify that required address fields are validated correctly in isolation | developers | verify that an empty required address field is rejected. |
| Component Integration Testing | Interaction between the user account/address data and checkout | Verify that address data is correctly transferred to the checkout component. | developers / testers | verify that the billing address saved in the user account is correctly loaded at checkout |
| System Testing | Complete Toolshop checkout flow  | Verify that the complete purchase process works correctly from cart to order confirmation. | an independent test team | Add a product, complete checkout, pay by card, and verify that the order receipt appears in the account. |
| System Integration Testing | Integration between Toolshop checkout and the configured payment validation service | Verify that Toolshop sends the payment data correctly to the payment validation endpoint and handles the returned success or error response correctly. | testers | Configure a payment endpoint, submit valid credit-card details, and verify that a successful response allows the checkout to create the invoice and confirm the order. |
| Acceptance Testing | Complete Toolshop checkout from the user's perspective | Verify that the checkout process fulfills the user's needs and is ready for use. | intended users | Verify that a customer can purchase a product successfully and receive an order confirmation. |


**Note:** Acceptance testing can have different types and different objectives. In this table, I considered User Acceptance Testing (UAT). The objective is to verify that the complete checkout process meets the user's needs and works as expected from the user's point of view.

---

**Write one sentence explaining which test level you actually performed in step 1 and why the other levels need different environments or roles.**

In step 1, I performed end-to-end system testing of the checkout flow; the other test levels need different roles or environments because component testing is normally done by developers, component integration was not tested separately, system integration with the configured payment service requires a separate integration environment, and acceptance testing requires the user's or business perspective.