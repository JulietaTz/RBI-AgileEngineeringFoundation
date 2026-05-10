### 1. Title
Prioritize Automation vs Manual Testing for Toolshop Core Flows

---

### 2. Syllabus Reference
ISTQB TAE – 1.1.1 Explain the Advantages and Disadvantages of Test Automation

---

### 3. Learning Objective
You will apply the concept of test automation trade-offs by deciding which webshop tests should be automated now and which should remain manual. You will justify your decisions using clear advantages and disadvantages.

---

### 4. Context / Scenario
Your team is preparing a regression suite for the Toolshop webshop. Time is limited, so only the most valuable candidates can be automated in this sprint.

---

### 5. Task Instructions
1. Open https://practicesoftwaretesting.com/ and execute these three user journeys:
   - Search for a product and apply at least one filter.
   - Add an item to the cart and change its quantity.
   - Start checkout and proceed until authentication/account input is required.
2. Open https://github.com/testsmith-io/practice-software-testing/tree/main/docs and review:
   - `features.md` for feature scope.
   - `README.md` for environment/account details.
3. For each of the three journeys, write down:
   - One automation advantage (for example speed, repeatability, regression coverage).
   - One automation disadvantage (for example maintenance effort, fragile selectors, test-data dependency).
4. Build a decision matrix with columns:
   - Journey
   - Advantage
   - Disadvantage
   - Decision (Automate Now / Automate Later / Keep Manual)
5. Add one sentence per journey explaining why the final decision is appropriate.
6. Select exactly one journey as the top automation priority and provide a final rationale sentence focused on business value and feedback speed.

---

### 6. Expected Outcome / Deliverable
A markdown file named `TAE-1.1.1-automation-decision-matrix.md` containing:
- A 3-row decision matrix
- 3 short decision rationale sentences
- 1 final top-priority recommendation sentence

---

### 7. Timebox
30 minutes

---

### 8. Hints (Optional)
- Prioritize tests that are run often and are stable in UI behavior.
- Defer scenarios with high setup complexity or frequently changing UI.
- Keep decision criteria consistent across all three journeys.
