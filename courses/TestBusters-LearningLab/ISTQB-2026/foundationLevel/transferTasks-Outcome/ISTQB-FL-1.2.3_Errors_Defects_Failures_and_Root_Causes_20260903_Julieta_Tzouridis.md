# J112 - Apply Errors, Defects, Failures, and Root Causes on Toolshop

## Reference to ISTQB Syllabus chapter

**ISTQB FL – 1.2.3 Errors, Defects, Failures, and Root Causes**

## Link to the transfer task file

**GitHub Transfer Task:**  
https://github.com/JulietaTz/RBI-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter%201/ISTQB-FL-1.2.3_Errors_Defects_Failures_and_Root_Causes_20260405.md

**Trello Transfer Task:**  
https://trello.com/c/KIUG7LCA/46-j112-istqb-fl-123errorsdefectsfailuresandrootcauses-create-bugs-for-v5

## Outcome

The outcome starts here.

### Selected Feature Flow

**Login – Account Lockout**

### Test Basis

**User Story:** US3370 – Login enhancement / Locked User

**User Story Link:**  
https://github.com/testsmith-io/practice-software-testing/blob/main/sprint5-with-bugs/testSessions/userStories/us3370-login-locked-account.md

**Known V5 Bug List:**  
https://github.com/testsmith-io/practice-software-testing/blob/main/sprint5-with-bugs/testSessions/listOfBugs.md

---

## Requirement / User Story

### Background

The user should be locked after **3 consecutive false login attempts**.

### User Story

As a shop admin,  
I want that the user gets locked after **3 consecutive false login attempts**  
to avoid a brute force attack.

### Acceptance Criteria

#### UC1 – 3 consecutive failed login attempts

**GIVEN**  
the user did 3 consecutive invalid login attempts with the same e-mail and wrong password

**WHEN**  
the user tries to log in again with the same username and wrong password

**THEN**  
the message **"User is locked - Login not possible!"** will be displayed

**AND**  
the user stays on the login screen.

#### UC2 – 2 consecutive failed login attempts

**GIVEN**  
the user did 2 consecutive invalid login attempts with the same username and wrong password

**WHEN**  
the user tries to log in again with the same username and correct password

**THEN**  
the user is logged in.

---

## Historical UAT V5 Finding

The V5 bug list contains the following historical finding:

**ID 21 – User will be locked after 1 invalid attempt**

This behavior would violate US3370 because the account should only be locked after **3 consecutive invalid login attempts**.

During the current execution, this historical finding could **not** be reproduced.

### Retest Result of Historical Finding

**PASS – historical Bug ID 21 appears to be fixed.**

The account was not locked after only one invalid login attempt.

After fewer than 3 failed attempts, login with the correct password was still possible.

---

## New Finding During Retest

During the retest of US3370, another deviation from the acceptance criteria was observed.

The account lockout itself occurs, but the displayed message does **not** match the message specified in UC1.

### Expected Message According to UC1

`User is locked - Login not possible!`

### Actual Message Observed

`Account locked, too many failed attempts. Please contact the administrator.`

### Result

**FAIL – displayed lockout message does not match US3370 / UC1.**

This finding is used as the bug for this transfer task.

---

## Test Environment

- Application: Toolshop
- Feature: Login / Account Lockout
- Browser: **Firefox**
- Test User: **tester@mail.at**
- Password: **not stored in this repository artifact**
- Test Date: **10 September 2026**

> The password was used during execution but is intentionally not included in the Markdown file because credentials should not be committed to a repository.

---

## Test Data

| Field | Test Data |
|---|---|
| E-mail | `ju@mail.at` |
| Valid Password | `<stored separately – not committed>` |
| Invalid Password | Any password different from the valid password |
| Browser | Firefox |

---

# Bug Report

## Title

**[Login][US3370][UC1] Incorrect message displayed when user account is locked**

## Related Requirement

**US3370 – Login enhancement / Locked User**

**Acceptance Criterion:** UC1 – 3 consecutive failed login attempts

## Preconditions

- A registered customer account exists.
- The account is active and not locked before the test.
- The tester knows the valid password.
- Firefox is used for execution.
- A test account that can be reset should be used because the test changes the account lock state.

## Steps to Reproduce

1. Open the Toolshop login page.
2. Enter the registered e-mail address `tester@mail.at`.
3. Enter an incorrect password.
4. Click **Login**.
5. Repeat the invalid login until **3 consecutive failed login attempts** have been performed.
6. Try to log in again with the same e-mail address and a wrong password.
7. Observe the message displayed on the login screen.

## Expected Result

According to **US3370 / UC1**:

- the user is locked after 3 consecutive failed login attempts,
- the user stays on the login screen,
- the following message is displayed:

`User is locked - Login not possible!`

## Actual Result

The user is locked and remains on the login screen, but a different message is displayed:

`Account locked, too many failed attempts. Please contact the administrator.`

Therefore, the functional lockout behavior works, but the displayed message does not match the acceptance criterion.

## Test Result

**FAIL**

---

## Evidence

**Screenshot:** Lockout message captured during manual execution in Firefox.

Observed message:

`Account locked, too many failed attempts. Please contact the administrator.`

Expected message according to UC1:

`User is locked - Login not possible!`

---

# ISTQB FL 1.2.3 Analysis

According to ISTQB FL 1.2.3:

- Human beings make **errors**.
- Errors can introduce **defects** into work products such as requirements, source code, configuration, or test artifacts.
- When a defect is executed, it may cause an observable **failure**.
- A **root cause** is the fundamental reason for the occurrence of a problem.
- Root cause analysis can help prevent similar defects or failures.

