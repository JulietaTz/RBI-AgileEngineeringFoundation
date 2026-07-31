## AI-Assisted Test Design for the Checkout Flow

---

## 2. Syllabus Reference
ISTQB GenAI – 2.2.2 Test Design and Test Implementation with Generative AI


## Link to the Transfer Task File
https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks/Chapter2/ISTQB-GenAI-2.2.2_Test_Design_and_Test_Implementation_with_Generative_AI_20260405.md


# Outcome
# 1. Feature Verification

The `features.md` documentation was reviewed to verify the required functionality.

| Required Functionality | Documentation Entry | Status |
|------------------------|---------------------|--------|
| Search | Product Overview → Search | Confirmed |
| Filter | Product Overview → Filter | Confirmed |
| Cart | Not listed as a separate feature; covered by Checkout Flow → Increase/decrease quantity and Remove item | Confirmed indirectly |
| Checkout Flow | Checkout Flow | Confirmed |

The shopping cart is not listed as an independent feature in the documentation. However, cart-related functionality is included under **Checkout Flow**, particularly through the features for changing the item quantity and removing items.

---

# 2. AI-Generated Test Cases

## Positive Test Cases

### TC_CHECKOUT_001 – Find a Product via Search and Add It to the Cart

| Field | Content |
|------|---------|
| **Test ID** | TC_CHECKOUT_001 |
| **Test Type** | Positive |
| **Preconditions** | The website is available. An in-stock product exists. The shopping cart is empty. |
| **Test Data** | Search term: Hammer |
| **Test Steps** | 1. Open the Practice Software Testing homepage.<br>2. Enter **Hammer** in the search field.<br>3. Execute the search.<br>4. Select a matching product from the search results.<br>5. Verify the product name, price, and availability on the product detail page.<br>6. Click **Add to cart**.<br>7. Open the shopping cart. |
| **Expected Result** | The search results contain matching products. The product detail page is displayed correctly. The selected product is added to the cart once with the correct name and price. |
| **Priority** | High |

---

### TC_CHECKOUT_002 – Successfully Increase the Cart Quantity

| Field | Content |
|------|---------|
| **Test ID** | TC_CHECKOUT_002 |
| **Test Type** | Positive |
| **Preconditions** | A product with quantity **1** is already in the shopping cart. At least two items are available in stock. |
| **Test Data** | Initial quantity: 1<br>New quantity: 2 |
| **Test Steps** | 1. Open the shopping cart.<br>2. Verify the current product quantity.<br>3. Increase the quantity from **1** to **2**.<br>4. Update the shopping cart if this is not done automatically.<br>5. Verify the unit price, quantity, and total price. |
| **Expected Result** | The product quantity is updated to **2**. The total price equals twice the unit price. No error message is displayed. |
| **Priority** | High |

---

### TC_CHECKOUT_003 – Complete Checkout with Valid Address and Payment Details

| Field | Content |
|------|---------|
| **Test ID** | TC_CHECKOUT_003 |
| **Test Type** | Positive |
| **Preconditions** | A product is in the shopping cart. The user is logged in or can log in during checkout. |
| **Test Data** | Email: testuser@example.com<br>Password: valid password<br>Street: Teststraße 10<br>City: Vienna<br>State: Vienna<br>Country: Austria<br>Postal Code: 1020<br>Payment Method: Cash on Delivery |
| **Test Steps** | 1. Open the shopping cart.<br>2. Click **Proceed to checkout**.<br>3. Log in with valid credentials if required.<br>4. Fill in the checkout form with valid address data.<br>5. Continue to the next checkout step.<br>6. Select a valid payment method.<br>7. Confirm the order. |
| **Expected Result** | All valid data is accepted. The user can complete all checkout steps successfully. The order is created successfully and an order confirmation is displayed. |
| **Priority** | Critical |

---

## Negative Test Cases

### TC_CHECKOUT_004 – Search for a Non-Existing Product

| Field | Content |
|------|---------|
| **Test ID** | TC_CHECKOUT_004 |
| **Test Type** | Negative |
| **Preconditions** | The website is available. |
| **Test Data** | Search term: XYZ123NichtVorhanden |
| **Test Steps** | 1. Open the homepage.<br>2. Enter **XYZ123NichtVorhanden** in the search field.<br>3. Execute the search.<br>4. Verify the results area. |
| **Expected Result** | No unrelated products are displayed. The user receives a clear message indicating that no products were found. The page remains fully functional. |
| **Priority** | Medium |

