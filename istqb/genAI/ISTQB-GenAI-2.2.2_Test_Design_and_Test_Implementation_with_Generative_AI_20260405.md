### 1. Title
Design and Implement AI-Assisted Checkout Test Cases

---

### 2. Syllabus Reference
ISTQB GenAI – 2.2.2 Test Design and Test Implementation with Generative AI

---

### 3. Learning Objective
Use Generative AI to design concrete test cases and lightweight execution steps for a real webshop flow. The learner will practice turning feature information and observed system behavior into implementable tests with clear expected results.

---

### 4. Context / Scenario
You are the tester for the Toolshop webshop release and need quick, high-value coverage for the purchase path. The product owner asks for AI-supported test design for Search → Product Detail → Cart → Checkout before the next sprint review.

System under test: https://practicesoftwaretesting.com/

Supporting documentation:
- https://github.com/testsmith-io/practice-software-testing/tree/main/docs
- https://raw.githubusercontent.com/testsmith-io/practice-software-testing/main/docs/features.md

---

### 5. Task Instructions
1. Open the webshop home page and identify one product category (for example, Hand Tools) and one concrete product you can add to cart.
2. Open the feature documentation (`docs/features.md`) and confirm that Search, Filter, Cart, and Checkout Flow are listed as available functionality.
3. Use any Generative AI assistant you have access to and provide this prompt:
   - “Create 6 test cases for an e-commerce checkout flow (3 positive, 3 negative) for practicesoftwaretesting.com, covering search, product detail, cart quantity update, and checkout form validation. Return: Test ID, Preconditions, Test Steps, Test Data, Expected Result, Priority.”
4. Review the AI output and refine it manually so each test case is executable on the real webshop (remove vague steps, add concrete data and expected messages/pages).
5. Execute at least 2 of your refined test cases in the webshop (minimum: 1 positive and 1 negative).
6. Capture actual results and mark each executed test as Pass/Fail.
7. For one failed or weakly specified test, improve the AI prompt once and regenerate only that test case, then update your final version.

---

### 6. Expected Outcome / Deliverable
A single markdown artifact containing:
- 6 refined test cases (3 positive, 3 negative) with IDs
- Execution evidence for at least 2 test cases (actual result + Pass/Fail)
- 1 short comparison note (3–5 bullet points) describing how the regenerated test case improved after prompt refinement

---

### 7. Timebox
30 minutes

---

### 8. Hints (Optional)
- Use a logged-out user flow first to keep setup minimal.
- Keep test data explicit (search keyword, quantity, required checkout fields).
- If AI generates generic expected results, rewrite them as verifiable UI outcomes (e.g., “Cart badge increases to 1”, “Validation message shown for required field”).