# Transfer Task Outcome

## Title of the transfer task

Apply Confirmation Testing and Regression Testing on Toolshop

## Reference to ISTQB Syllabus chapter

ISTQB FL – 2.2.3 Confirmation Testing and Regression Testing

## Link to the transfer task file

[ISTQB-FL-2.2.3_Confirmation_Testing_and_Regression_Testing_20260405.md](https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter%202/ISTQB-FL-2.2.3_Confirmation_Testing_and_Regression_Testing_20260405.md)

## Outcome

### 1. Scope and objective

The selected feature is the **Shopping Cart** in the Toolshop web application.

Application: https://practicesoftwaretesting.com/

The objective is to verify that a previously fixed cart quantity defect has been resolved and that the existing add-to-cart functionality still works correctly.

The test scope consists of one confirmation test and one regression test.

### 2. Requirement and reference

**Behavior under test:** When a customer adds a product to the shopping cart, the selected product and its quantity should be displayed correctly, and the subtotal should correspond to the quantity and unit price.

**Documentation references:**
- [Toolshop documentation](https://github.com/testsmith-io/practice-software-testing/tree/main/docs)
- [Sprint 5 source code](https://github.com/testsmith-io/practice-software-testing/tree/main/sprint5)
- [Repository release notes](https://github.com/testsmith-io/practice-software-testing/releases)

The release notes mention a cart quantity fix in v2.0. The exact defect description and its applicability to Sprint 5 must be verified before treating it as the confirmed issue under test.

**Assumption:** The expected cart behavior is that the displayed quantity and subtotal remain consistent with the selected product and quantity.

### 3. Confirmation testing

**Test ID:** CT-01

**Test objective:** Verify that the previously reported cart quantity defect has been fixed.

**Preconditions:**
- The Toolshop application is available.
- The shopping cart is empty.
- An available product can be added to the cart.

**Test steps and expected results:**

| Step | Action | Expected result |
|---|---|---|
| 1 | Open the Toolshop application. | The application loads successfully. |
| 2 | Select an available product and add it to the cart. | The product is added to the cart. |
| 3 | Open the shopping cart. | The selected product is displayed. |
| 4 | Change the quantity from 1 to 2. | The displayed quantity becomes 2. |
| 5 | Check the subtotal. | The subtotal equals twice the unit price, subject to the application's pricing rules. |

**Test data:** One available product; quantity 2.

**Actual result:** To be recorded during test execution.

**Evidence:** Screenshot of the cart after the quantity change, showing the product, quantity, unit price, and subtotal where available.

**Status:** Not executed.

### 4. Regression testing

**Test ID:** RT-01

**Test objective:** Verify that the existing add-to-cart functionality continues to work after the quantity-related fix.

**Preconditions:**
- The Toolshop application is available.
- The shopping cart is empty.
- An available product can be added to the cart.

**Test steps and expected results:**

| Step | Action | Expected result |
|---|---|---|
| 1 | Open the Toolshop application. | The application loads successfully. |
| 2 | Select an available product. | The product details are displayed. |
| 3 | Add the product to the cart. | The product is added successfully. |
| 4 | Open the shopping cart. | The selected product is displayed with the correct initial quantity and subtotal. |

**Test data:** One available product; default quantity.

**Actual result:** To be recorded during test execution.

**Evidence:** Screenshot of the shopping cart showing the selected product and its quantity and subtotal where available.

**Status:** Not executed.

### 5. Test execution summary

| Test ID | Test type | Result |
|---|---|---|
| CT-01 | Confirmation testing | Not executed |
| RT-01 | Regression testing | Not executed |

A test can be marked **Pass** only after the observed result matches the expected result. If the result differs, record the actual behavior, attach evidence, and report the defect to the Scrum team.

### 6. Risk and quality impact

| Risk | Potential impact | Priority |
|---|---|---|
| Cart quantity defect remains unresolved | Customers may order an incorrect quantity. | High |
| Add-to-cart functionality is broken | Customers may be unable to add products to their orders. | High |
| Subtotal is incorrect | Customers may receive misleading price information. | High |

These are preliminary priorities based on potential business impact.

### 7. Recommendation to the Scrum team

Execute CT-01 to verify the specific fix, then execute RT-01 to confirm that the existing add-to-cart functionality remains operational.

Record the actual results and evidence before deciding whether the tested functionality is ready for release.

### Learning Summary

| Concept | One-line takeaway |
|---|---|
| Confirmation testing | Re-test the previously failing behavior to verify that the defect has been fixed. |
| Regression testing | Test previously working functionality to detect unintended side effects of a change. |
| Evidence | Record actual observations rather than assuming that the expected result occurred. |
| Risk-based testing | Prioritize failures that could affect orders, quantities, or pricing. |

**Conclusion:** Confirmation testing checks whether the specific cart quantity defect has been resolved, while regression testing checks whether the existing add-to-cart functionality continues to work after the change.
