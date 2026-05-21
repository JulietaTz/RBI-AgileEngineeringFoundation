# Automate Search Functionality with T1–T5 Test Design

## Title
Automate Search Functionality Using a Full T1–T5 Test Set

---

## Syllabus Reference
ISTQB TAE – Test Design Techniques in Test Automation

---

## Learning Objective
Apply structured test design (T1–T5) to create a comprehensive and
automation-ready test set for a search functionality.

---

## Context / Scenario
You are automating tests for the webshop
https://practicesoftwaretesting.com/.

The search functionality is a critical feature for users to find
products. Your task is to design a complete T1–T5 test set and prepare
it for automation.

The goal is to ensure both broad coverage and efficient automation by
applying structured test design.

---

## Task Instructions

1. Explore the search functionality:
   - Open https://practicesoftwaretesting.com/
   - Use the search bar to test different inputs (e.g. product names,
     partial terms, invalid inputs)

2. Identify T1 (Happy Path):
   - Define 1–2 core scenarios where search works as expected
   - Example: valid product name returns correct results

3. Identify T2 (Alternative Paths):
   - Define variations of valid usage
   - Example:
     - Partial search terms
     - Case sensitivity
     - Multiple results

4. Identify T3 (Edge Cases):
   - Define boundary or unusual but valid inputs
   - Example:
     - Very short search terms
     - Very long search strings
     - Special characters

5. Identify T4 (Negative Tests):
   - Define invalid or unexpected inputs
   - Example:
     - Empty search
     - Non-existing product names
     - Random strings

6. Identify T5 (Error & Risk-Based Tests):
   - Focus on potential system weaknesses
   - Example:
     - Injection-like inputs
     - Performance-heavy queries
     - Repeated rapid searches

7. Convert your T1–T5 set into automation-ready test cases:
   - Define for each test:
     - Test name
     - Input data
     - Expected result

8. (Optional) Define how test data is handled:
   - Are static search terms sufficient?
   - Do you need dynamic data?

---

## Expected Outcome / Deliverable
A structured T1–T5 Test Set including:
- At least:
  - 1–2 T1 tests
  - 2–3 T2 tests
  - 2–3 T3 tests
  - 2–3 T4 tests
  - 1–2 T5 tests
- Each test defined with:
  - Test name
  - Input
  - Expected result

---

## Timebox
30 minutes

---

## Hints
- Think in coverage layers, not just individual tests
- Keep tests automation-friendly (clear inputs and expected outputs)
- Focus on user behavior and risks, not just functionality