### 1. Title
Automation Suitability Snapshot for Toolshop Checkout and Search

---

### 2. Syllabus Reference
ISTQB TAE – 1.1.1 Explain the Advantages and Disadvantages of Test Automation

---

### 3. Learning Objective
You will evaluate where test automation adds value and where it introduces limitations in a realistic webshop context. By the end, you will justify automation decisions for selected scenarios using ISTQB terminology.

---

### 4. Context / Scenario
You are the tester for a Toolshop sprint planning session. The team wants fast regression feedback but has limited automation capacity. You must propose what to automate first in the webshop and what should stay manual for now.

---

### 5. Task Instructions
1. Open the webshop at https://practicesoftwaretesting.com/ and quickly explore these flows:
   - Product search and filter on the catalog page
   - Add a product to cart and update quantity
   - Checkout path until the point where account/login details are required
2. Open the project documentation at https://github.com/testsmith-io/practice-software-testing/tree/main/docs and review:
   - `features.md` (to identify key user-facing capabilities)
   - `README.md` (to note available environments and default accounts)
3. Select exactly **3 candidate test scenarios** from your exploration (for example: search/filter behavior, cart quantity update, checkout validation).
4. Create a small decision table with one row per scenario and these columns:
   - Scenario
   - Potential automation advantages (e.g., repeatability, speed, regression coverage)
   - Potential automation disadvantages (e.g., maintenance effort, brittle UI locators, setup/data dependency)
   - Decision: Automate now / Later / Keep manual
5. For each scenario, write **one short justification sentence** that balances at least one advantage and one disadvantage.
6. Mark one scenario as your **highest-priority automation candidate** and add a final sentence explaining why it gives the best return for regression testing.

---

### 6. Expected Outcome / Deliverable
A one-page markdown artifact named `TAE-1.1.1-automation-tradeoff-analysis.md` containing:
- A 3-row decision table
- 3 short justification sentences (one per scenario)
- 1 final priority recommendation sentence

---

### 7. Timebox
30 minutes

---

### 8. Hints (Optional)
- Favor stable, frequently repeated flows when arguing for early automation.
- Be explicit about maintenance and test-data constraints when arguing against immediate automation.
- Use the same quality criteria across all three scenarios so your decisions stay comparable.
