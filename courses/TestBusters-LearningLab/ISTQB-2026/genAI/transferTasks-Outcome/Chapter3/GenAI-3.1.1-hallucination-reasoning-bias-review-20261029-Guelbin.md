## Detect and Mitigate GenAI Hallucinations, Reasoning Errors, and Bias in Toolshop Test Preparation

---

### Syllabus Reference

ISTQB GenAI – 3.1.1 Hallucinations, Reasoning Errors and Biases in Generative AI

---

### Link to the Transfer Task File

https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks/Chapter3/ISTQB-GenAI-3.1.1_Hallucinations_Reasoning_Errors_and_Biases_in_Generative_AI_20260405.md

---

## OUTCOME

References:** [Toolshop webshop](https://practicesoftwaretesting.com/) and [documentation directory](https://github.com/testsmith-io/practice-software-testing/tree/main/docs).

### 1. GenAI Response

| No. | Test idea | User assumption and test approach | Expected behavior | Why high priority? |
| --- | --- | --- | --- | --- |
| 1 | Reliably find existing products | Users know a product name or part of it. Search for an existing product such as “Combination Pliers”; then vary partial terms, capitalization, and leading or trailing spaces. | The product is found. Capitalization and leading or trailing spaces do not affect the results. Opening a result displays the selected product with the corresponding information. | Products that cannot be found, or results linked to the wrong products, prevent purchases. |
| 2 | Correctly combine search and filters | Users narrow their search by category, brand, and budget. Combine a search term with compatible and conflicting filters; then remove individual filters. | Every result matches the search term and all active filters. Removed restrictions no longer affect the results. A combination with no matching products returns no results. | Incorrect restrictions hide products that could be purchased or lead to unsuitable results. |
| 3 | Continue searching after an unsuccessful search | Users try several terms. Enter a term with no results, then search for an existing product name and reset the search using “X”. | A clear message explains that no results were found. The next search works immediately. Resetting removes the search term; filters that remain active continue to apply, and their effect remains clear. | An unsuccessful search must not prevent users from continuing to shop. |
| 4 | Correctly add products to the cart as a guest | Users want to collect products before signing in. Add two different available products, add one of them again, then navigate between the product page and the cart and reload the page. | Product identity, unit prices, and quantities are correct. Adding the same product again increases the total quantity in an understandable way. The cart is retained within the same session. | Missing, additional, or lost items put the entire purchase at risk. |
| 5 | Correctly handle quantities and monetary amounts | Users order multiple units and may make typing mistakes. Change quantities; also try 0, negative values, decimal values, and quantities exceeding the available stock. | Valid quantities correctly update line totals and the cart total. For example, two units at $14.15 each total $28.30. Invalid quantities are handled clearly and do not cause incorrect amounts; stock limits are respected. | Quantity and calculation errors can directly lead to incorrect orders and payment amounts. |
| 6 | Completely remove items from the cart | Users change their purchase decisions. First remove one of several items, then remove the last item and reload the page. | Only the selected item is removed; totals and the cart indicator are updated. Removed items do not reappear after reloading. The empty cart is clearly recognizable and cannot be used to continue placing an order. | Unwanted items or outdated totals can lead to incorrect purchases. |

---

### 2. Review of GenAI Statements

| Type | GenAI statement | Evidence | Verdict |
| --- | --- | --- | --- |
| Reviewed behavior expectation — No. 1 | “Capitalization and leading or trailing spaces do not affect the results.” | In the manual tests performed, differences in capitalization and leading or trailing spaces did not affect the search results. | **Not an issue.** The statement was confirmed for the search queries tested. |
| Review for a reasoning error / wording issue — No. 2 | “Removed restrictions no longer affect the results.” | The results list sometimes failed to update when the parent category was deselected. This is a possible application deviation. The expectation that removed filters should no longer restrict results is logically reasonable. | **Not an issue regarding a GenAI reasoning error.** The wording can be clarified: after removing a filter, recalculate the results based on the remaining search conditions. |
| Potential hallucination / unsupported behavior expectation — No. 3 | “Resetting removes the search term; filters that remain active continue to apply, and their effect remains clear.” | In the manual test, “X” cleared both the search term and the category selection. The supplied Sprint 5 text describes resetting all filters when a new search is submitted, but does not explicitly define the function of “X”. | **Open.** Preservation of the filters is not supported by evidence. The wording is also ambiguous; a conclusive GenAI error has not yet been established. |
| Reviewed behavior expectation — No. 4 | “Adding the same product again increases the total quantity in an understandable way. The cart is retained within the same session.” | The manual tests showed that adding products as a guest, adding the same product again, and retaining the cart worked. A temporarily missing display after reloading was observed separately; the contents became visible again after navigating back to the cart. | **Not an issue for the expectations tested.** The temporarily missing display should be treated separately as a possible application deviation. |
| Potential hallucination / unsupported assumption — No. 5 | “Stock limits are respected.” | The actual stock quantity was not stated in the information reviewed. Entering 100 changed the quantity to 99. The message was: “You can order at most 99 of this product.” This establishes a maximum order quantity, but not its relationship to stock availability. | **Open.** An order quantity limit was observed; this does not establish that a stock-dependent check is performed. |
| Distinction between an application defect and a GenAI error — No. 5 | “Invalid quantities are handled clearly and do not cause incorrect amounts.” | Entering `1,5` (a decimal quantity of 1.5) triggered an integer validation error. In the manual test, even after pressing Enter, the line total remained calculated using 1.5 while the cart total remained unchanged. | **Not an issue regarding the GenAI test idea.** The expectation is reasonable; the test exposed a display and validation inconsistency in the application. |
| Reviewed behavior expectation — No. 6 | “Removed items do not reappear after reloading. The empty cart is clearly recognizable.” | Remaining items were displayed correctly after reloading; removed items did not reappear. After all items were removed, the message “Der Einkaufswagen ist leer. Nichts zum Anzeigen.” appeared (“The shopping cart is empty. Nothing to display.”). No “Zur Kasse” (“Checkout”) button was visible. | **Not an issue.** The expectations tested were confirmed. |

---

### 3. Corrected List of 6 Test Ideas

1. **Find existing products using search**

   Enter known product names and matching partial terms within the 3–40 character range described in the supplied Sprint 5 text, and submit the search. Vary capitalization and leading or trailing spaces.

   **Expected behavior:** Matching products are found. For the spelling and spacing variants tested so far, the results remain the same. The product page opened belongs to the selected product.

2. **Combine search with active filters**

   Submit a search query first, then apply category, brand, and price filters. Remove individual filters again.

   **Expected behavior:** The results match the search term and the currently active filter conditions. After a filter is removed, the list is recalculated using the remaining conditions. This sequence accounts for the supplied Sprint 5 text stating that a new search query resets all active filters.

3. **Search again after an unsuccessful search and reset the search**

   Submit a term known to return no results for the current test data. Then enter a known product name and submit another search. Afterwards, reset the search using “X”.

   **Expected behavior:** The unsuccessful search is clearly indicated, and the next search works. “X” removes the search term. In the manual test, it also cleared category selections. Preservation of those filters is not assumed because a clear requirement has not been established.

4. **Add products to the cart without signing in**

   Without signing in, add two different available products and add one of them again. Open product pages and the cart, and reload the page within the same session.

   **Expected behavior:** Product identity, unit prices, and quantities are correct. Adding the same product again increases its quantity accordingly. The cart contents are retained and displayed correctly.

5. **Check quantity validation, monetary amounts, and the observed order quantity limit**

   In the cart, enter valid quantities as well as `0`, negative values, and `1,5` (1.5), confirming each entry with Enter. For the tested “Pliers” product, also check the observed limit of 99 using the values 99 and 100.

   **Expected behavior:** Valid quantities produce correct line totals and cart totals. Invalid entries are corrected or clearly rejected; afterwards, the displayed totals must represent the same valid cart state. In the observed behavior, 100 is changed to 99. This establishes an order quantity limit; its relationship to the actual stock quantity remains unresolved.

6. **Remove individual items and all items from the cart**

   With two different products in the cart, remove one and reload the page. Then remove the remaining product and reload again.

   **Expected behavior:** Only selected items are removed; remaining items are retained. Totals and the cart indicator are updated accordingly. Removed items do not reappear. Once all items have been removed, the empty cart is explicitly indicated; in the tested state, no “Zur Kasse” (“Checkout”) button is visible.

---

### 4. Three Prompt-Improvement Bullets

The following additions can be appended to the original prompt:

- **Evidence and uncertainty:** Use the application and the [Toolshop documentation](https://github.com/testsmith-io/practice-software-testing/tree/main/docs) as references. Distinguish documented requirements, observed behavior, and assumptions that still need validation. Cite the corresponding evidence. If you cannot inspect the sources, state this explicitly.
- **Preconditions and clear terminology:** Keep the output at the level of test ideas, but state the essential preconditions and actions. In particular, distinguish submitting a new search from resetting it using “X”, and distinguish a maximum order quantity from actual stock availability. Identify required test data that is currently unknown.
- **Neutral user assumptions:** Express user assumptions as concrete, testable conditions, such as “not signed in”. Avoid unsupported generalizations about user groups, language, origin, or behavior. Justify any restriction to particular user groups when it is necessary for a test idea.

---

### 5. Limitations of the Review

The assessment is based on the manual tests performed and the materials reviewed. The actual stock quantity and a clear requirement for resetting the search using “X” could not be established. The corresponding behavior expectations therefore remained unresolved.

No conclusive reasoning errors or biases were established in the GenAI output reviewed. The number and distribution of flagged issues required by the task were not achieved with this output. Observed application deviations were documented separately from possible GenAI errors.
