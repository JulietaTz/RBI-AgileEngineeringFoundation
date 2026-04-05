### 1. Title
Design Equivalence Partitions for Price Range and Search in the Toolshop

---

### 2. Syllabus Reference
ISTQB FL – 4.2.1 Equivalence Partitioning

---

### 3. Learning Objective
Apply equivalence partitioning to derive efficient, high-value test cases for webshop filtering behavior. Create valid and invalid input classes and execute representative tests against the real system.

---

### 4. Context / Scenario
You are preparing a fast smoke-level test design for product discovery in the Toolshop webshop before a sprint demo. The team wants confidence that users can find products reliably using search and price filtering.

System under test: https://practicesoftwaretesting.com/

Supporting documentation:
- https://github.com/testsmith-io/practice-software-testing/tree/main/docs
- https://raw.githubusercontent.com/testsmith-io/practice-software-testing/main/docs/features.md

---

### 5. Task Instructions
1. Open the webshop homepage and identify the `Search` field and `Price Range` filter.
2. Define equivalence partitions for **Search input** in a small table with at least 4 classes, for example:
   - Valid keyword (existing product term)
   - Valid keyword (non-existing product term)
   - Empty input
   - Special-character-only input
3. Define equivalence partitions for **Price Range** in a second table with at least 4 classes, for example:
   - Valid in-range selection
   - Lowest boundary selection
   - Highest boundary selection
   - Invalid/out-of-intent selection behavior (e.g., very narrow range with no matching products)
4. Select one representative value/test condition from each partition and create **6 concrete test cases** total (minimum 3 for Search, minimum 3 for Price Range).
5. Execute at least **4 test cases** on the webshop.
6. Record actual results and mark each executed test as Pass/Fail.
7. If one result is unclear, refine one partition definition so it becomes more precise and update the linked test case.

---

### 6. Expected Outcome / Deliverable
One markdown artifact containing:
- Partition table for Search input
- Partition table for Price Range
- 6 test cases derived from partitions (with ID, representative input, expected result)
- Execution log for at least 4 test cases (actual result + Pass/Fail)
- One short note explaining one partition refinement you made after execution

---

### 7. Timebox
30 minutes

---

### 8. Hints (Optional)
- Keep partitions mutually exclusive where possible.
- Use product names visible on the homepage to define realistic valid keywords.
- For expected results, describe observable UI behavior (product list changes, result count trend, or no-results state).