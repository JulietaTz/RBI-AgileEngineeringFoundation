# 2.2.1 – Generate Risk-Based Test Conditions from Feature Information

## Reference to ISTQB Syllabus Chapter

**ISTQB GenAI – 2.2.1 Test Analysis with Generative AI**

## Link to the Transfer Task File

(https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks-Outcome/Chapter2/ISTQB-GenAI-2.2.1_Test_Analysis_with_Generative_AI_20260405_Serife.md)

---

# Outcome

## Test Basis

### User Story

**US2300 – Czech Language Support**

As a customer from the Czech Republic, I want to use the webshop in the Czech language so that I can understand products, navigation, and the checkout process more easily and trust the webshop during my purchase journey.

### Scope

The test basis consists of US2300 and its acceptance criteria. The objective is to provide Czech-language support across customer-facing webshop functionality, including navigation, product information, validation messages, and checkout.

---

## Prompt Used for GenAI

Generate risk-based test conditions for the provided user story and acceptance criteria.

For each test condition:

- Assign a risk level (High, Medium, Low).
- Assign a priority (P1, P2, P3).
- Reference the related acceptance criterion.
- State assumptions explicitly.

---

## GenAI-Assisted Test Analysis

Generative AI was used to analyze the user story and acceptance criteria and derive risk-based test conditions.

The generated output included:

- Test conditions
- Risk levels
- Priorities
- Assumptions
- Traceability references

### Assumptions

- Czech is configured as a supported webshop language.
- The webshop supports language switching.
- Customer-facing pages are within scope.
- Registration, login, profile management and checkout are available in the test environment.

---

## Human Verification and Refinement

The AI-generated test conditions were manually reviewed.

The review focused on:

- Traceability to acceptance criteria
- Verifiability of test conditions
- Duplicate conditions
- Coverage completeness
- Risk and priority assignments

### Review Results

- No non-verifiable test conditions were identified.
- No duplicate test conditions were identified.
- All test conditions remained traceable to the original acceptance criteria.
- All acceptance criteria were covered by the final test-condition set.

The final responsibility for validating and refining the AI-generated output remained with the tester.

---

## Final Risk-Based Test Conditions

| ID | Test Condition | Risk Level | Priority | AC Reference |
|----|----------------|------------|----------|--------------|
| TC-01 | Verify that the language switcher is visible on all customer-facing pages. | High | P1 | ACC-01 |
| TC-02 | Verify that a customer can switch the webshop language to Czech at any time during the session. | High | P1 | ACC-02 |
| TC-03 | Verify that all main navigation elements are displayed in Czech after selecting the Czech language. | High | P1 | ACC-03 |
| TC-04 | Verify that all static UI elements (buttons, labels, menus, headings and dialogs) are displayed in Czech. | High | P1 | ACC-04 |
| TC-05 | Verify that product titles are displayed in Czech for all products available in the webshop. | High | P1 | ACC-05 |
| TC-06 | Verify that product descriptions are displayed in Czech for all products available in the webshop. | Medium | P2 | ACC-06 |
| TC-07 | Verify that error messages are displayed in Czech when validation or system errors occur. | High | P1 | ACC-07 |
| TC-08 | Verify that form validation messages are displayed in Czech during registration, login, checkout and profile updates. | High | P1 | ACC-08 |
| TC-09 | Verify that the selected Czech language remains active while navigating through the webshop pages. | Medium | P2 | ACC-09 |
| TC-10 | Verify that the checkout workflow, including cart, address, payment and order confirmation pages, is fully available in Czech. | High | P1 | ACC-10 |

---

## Traceability Matrix

| Acceptance Criterion | Test Condition(s) |
|----------------------|-------------------|
| ACC-01 | TC-01 |
| ACC-02 | TC-02 |
| ACC-03 | TC-03 |
| ACC-04 | TC-04 |
| ACC-05 | TC-05 |
| ACC-06 | TC-06 |
| ACC-07 | TC-07 |
| ACC-08 | TC-08 |
| ACC-09 | TC-09 |
| ACC-10 | TC-10 |

---

## Coverage Analysis

### Coverage Result

All acceptance criteria are covered by at least one test condition.

**Coverage Gaps Identified:** None.

The traceability mapping confirms that every acceptance criterion is represented by at least one test condition.

---

## Top 5 Test Conditions for Immediate Test Design

| Priority Order | Test Condition | Risk |
|----------------|----------------|------|
| 1 | Verify that a customer can switch the webshop language to Czech at any time during the session. | High |
| 2 | Verify that all static UI elements are displayed in Czech. | High |
| 3 | Verify that product titles are displayed in Czech for all products. | High |
| 4 | Verify that error messages are displayed in Czech when validation or system errors occur. | High |
| 5 | Verify that the checkout workflow is fully available in Czech. | High |

These conditions were selected because they have the highest impact on usability, customer trust and successful completion of purchases.

---

## Learning Summary

| Concept | One-line Takeaway |
|----------|------------------|
| Test Conditions | GenAI can derive test conditions from user stories and acceptance criteria. |
| Risk-Based Prioritization | High-risk functionality should be prioritized for testing. |
| Traceability | Every test condition should be linked to a source requirement. |
| Coverage Analysis | Mapping acceptance criteria to test conditions helps identify coverage gaps. |
| Human Verification | AI-generated outputs must be reviewed and validated by a tester. |
| Assumptions | AI-generated assumptions should be checked against actual requirements and system behavior. |

> GenAI supported the identification and prioritization of test conditions and assisted with coverage analysis. However, the tester remained responsible for validating assumptions, verifying traceability, identifying coverage gaps and refining the final test-condition set.