---

### TC_CHECKOUT_005 – Increase Cart Quantity Beyond Available Stock

| Field | Content |
|------|---------|
| **Test ID** | TC_CHECKOUT_005 |
| **Test Type** | Negative |
| **Preconditions** | A product with limited stock is already in the shopping cart. |
| **Test Data** | Quantity exceeding the available stock |
| **Test Steps** | 1. Open the shopping cart.<br>2. Increase the product quantity step by step until no further increase is possible.<br>3. Attempt to increase the quantity once more.<br>4. Verify the displayed quantity, any validation message, and the total price. |
| **Expected Result** | The quantity cannot be increased beyond the available stock. The system keeps the last valid quantity or resets the invalid input. The total price is calculated only for the valid quantity. If a validation message is implemented, it is displayed. |
| **Priority** | High |

---

### TC_CHECKOUT_006 – Submit the Checkout Form with Missing Required Fields

| Field | Content |
|------|---------|
| **Test ID** | TC_CHECKOUT_006 |
| **Test Type** | Negative |
| **Preconditions** | A product is in the shopping cart. The user has started the checkout process. |
| **Test Data** | Street: empty<br>City: Vienna<br>Country: not selected<br>Postcode: empty |
| **Test Steps** | 1. Navigate to the checkout form.<br>2. Leave the **Street** field empty.<br>3. Enter **Vienna** in the **City** field.<br>4. Do not select a country.<br>5. Leave the **Postcode** field empty.<br>6. Click **To the cash register**. |
| **Expected Result** | The user cannot proceed to the next checkout step. All missing required fields are clearly highlighted. A validation message is displayed for each invalid or empty required field. Previously entered valid data remains unchanged. |
| **Priority** | Critical |

---

# 3. Review and Refinement of AI-Generated Test Cases

| Test Case | Review Findings | Changes Made |
|-----------|-----------------|--------------|
| TC001 | Already executable | No changes required |
| TC002 | The quantity update mechanism was too generic. | Updated according to the actual web shop behavior (press **Enter** or click outside the input field). |
| TC003 | The checkout flow did not fully match the application. | Updated the credentials, UI labels, and test steps to reflect the actual checkout flow. |
| TC004 | Already executable | No changes required |
| TC005 | Assumed that the available stock quantity was visible. | Removed the hardcoded stock value and made the test executable. |
| TC006 | Generic field and button names. | Updated with the actual UI labels ("Postcode", "To the cash register"). |

---

# 4. Final Refined Test Cases

## TC_CHECKOUT_001 – Find a Product via Search and Add It to the Cart

| Field | Content |
|------|---------|
| **Test ID** | TC_CHECKOUT_001 |
| **Test Type** | Positive |
| **Preconditions** | The website is available. An available product exists in the system. The shopping cart is empty. |
| **Test Data** | Search term: **Hammer** |
| **Test Steps** | 1. Open the Practice Software Testing homepage.<br>2. Enter **Hammer** in the search field.<br>3. Execute the search.<br>4. Select the product **"Hammer"** from the search results.<br>5. Verify the product name, price, and availability on the product detail page.<br>6. Click **Add to cart**.<br>7. Open the shopping cart. |
| **Expected Result** | The search results contain the product **"Hammer"**. The product detail page is displayed correctly. The product is added to the shopping cart once with the correct name and price. |
| **Priority** | High |

---

## TC_CHECKOUT_002 – Successfully Increase the Shopping Cart Quantity

| Field | Content |
|------|---------|
| **Test ID** | TC_CHECKOUT_002 |
| **Test Type** | Positive |
| **Preconditions** | An available product with a quantity of **1** is already in the shopping cart. At least two items are available in stock. |
| **Test Data** | Initial quantity: **1**<br>New quantity: **2** |
| **Test Steps** | 1. Open the shopping cart.<br>2. Verify the current product quantity.<br>3. Change the quantity from **1** to **2** and confirm the change by pressing **Enter** or clicking outside the input field.<br>4. Verify the unit price, quantity, and total price. |
| **Expected Result** | The product quantity is updated to **2**. The total price equals twice the unit price. No error message is displayed. |
| **Priority** | High |

