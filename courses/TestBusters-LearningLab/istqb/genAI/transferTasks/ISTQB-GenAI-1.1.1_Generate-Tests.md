# TRATAS – GenAI Test Case Generator: Cart & Checkout

## Goal
Use generative AI to create valuable and reusable test cases for the Cart and Checkout functionality of the ToolShop system, and critically evaluate the results.

---

## Context (System Under Test)
- ToolShop E-Commerce Platform: https://practicesoftwaretesting.com/
- Focus Areas:
  - Cart (Add, Update, Remove items)
  - Checkout (User flow, payment, validation)

---

## Task (≤ 30 minutes)

### Part 1 – Understand the Test Basis (5 min)
1. Open the ToolShop application
2. Perform the following actions:      
   - Add a product to the cart
   - Navigate to the checkout

**Document:**
- 2–3 key steps of the user flow

---

### Part 2 – Create a Prompt (5 min)

Use the following structured prompt:

<define the prompt - TestCase Title should have T1T5 syntax>

---

### Part 3 – Generate Test Cases (10 min)
1. Execute the prompt using a generative AI tool (e.g. ChatGPT)
2. Review the output
3. Select at least 5 useful test cases

---

### Part 4 – Evaluate Quality (10 min)

Evaluate the generated test cases:

- Are they realistic for ToolShop?
- Are important scenarios missing?
- Are any test cases too generic?

Enhance the result:
- Add at least 2 missing test cases that the AI did not generate

---

## Expected Outcome / Team Value

- Initial set of AI-generated test cases for Cart & Checkout
- Identification of:
  - Gaps in AI-generated tests
  - Typical weaknesses of generative AI
- Basis for:
  - Regression testing
  - Test automation

---

## Acceptance Criteria

- Cart and Checkout flow was explored manually
- A structured prompt was created and executed
- At least 5 generated test cases were selected
- Test cases include:
  - Preconditions
  - Test steps
  - Expected results
- A critical review of the AI output was performed
- At least 2 additional test cases were added manually
- Results are documented and shared with the team (e.g. Test Management Tool, Confluence)
