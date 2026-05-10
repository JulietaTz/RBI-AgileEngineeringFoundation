# Test Data Strategy for Isolated Automated Tests

## Title
Design a Test Data Strategy for Isolated Automated Tests

---

## Syllabus Reference
NA

---

## Learning Objective
Understand how to design a test data approach that ensures automated tests are independent, reliable, and not affected by manual testing activities.

---

## Context / Scenario
You are part of a team automating tests for the webshop https://practicesoftwaretesting.com/.  

Currently, both manual testers and automated tests use the same test accounts and data. Manual testing often modifies or deletes data, causing automated tests to fail unpredictably.

Your goal is to design a **test data strategy** where each automated test creates and manages its own data via API.

---

## Task Instructions

1. Explore the webshop:
   - Navigate to account-related features (e.g. registration, login, profile)
   - Identify which data is typically required for user-based test scenarios

2. Review the available documentation:
   - Open the GitHub repository: https://github.com/testsmith-io/practice-software-testing/tree/main/docs  
   - Identify if API endpoints exist for creating users or test data

3. Define a **Test Data Approach** for automated tests:
   - Describe how automated tests will **create their own test data via API before execution**
   - Define how test data will be made **unique** (e.g. timestamp, randomization)

4. Define **Test Data Lifecycle Rules**:
   - When is test data created?
   - Is it cleaned up after execution or reused?
   - How do you ensure no dependency between test runs?

5. Identify **Risks and Mitigations**:
   - What happens if API data creation fails?
   - What if test data is not cleaned up?
   - How do you prevent collisions with manual testers?

6. Document your approach in a short **Test Data Strategy (max. 1 page)**

---

## Expected Outcome / Deliverable
A short written **Test Data Strategy** including:
- Approach for API-based test data creation
- Rules for data uniqueness
- Lifecycle management (setup/cleanup)
- Risk mitigation measures

---

## Timebox
30 minutes

---

## Hints
- Think in terms of **test isolation**
- Assume every test must be runnable **independently and repeatedly**
- Focus on **creating data, not reusing shared data**