---

## TC_CHECKOUT_003 – Complete the Checkout with Valid Address and Payment Details

| Field | Content |
|------|---------|
| **Test ID** | TC_CHECKOUT_003 |
| **Test Type** | Positive |
| **Preconditions** | A product is in the shopping cart. The user is logged in or can log in during the checkout process. |
| **Test Data** | Email: **customer@practicesoftwaretesting.com**<br>Password: **welcome01**<br>Street: **Test Street 98**<br>City: **Vienna**<br>Country: **Austria**<br>Payment Method: **Cash on Delivery** |
| **Test Steps** | 1. Open the shopping cart.<br>2. Click **"To the cash register"**.<br>3. If required, log in using valid credentials.<br>4. After a successful login, click **"To the cash register"** again.<br>5. Complete the **Billing Address** form with valid address information.<br>6. Select **"Cash on Delivery"** as the payment method.<br>7. Confirm the order. |
| **Expected Result** | All valid data is accepted. The user can complete all checkout steps successfully. The order is placed successfully, and an order confirmation is displayed. |
| **Priority** | Critical |

---

## TC_CHECKOUT_004 – Search for a Non-Existing Product

**No changes required.**

---

## TC_CHECKOUT_005 – Increase the Shopping Cart Quantity Beyond the Available Stock

| Field | Content |
|------|---------|
| **Test ID** | TC_CHECKOUT_005 |
| **Test Type** | Negative |
| **Preconditions** | An available product with limited stock is already in the shopping cart with a quantity of **1**. |
| **Test Data** | A quantity that exceeds the available stock |
| **Test Steps** | 1. Open the shopping cart.<br>2. Increase the product quantity step by step until the system no longer allows further increases.<br>3. Attempt to increase the quantity once more.<br>4. Verify the displayed quantity, any validation message, and the total price. |
| **Expected Result** | The product quantity cannot be increased beyond the available stock. The system keeps the last valid quantity or resets the invalid input. The total price is calculated only for the valid quantity. If a validation message is implemented, it is displayed. |
| **Priority** | High |

> **Note:** The available stock quantity is not visible in the application.

---

## TC_CHECKOUT_006 – Submit the Checkout Form with Missing Required Fields

| Field | Content |
|------|---------|
| **Test ID** | TC_CHECKOUT_006 |
| **Test Type** | Negative |
| **Preconditions** | A product is in the shopping cart. The user has started the checkout process. |
| **Test Data** | Street: empty<br>City: Vienna<br>Country: not selected<br>Postcode: empty |
| **Test Steps** | 1. Navigate to the checkout form.<br>2. Leave the **Street** field empty.<br>3. Enter **Vienna** in the **City** field.<br>4. Do not select a country.<br>5. Leave the **Postcode** field empty.<br>6. Click **"To the cash register"**. |
| **Expected Result** | The user cannot proceed to the next checkout step. All missing required fields are clearly highlighted. A validation message is displayed for each invalid or empty required field. Previously entered valid data remains unchanged. |
| **Priority** | Critical |

---

# 5. Execution

The following refined test cases were executed in the Practice Software Testing web shop.

### TC_CHECKOUT_002 - Successfully Increase the Shopping Cart Quantity

**Status:** Pass

**Actual Result:** The product quantity was successfully updated to **2**. The total price was calculated correctly, and no error message was displayed.

### TC_CHECKOUT_004 - Search for a Non-Existing Product

**Status:** Pass

**Actual Result:** The search for **"XYZ123NichtVorhanden"** returned **0 products**. The message **"No matching products were found."** was displayed. No unrelated products were shown, and the page remained fully functional.

---

# 6. Prompt Improvement

## Improved Prompt

> *Create six test cases for the checkout process of Practice Software Testing (three positive and three negative). Use only the existing functionality of the application. Cover product search, product details, shopping cart, quantity updates, and checkout validation. The test steps should be numbered and easy to follow. Use the original names of the buttons and input fields as they appear in the application.*

## Rationale

The prompt was refined to ensure that only existing application features and the original UI labels are used. This helps generate more realistic and consistent test cases.