Applied to the current Toolshop finding:

| ISTQB Concept | Application to Toolshop |
|---|---|
| **Failure** | The system displays a lockout message that differs from the message required by UC1. |
| **Defect** | The implemented lockout message does not match the text defined in the acceptance criterion. |
| **Possible Human Error** | The developer may have implemented another message text or may not have checked the exact wording defined in UC1. |
| **Possible Root Cause** | Requirement-to-implementation traceability or review of UI messages may have been insufficient. |

> **Important:** The failure is directly observable during testing. The possible human error and root cause are hypotheses and would need to be confirmed through root cause analysis.

---

## Failure

A **failure** is incorrect behavior observed while the software is running.

### Observed Toolshop Failure

After the lockout condition is reached, the application displays:

`Account locked, too many failed attempts. Please contact the administrator.`

However, UC1 requires:

`User is locked - Login not possible!`

The visible system behavior therefore differs from the specified expected behavior.

---

## Defect

A **defect** is a fault in a work product that can cause a failure.

### Possible Toolshop Defect

The lockout notification implemented in the application contains a different text from the message specified in US3370 / UC1.

Possible locations of the defect could include:

- frontend message configuration,
- backend response message,
- translation/resource file,
- UI mapping of an authentication error.

The exact technical location would need to be identified by the development team.

---

## Possible Human Error

A possible human error is:

**The required lockout message from UC1 was not implemented exactly as specified.**

Possible explanations include:

- a different text was entered during implementation,
- the acceptance criterion was not checked carefully,
- an existing generic authentication message was reused,
- the UI message was changed without updating the requirement.

This is a **hypothesis** and cannot be treated as a confirmed cause without further investigation.

---

## Possible Root Cause

A possible root cause is:

**Insufficient traceability and review between the acceptance criterion and the implemented user-facing message.**

Possible contributing factors include:

- missing verification of exact UI text,
- insufficient review of US3370 / UC1,
- no test case that explicitly checks the lockout message,
- requirement and implementation may have changed independently.

The actual root cause would need to be confirmed through root cause analysis.

---

# Risk / Quality Impact

## Functional Impact

The main lockout functionality still works, so the user is protected against repeated login attempts.

However, the displayed behavior does not fully comply with the acceptance criterion.

## Usability Impact

The actual message has different wording and gives different information than the required message.

This can lead to:

- inconsistent user communication,
- confusion about expected behavior,
- mismatch between specification and implementation.

## Maintainability / Testability Impact

If automated or manual tests validate the exact acceptance-criterion text, they will fail.

A mismatch between requirements and implementation also reduces traceability and makes future maintenance more difficult.

## Suggested Severity

**Low to Medium**

Reason: The security-related lockout functionality works, but the specified user-facing message is incorrect.

## Suggested Priority

**Medium**

Reason: The defect should be clarified and corrected to keep implementation and acceptance criteria consistent, but it does not block the core login-lockout functionality.

---

# Recommended Retest After Fix

| Test | Previous Failed Attempts | Next Action | Expected Result |
|---|---:|---|---|
| RT-01 | 1 | Login with correct password | Login successful |
| RT-02 | 2 | Login with correct password | Login successful – confirms UC2 |
| RT-03 | 3 | Login again with wrong password | User is locked |
| RT-04 | 3 | Check displayed text | `User is locked - Login not possible!` |
| RT-05 | 3 | Check navigation | User stays on login screen |

---

# Error → Defect → Failure Trace

**Possible Human Error**  
The required UI message from UC1 is implemented incorrectly or another generic message is reused.

↓

**Defect**  
The application contains a different lockout message from the one specified in US3370 / UC1.

↓

**Execution Condition**  
The user reaches the account-lock condition after consecutive failed login attempts.

↓

**Failure**  
The application displays the wrong lockout message.

↓

**Impact**  
The implementation does not fully comply with the acceptance criterion and produces inconsistent user communication.

---

# Sprint-Ready Summary

**Requirement:** US3370 / UC1 requires the message  
`User is locked - Login not possible!`

**Historical V5 Finding:** Bug ID 21 stated that the user was locked after only one invalid attempt.

**Historical Finding Retest:** PASS – this defect could not be reproduced and appears to be fixed.

**New Finding:** The user is locked at the expected stage, but the displayed lockout message differs from UC1.

**Expected:**  
`User is locked - Login not possible!`

**Actual:**  
`Account locked, too many failed attempts. Please contact the administrator.`

**Failure:** Incorrect lockout message is displayed.

**Defect:** Implemented message does not match the acceptance criterion.

**Possible Human Error:** Wrong or generic message text was implemented.

**Possible Root Cause:** Insufficient traceability/review of the user-facing message against UC1.

**Risk:** Low to Medium – core lockout functionality works, but the implementation does not fully comply with the requirement.

**Status:** FAIL / Bug should be recorded according to the Bug Tracking SOP.

---

# Learning Summary

| Concept | One-line takeaway |
|---|---|
| Error | A human mistake can introduce a defect into a work product. |
| Defect | A defect is the fault in the implementation, such as an incorrect UI message. |
| Failure | A failure is the observable difference between expected and actual system behavior. |
| Root Cause | The root cause explains why the problem was introduced or remained undetected. |
| Retest | A historical defect may be fixed even though another deviation is discovered during retesting. |
| Toolshop Example | Correct lockout behavior + wrong message → requirement mismatch → observable failure. |

> **A failure is what we observe during testing. A defect is the fault that causes the failure. An error is a possible human action that introduced the defect, and the root cause explains why the problem occurred.**