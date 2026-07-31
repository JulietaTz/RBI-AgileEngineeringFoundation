# Apply Generic Skills Required for Testing on Toolshop

## Reference to ISTQB Syllabus chapter
**ISTQB FL – 1.5.1 Generic Skills Required for Testing**

# Outcome

## Selected Feature
**Login**

## Requirement / Behaviour
The application shall allow registered users to authenticate using a valid email address and password. Invalid credentials must not grant access.

**Assumption:** The Toolshop Sprint 5 documentation and login page represent the expected behaviour.

## Testing Artifact – Test Case

### Test Case ID
TC_LOGIN_001

### Objective
Verify that a registered user can log in successfully and that invalid credentials are handled correctly.

### Preconditions
- Toolshop Sprint 5 is available.
- Login page is reachable.
- Test user exists.

### Test Data

| Scenario | Input |
|---|---|
| Valid Login | customer@practicesoftwaretesting.com / welcome01 |
| Invalid Password | customer@practicesoftwaretesting.com / WrongPassword123 |
| Empty Fields | Email = empty, Password = empty |

### Test Execution

| Step | Action | Expected Result | Actual Result | Status |
|---|---|---|---|---|
|1|Open Login page|Login page displayed|Login page displayed|Pass|
|2|Login with valid credentials|User logged in|User logged in|Pass|
|3|Logout|Session terminated|Session terminated|Pass|
|4|Login with wrong password|Generic error message|Generic error displayed|Pass|
|5|Submit empty fields|Validation shown|Validation shown|Pass|

## Risks / Observations

| ID | Observation | Impact |
|---|---|---|
|R1|Verify password masking.|Security|
|R2|Do not reveal whether an account exists.|Security|
|R3|Test keyboard navigation and whitespace.|Usability|

## Application of Generic Skills

### Analytical Thinking
Separated the login flow into positive, negative and validation scenarios.

### Attention to Detail
Checked labels, mandatory fields and validation.

### Communication
Documented the test clearly so the Scrum team can reuse it.

### Curiosity
Identified additional scenarios for future testing:
- repeated failed logins
- whitespace
- keyboard navigation
- Google Sign-In

## Learning Summary

| Generic Skill | Application |
|---|---|
|Analytical Thinking|Created logical scenarios.|
|Attention to Detail|Reviewed validation and UI details.|
|Communication|Produced a reusable sprint artifact.|
|Curiosity|Identified additional risks and tests.|

> Generic tester skills improve the quality of test design and help the Scrum team detect risks earlier.
