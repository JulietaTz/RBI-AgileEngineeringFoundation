### 1. Title
Apply Framework Layering to Toolshop Testware

---

### 2. Syllabus Reference
ISTQB TAE – 3.1.3 Apply Layering of Test Automation Frameworks

---

### 3. Learning Objective
Organize automated tests into clear layers to improve maintainability and separation of concerns.

---

### 4. Context / Scenario
The team’s scripts mix locators, business steps, and assertions in the same files.

---

### 5. Task Instructions
1. Define four layers: Test Cases, Business Actions, Page/API Adapters, Utilities.
2. Select one Toolshop scenario: search product and add item to cart.
3. Write pseudo-structure showing what logic belongs in each layer.
4. Move at least 5 example statements to the correct layer (e.g., selector lookup, cart assertion, retry helper).
5. Add one rule that prevents cross-layer violations.

---

### 6. Expected Outcome / Deliverable
A layered framework sketch with one mapped scenario and layer assignment examples.

---

### 7. Timebox
30 minutes

---

### 8. Hints (Optional)
Assertions usually belong in test/business layers; raw selector logic belongs in adapter layers.
