# Improvement Issue Template – Testability & Automation

## Title
[Improvement]: <Feature / Area> – <Short Description>

---

## Context
What is the current situation?

---

## Problem Statement
- What is missing?
- Why is it a problem?
- Who is affected?

---

## Proposed Improvement
What should be changed or added?

---

## Test Automation Value
- What becomes easier to test?
- Which problems are solved?

---

## Example Test Scenario
Given ...
When ...
Then ...

---

## Technical Considerations
- APIs involved?
- UI changes?
- Testability hooks needed?

---

## Risks
- What could break?
- Dependencies?

---

## Business Value
- UX improvement?
- Stability?
- Speed?

---

## Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

---

## Suggested Owner
- Frontend / Backend / QA / DevOps

---

## Labels
enhancement, test-automation, testability


---

# Example Issue: Registration with GitHub

## Title
[Improvement]: Registration – Add GitHub OAuth Login Option

---

## Context
Currently, users must register manually using email and password.

---

## Problem Statement
- Manual registration slows down automation
- Test data management is complex
- No fast way to create authenticated sessions

---

## Proposed Improvement
Add a "Sign up with GitHub" option using OAuth.

---

## Test Automation Value
- Faster login setup
- Less test data management
- More stable automation

---

## Example Test Scenario
Given the user clicks "Sign up with GitHub"
When the user authenticates successfully
Then the user should be logged in

---

## Technical Considerations
- OAuth integration
- Secure token handling
- Test hooks like data-testid

---

## Risks
- External dependency on GitHub
- OAuth complexity

---

## Business Value
- Better UX
- Faster testing
- Lower maintenance

---

## Acceptance Criteria
- [ ] GitHub login button exists
- [ ] OAuth flow works
- [ ] User is created or reused
- [ ] Works in test environment
- [ ] Accessible for automation

---

## Suggested Owner
- Backend
- Frontend
- QA

---

## Labels
enhancement, test-automation, oauth
