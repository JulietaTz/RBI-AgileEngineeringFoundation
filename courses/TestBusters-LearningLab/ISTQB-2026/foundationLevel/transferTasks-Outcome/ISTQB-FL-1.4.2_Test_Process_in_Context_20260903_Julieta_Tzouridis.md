# Test Process in Context on Toolshop – Login Flow

## Reference to ISTQB Syllabus

**ISTQB FL – 1.4.2 Test Process in Context**

## Selected Flow

**Login**

## Requirement / Test Basis

Sprint 5 defines successful login behavior for registered customers.

A customer using valid credentials should be authenticated and redirected to the account area.

## Context Analysis

| Context Factor | Toolshop Context | Impact on Testing |
|---|---|---|
| Stakeholders | Scrum team and webshop customers | Login availability is important because customers need access to their accounts |
| Team members | Testing is performed within the Scrum team | The test should be easy to execute and understand |
| Business domain | E-commerce webshop | Login failure can block customer account functionality |
| Technical factors | Browser-based web application | Functional system testing through the UI is appropriate |
| Project constraints | 30-minute timebox and one selected flow | Test scope is intentionally narrow |
| Organizational factors | Result must be usable during the sprint | A concise Markdown artifact is used |
| SDLC | Agile / Scrum | Fast feedback during the sprint is required |
| Tools | Browser, Toolshop and GitHub documentation | Manual execution is sufficient for this focused task |

## Test Approach Derived from Context

- **Strategy:** focused risk-based functional testing
- **Technique:** specification-based testing
- **Automation:** manual execution due to the limited timebox
- **Coverage:** one critical login scenario
- **Testware:** concise test case
- **Reporting:** mini test report in Markdown

## Test Case

**ID:** `TC-LOGIN-001`  
**Title:** Successful login with valid customer credentials  
**Priority:** High

### Preconditions

- Toolshop is available.
- User is logged out.
- Valid customer account exists.

### Test Data

- **Email:** `customer@practicesoftwaretesting.com`
- **Password:** `welcome01`

### Test Execution

| Step | Action | Expected Result | Actual Result | Status |
|---|---|---|---|---|
| 1 | Open Toolshop: https://practicesoftwaretesting.com/ | Homepage is displayed | Homepage was displayed successfully | PASS |
| 2 | Open **Sign in** | Login form is displayed | Login form was displayed | PASS |
| 3 | Enter valid email | Email is accepted | Email was accepted | PASS |
| 4 | Enter valid password | Password is masked | Password was accepted and masked | PASS |
| 5 | Submit login | Authentication succeeds | Authentication succeeded without an error message | PASS |
| 6 | Check destination | Customer account page is displayed | Customer account page was displayed successfully after login | PASS |

## Result

**Status:** **PASS**

The login flow was executed successfully. All expected results matched the actual behavior.

## Mini Test Report

| Metric | Result |
|---|---:|
| Test cases designed | 1 |
| Test cases executed | 1 |
| Passed | 1 |
| Failed | 0 |
| Blocked | 0 |
| Overall status | PASS |

## Risks and Observations

- Login is a high-impact function because authentication problems can block access to customer functionality.
- The tested happy path worked as expected with valid customer credentials.
- Invalid credentials, account locking and other authentication scenarios remain relevant risks but are outside the intentionally limited scope of this task.
- Additional login scenarios should be covered in normal sprint testing.
- No unexpected behavior was observed during this focused test execution.

## Conclusion

The test process was adapted to the Toolshop context according to **ISTQB FL 1.4.2 – Test Process in Context**.

The selected context factors influenced the test approach, scope, level of detail, execution method and reporting. Due to the 30-minute timebox and the need for a sprint-ready artifact, the scope was limited to one critical manual login scenario.

The test was executed successfully and the expected behavior matched the actual result. The final status is **PASS**.
