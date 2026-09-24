# 2.2.3 - Apply Confirmation Testing and Regression Testing on Toolshop

## Reference to ISTQB Syllabus chapter

ISTQB FL – 2.2.3 Confirmation Testing and Regression Testing

## Link to the transfer task file

[ISTQB-FL-2.2.3 Confirmation Testing and Regression Testing](https://github.com/JulietaTz/RBI-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter%202/ISTQB-FL-2.2.3_Confirmation_Testing_and_Regression_Testing_20260405.md)

## Outcome

### Selected Feature Flow

**Product Detail / Shopping Cart – Product Quantity**

For this transfer task, the product quantity and shopping cart flow was selected.

The selected flow covers:

1. opening a product
2. selecting a product quantity
3. adding the product to the shopping cart
4. checking the transferred quantity
5. checking the cart price calculation

A known defect existed where unrealistic product quantities could be accepted without appropriate validation.

The defect is related to:

- Issue #793 – Shopping cart allows unrealistic product quantities without validation
- PR #810 – quantity validation fix
- Toolshop Sprint 5 implementation

The fix introduced a maximum allowed product quantity of:

`99`

---

## Test Environment

The manual tests were executed on the available Toolshop Holistic Testing environment.

Test object:

**Toolshop – Product Detail and Shopping Cart**

Test product:

**Pliers**

Product price:

**$12.01**

The Sprint 5 repository and related defect/fix information were used as the reference for the expected behaviour.

---

# Requirement / Expected Behaviour

The user should only be able to select a valid product quantity.

The maximum allowed quantity is:

`99`

If the user attempts to increase the quantity above `99`:

- the quantity must not exceed `99`
- the application must prevent the invalid quantity
- a warning message should be displayed

Expected warning:

> You can order at most 99 of this product.

Valid quantities must continue to work correctly after the fix.

For example:

- quantity `1` should be accepted
- quantity `2` should be accepted
- a valid quantity should be added to the shopping cart
- cart totals should be recalculated correctly

---

# Confirmation Testing

According to ISTQB, confirmation testing confirms that an original defect has been successfully fixed.

The confirmation test therefore focuses directly on the previously defective behaviour:

> Can the user still select a product quantity above the maximum of 99?

---

## CT-01 – Verify Maximum Product Quantity

| Field | Description |
|---|---|
| Test ID | CT-01 |
| Test Type | Confirmation Testing |
| Feature | Product Quantity |
| Area | Product Detail Page |
| Related Defect | Issue #793 |
| Priority | High |
| Maximum Quantity | `99` |

### Preconditions

- Toolshop is available.
- An available product is open.
- The product quantity control is visible.

### Test Steps

1. Open Toolshop.
2. Open the product **Pliers**.
3. Increase the product quantity using the `+` button.
4. Continue increasing the quantity until `99` is reached.
5. Try to increase the quantity once more above `99`.
6. Observe the displayed quantity.
7. Observe the system message.

### Expected Result

- Quantity `99` is accepted.
- A quantity above `99` must not be accepted.
- The displayed quantity must remain at `99`.
- A warning message must inform the user about the maximum allowed quantity.

Expected warning:

> You can order at most 99 of this product.

### Actual Result

- Quantity `99` was reached successfully.
- An attempt to increase the quantity above `99` was rejected.
- The displayed quantity remained at `99`.
- The following warning was displayed:

> You can order at most 99 of this product.

### Status

**PASS**

### Evidence

Manual test evidence shows:

- Product: **Pliers**
- Quantity displayed: `99`
- Further increase prevented
- Warning message displayed:
  **"You can order at most 99 of this product."**

### Quality / Risk Impact

The confirmation test shows that the tested quantity restriction works as expected.

The fix reduces the risk of:

- unrealistic order quantities
- invalid product quantities
- unrealistic cart totals
- invalid data being transferred to the shopping cart
- problems in later checkout processing

---

# Regression Testing

According to ISTQB, regression testing confirms that a change has not caused adverse consequences in previously working functionality.

After confirming the quantity limit, related functionality around the changed area was tested.

The main question for regression testing was:

> Does normal product quantity and shopping cart functionality still work after the quantity validation change?

---

# Impact Analysis

The quantity validation change can potentially affect several related parts of the Toolshop flow.

| Area | Possible Regression Risk |
|---|---|
| Product Detail | Valid quantities may no longer be selectable |
| Quantity Control | Normal quantity changes may stop working |
| Add to Cart | Products may no longer be added correctly |
| Shopping Cart | Selected quantity may not be transferred correctly |
| Price Calculation | Item total may not be recalculated correctly |
| Cart Total | Overall cart total may be incorrect |

Based on this impact analysis, a small regression subset was selected.

The regression subset focuses on:

1. adding a product with the normal default quantity `1`
2. using another valid quantity `2`
3. verifying the quantity and price calculations in the cart

---

# Regression Test Subset

## RT-01 – Add Product with Default Quantity 1

| Field | Description |
|---|---|
| Test ID | RT-01 |
| Test Type | Regression Testing |
| Feature | Add to Cart |
| Input | Quantity `1` |
| Priority | High |

### Preconditions

- Toolshop is available.
- Product **Pliers** is available.
- Product Detail page is open.

### Test Steps

1. Open the product **Pliers**.
2. Keep the default quantity at `1`.
3. Click **Add to cart**.
4. Observe the confirmation message.
5. Open the shopping cart.
6. Check the product, quantity and prices.

### Expected Result

- Quantity `1` is accepted.
- The product is successfully added to the cart.
- A confirmation message is displayed.
- The shopping cart contains **Pliers**.
- Quantity `1` is displayed.
- Product price and cart total are calculated correctly.

### Actual Result

The product was successfully added to the shopping cart with quantity `1`.

The application displayed the confirmation message:

> Product added to shopping cart.

The shopping cart displayed:

| Field | Actual Value |
|---|---:|
| Product | Pliers |
| Quantity | `1` |
| Unit Price | `$12.01` |
| Product Total | `$12.01` |
| Delivery Costs | `$7.90` |
| Overall Total | `$19.91` |

The selected quantity was transferred correctly to the cart and the displayed totals were correct.

### Status

**PASS**

### Evidence

Manual test evidence shows:

- quantity `1` selected
- successful **Add to cart**
- success notification displayed
- cart contains **Pliers**
- cart quantity is `1`
- product total is `$12.01`
- delivery costs are `$7.90`
- overall total is `$19.91`

### Quality / Risk Impact

The test confirms that the quantity validation change did not break the normal Add to Cart flow for the default quantity.

---

## RT-02 – Update to Valid Quantity 2

| Field | Description |
|---|---|
| Test ID | RT-02 |
| Test Type | Regression Testing |
| Feature | Shopping Cart Quantity |
| Input | Quantity `2` |
| Priority | High |

### Preconditions

- Product **Pliers** is in the shopping cart.
- The shopping cart is open.

### Test Steps

1. Open the shopping cart.
2. Change the quantity from `1` to `2`.
3. Observe the displayed quantity.
4. Check the product total.
5. Check the overall cart total.

### Expected Result

- Quantity `2` is accepted.
- No quantity validation error is displayed.
- The item total is recalculated for two products.
- Delivery costs remain correctly displayed.
- The overall cart total is recalculated correctly.
- The shopping cart remains functional.

### Actual Result

Quantity `2` was accepted successfully.

The shopping cart displayed:

| Field | Actual Value |
|---|---:|
| Product | Pliers |
| Quantity | `2` |
| Unit Price | `$12.01` |
| Product Total | `$24.02` |
| Delivery Costs | `$7.90` |
| Overall Total | `$31.92` |

No unexpected validation message was displayed.

The product total and overall cart total were recalculated correctly after changing the quantity.

### Status

**PASS**

### Evidence

Manual test evidence shows:

- quantity successfully changed to `2`
- quantity `2` remained accepted
- unit price remained `$12.01`
- product total changed to `$24.02`
- delivery costs remained `$7.90`
- overall total changed to `$31.92`

### Quality / Risk Impact

The test confirms that normal valid quantity changes continue to work after the quantity validation fix.

It also confirms that the cart calculation still reacts correctly to changes in quantity.

---

# Test Execution Overview

| Test ID | Test Type | Purpose | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|
| CT-01 | Confirmation | Attempt to exceed maximum quantity | Quantity remains at 99 and warning is displayed | Quantity remained 99 and warning was displayed | **PASS** |
| RT-01 | Regression | Add product with quantity 1 | Product added and cart data correct | Product added successfully with correct quantity and totals | **PASS** |
| RT-02 | Regression | Use valid quantity 2 | Quantity accepted and totals recalculated | Quantity 2 accepted and totals recalculated correctly | **PASS** |

---

# Test Result Summary

A total of **3 manual tests** were executed:

| Test Type | Executed | Passed | Failed |
|---|---:|---:|---:|
| Confirmation Testing | 1 | 1 | 0 |
| Regression Testing | 2 | 2 | 0 |
| **Total** | **3** | **3** | **0** |

All executed tests passed.

No regression was observed in the tested product quantity and shopping cart functionality.

---

# Confirmation Testing vs. Regression Testing

The Toolshop example demonstrates the difference between the two concepts clearly.

| Concept | Application in Toolshop |
|---|---|
| Confirmation Testing | Verify that a quantity above `99` can no longer be selected |
| Regression Testing | Verify that normal quantities such as `1` and `2` still work |
| Impact Analysis | Identify Product Detail, quantity controls, cart and price calculation as potentially affected areas |
| Regression Subset | Test important functionality close to the changed quantity validation |
| Test Evidence | Record actual quantities, messages and calculated cart totals |

---

# Confirmation Testing Result

The confirmation test directly targeted the previously defective behaviour.

Test flow:

`Attempt quantity > 99`

↓

`Quantity remains at 99`

↓

`Warning displayed`

↓

`PASS`

The result confirms that the tested defect area is working as expected.

---

# Regression Testing Result

After confirming the fix, nearby existing functionality was tested.

Regression flow:

`Select quantity 1`

↓

`Add product to cart`

↓

`Product successfully added`

↓

`Quantity 1 and totals correct`

↓

`Change quantity to 2`

↓

`Cart recalculates totals correctly`

↓

`PASS`

No adverse consequence of the quantity validation change was observed within the tested regression scope.

---

# Why These Regression Tests Were Selected

The regression tests were selected based on the impact analysis.

The fix affects product quantity handling.

Therefore, the closest related functionality has the highest regression risk:

`Product Detail`

↓

`Quantity Selection`

↓

`Add to Cart`

↓

`Shopping Cart`

↓

`Price Calculation`

Testing these areas provides confidence that the fix prevents invalid quantities without breaking normal shopping behaviour.

---

# Automation Consideration

According to ISTQB, regression testing is a strong candidate for automation because regression tests are executed repeatedly.

The tested Toolshop scenarios would be suitable for future automated regression tests.

Possible automated checks include:

- default quantity `1` is accepted
- quantity `2` is accepted
- quantity `99` is accepted
- quantity above `99` is rejected
- warning message is displayed for an invalid quantity
- valid quantity is transferred correctly to the cart
- product total is recalculated after a quantity change
- overall cart total is recalculated correctly

These tests could be executed automatically as part of a CI pipeline after future changes.

---

# Sprint-Ready Result

The known quantity validation area was tested using confirmation testing.

The application prevented the user from increasing the product quantity beyond `99` and displayed the expected warning message.

The confirmation test therefore passed.

A small regression subset was then executed around the changed functionality.

Normal quantity `1` could still be added successfully to the shopping cart.

Changing the quantity to `2` also worked correctly, and the product and overall cart totals were recalculated correctly.

All three executed tests passed.

No adverse consequences of the quantity validation fix were observed within the tested scope.

---

### Learning Summary

| Concept | One-line Takeaway |
|---|---|
| Confirmation Testing | Checks whether the original defect has been successfully fixed |
| Re-testing | The previously defective behaviour is executed again after the fix |
| Regression Testing | Checks whether the change has broken previously working functionality |
| Impact Analysis | Identifies which related areas could be affected by the change |
| Regression Subset | Focuses on the most relevant functionality around the change |
| Test Evidence | Actual results provide evidence that expected behaviour was observed |
| Automation | Repeated regression tests are strong candidates for automation |

> **Confirmation testing asks: "Is the defect fixed?" Regression testing asks: "Did the fix break anything else?"**