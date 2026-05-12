# Exploratory Feature Discovery and T1&T5 Test Design

## Part 1 - Exploratory Feature Discovery

### Authentication
- User registration
- User login/logout
- Session handling
- Profile management

### Product Search and Catalog
- Product search
- Category filtering
- Brand filtering
- Product detail pages
- Product sorting

### Shopping Cart
- Add products to cart
- Remove products from cart
- Update product quantities
- Persist cart state

### Checkout and Payments
- Guest checkout
- Login during checkout
- Address handling
- Payment processing
- Order confirmation

#### Critical Business Processes
- Checkout completion
- Payment authorization
- Order creation

### Favorites / Wishlist
- Save favorite products
- Manage favorites

### Administration
- Product management
- Inventory management
- Category management

---

## Part 2 - Selected Use Case

### Use Case

Customer register for a account.

### Test Cases

- T1_REGISTRATION_withValidEMail_loginIsDisplyed

- T2_REGISTRATION_withGoogleAccount_loginIsDisplayed
- T2_REGISTRATION_withGithubAccount_loginIsDisplayed

- T3_REGISTRATION_withCorrecteTypeInPassword_loginIsDisplayed

- T4_REGISTRATION_withALreadyUsedEMail_errorMessageIsDisplayed
- T4_REGISTRATION_withMissingMandatoryFields_loginIsDisplayed
- T4_REGISTRATION_withExpiredConfirmationLink_loginIsDisplayed

- T5_REGISTRATION_withSQLInjection_errorMessageIsDisplayed