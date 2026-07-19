---
artifact_id: TCA-TOOLSHOP-001
title: ToolShop Manual Test Cases - Registration Increment
artifact_type: test-cases
project: toolshop
version: 0.1
status: draft
created_by: test-designer
created_at: 2026-07-17T22:32:12+02:00
source_inputs:
  - projects/toolshop/working/test-conditions.md
  - projects/toolshop/working/reviews/test-conditions-review.md
  - projects/toolshop/working/reviews/qg-02-analysis-ready-evidence.md
  - projects/toolshop/input/registration-user-story.md
  - projects/toolshop/working/test-context.md
  - skills/design-test-cases/SKILL.md
  - agents/test-designer.agent.md
assumptions:
  - A-01: QG-02 PASS for TCN-TOOLSHOP-001 (QG02-TOOLSHOP-001 / REV-TCN-TOOLSHOP-001) authorises design from these reviewed conditions.
  - A-02: Emptiness of a required field is treated as invalid (carried from TCN A-02).
  - A-03: Login success means authentication with the newly registered credentials succeeds; post-login destination is unspecified (TCN A-04 / Q-BA-05).
  - A-04: “Registration success” is any observable SUT indicator that the account was created (message, redirect, or subsequent login capability) — exact UI chrome is not invented.
  - A-05: Error-message checks assert presence/observability only; exact wording and placement remain open (Q-BA-04 / R-05).
open_questions:
  - Q-01: Which ToolShop environment/version/URL is the SUT? (parameterised as {TOOLSHOP_BASE_URL})
  - Q-BA-01: Email format/syntax rules beyond uniqueness?
  - Q-BA-02: Password rules beyond minimum length 8?
  - Q-BA-03: Whitespace-only / max-length behaviour for required fields?
  - Q-BA-04: Exact error message text/placement?
  - Q-BA-05: Observable login-success criteria / auto-login vs manual login?
---

# Manual Test Cases — Registration

**Execution mode:** Manual only. No Playwright or other automation code in this artifact.  
**SUT:** ToolShop registration ({TOOLSHOP_BASE_URL} — version unknown, Q-01).  
**Data rule:** Synthetic data only; no production PII or credentials.

---

## 1. Technique Selection (justified)

| Technique | Why selected | Applied to |
|---|---|---|
| Equivalence Partitioning (EP) | Inputs fall into groups with equivalent expected behaviour (valid vs invalid required data; unique vs duplicate email; password length valid/invalid partitions). | Happy path; missing-field invalid partitions; uniqueness partitions |
| Boundary Value Analysis (BVA) | Ordered length threshold “at least 8 characters” (AC-3) → coverage at 7 / 8 / 9. No maximum invented. | Password length |
| Decision Table Testing | Combinations of required-field validity determine accept vs reject (AC-1 + AC-5). | Missing required fields |
| Error Guessing | Duplicate/already-registered email is a high-likelihood defect class (R-01 / AC-2). | Duplicate email |
| State/transition scenario (limited) | Registration creates a state in which login with those credentials must succeed (AC-4). | Post-registration login |

Not used: full exploratory charter suite (out of this design scope); state-machine modelling beyond registration→login.

---

## 2. Coverage Items

| Coverage Item ID | Technique | Description | Source Condition(s) |
|---|---|---|---|
| CI-EP-VALID | EP | Valid combination: unique email + password length ≥ 8 + all required fields present | TC-REG-001, TC-REG-003 |
| CI-DT-MISS-FN | Decision Table | First name empty; other required fields valid | TC-REG-002, TC-REG-008 |
| CI-DT-MISS-LN | Decision Table | Last name empty; other required fields valid | TC-REG-002, TC-REG-008 |
| CI-DT-MISS-EM | Decision Table | Email empty; other required fields valid | TC-REG-002, TC-REG-008 |
| CI-DT-MISS-PW | Decision Table | Password empty; other required fields valid | TC-REG-002, TC-REG-008 |
| CI-EG-DUP-EMAIL | Error Guessing / EP | Email already associated with an existing account | TC-REG-004, TC-REG-008 |
| CI-BVA-PW-7 | BVA | Password length = 7 (just below minimum) | TC-REG-005, TC-REG-008 |
| CI-BVA-PW-8 | BVA | Password length = 8 (minimum valid) | TC-REG-006 |
| CI-BVA-PW-9 | BVA | Password length = 9 (just above minimum) | TC-REG-007 |
| CI-ST-LOGIN | State/scenario | After successful registration, login with same credentials | TC-REG-009 |

### Decision Table — Required Field Validity (simplified)

| Rule | First name | Last name | Email | Password | Expected outcome |
|---|---|---|---|---|---|
| R1 | Valid | Valid | Valid unique | Length ≥ 8 | Accept (CI-EP-VALID) |
| R2 | Empty | Valid | Valid unique | Length ≥ 8 | Reject + observable error |
| R3 | Valid | Empty | Valid unique | Length ≥ 8 | Reject + observable error |
| R4 | Valid | Valid | Empty | Length ≥ 8 | Reject + observable error |
| R5 | Valid | Valid | Valid unique | Empty | Reject + observable error |

Additional rules outside this table: duplicate email (CI-EG-DUP-EMAIL); password length 7/8/9 (BVA items).

---

## 3. Shared Parameters and Synthetic Data Pools

| Parameter | Meaning | Example / rule |
|---|---|---|
| `{TOOLSHOP_BASE_URL}` | SUT base URL (unknown until Q-01 answered) | e.g. environment URL once confirmed |
| `{REGISTRATION_PATH}` | Path/entry to registration UI | Discover on SUT; do not invent labels beyond “registration / sign-up” |
| `{LOGIN_PATH}` | Path/entry to login UI | Discover on SUT |
| `{EXISTING_EMAIL}` | Email of a pre-existing customer account in the test environment | Synthetic seeded account only |
| `{EXISTING_PASSWORD}` | Password for that seeded account (if needed for setup verification) | Synthetic only |

**Valid baseline data pattern (unique per run):**

| Field | Synthetic value pattern |
|---|---|
| First name | `Ada` |
| Last name | `Tester` |
| Email | `ada.tester.{run_id}@example.test` |
| Password (length 8) | `Passw0rd` (8 characters) |
| Password (length 7) | `Passw0r` (7 characters) |
| Password (length 9) | `Passw0rdX` (9 characters) |

`{run_id}` = unique suffix (timestamp or counter) so emails remain unused except where a duplicate is intentional.

---

## 4. Test Cases

### TCASE-REG-001

- **Test Case ID:** TCASE-REG-001
- **Linked Test Condition:** TC-REG-001; TC-REG-003
- **Coverage Item:** CI-EP-VALID (Decision Table R1)
- **Objective:** Verify successful registration when all required fields are provided with a unique email and password length ≥ 8.
- **Priority:** High
- **Preconditions:**
  - SUT registration UI is reachable at `{TOOLSHOP_BASE_URL}` / `{REGISTRATION_PATH}`.
  - Email `ada.tester.{run_id}@example.test` is **not** already registered.
- **Test Data:**
  - First name: `Ada`
  - Last name: `Tester`
  - Email: `ada.tester.{run_id}@example.test`
  - Password: `Passw0rd` (8 characters)
- **Steps:**
  1. Open the registration page.
  2. Enter First name, Last name, Email and Password from Test Data.
  3. Submit the registration form.
- **Expected Result:**
  - Registration is accepted.
  - An observable registration-success indicator is present (e.g. success confirmation and/or ability to proceed to login). Exact message text/UI chrome is **not** asserted beyond observability (A-04).
  - No error message indicating registration failure is shown.
- **Postconditions:** Account exists for the submitted email (usable by TCASE-REG-009 if chained). Record the exact email/password used.
- **Automation Candidate:** Yes — deterministic happy path with stable form inputs; oracle is acceptance without inventing UI copy.

---

### TCASE-REG-002A

- **Test Case ID:** TCASE-REG-002A
- **Linked Test Condition:** TC-REG-002; TC-REG-008
- **Coverage Item:** CI-DT-MISS-FN (Decision Table R2)
- **Objective:** Verify registration is rejected with an observable error when first name is empty and other required fields are valid.
- **Priority:** High
- **Preconditions:**
  - Registration UI reachable.
  - Email `ada.missfn.{run_id}@example.test` is unused.
- **Test Data:**
  - First name: *(empty)*
  - Last name: `Tester`
  - Email: `ada.missfn.{run_id}@example.test`
  - Password: `Passw0rd`
- **Steps:**
  1. Open the registration page.
  2. Leave First name empty; enter Last name, Email and Password from Test Data.
  3. Submit the registration form.
- **Expected Result:**
  - Registration is rejected (no successful account creation for this email).
  - An error message is displayed and observable to the user (AC-5). Do **not** assert exact wording, colour or placement (Q-BA-04 / A-05).
- **Postconditions:** No new usable account for the submitted email (or account creation clearly failed).
- **Automation Candidate:** Yes — empty-field negative path; assert rejection + error presence (not text).

---

### TCASE-REG-002B

- **Test Case ID:** TCASE-REG-002B
- **Linked Test Condition:** TC-REG-002; TC-REG-008
- **Coverage Item:** CI-DT-MISS-LN (Decision Table R3)
- **Objective:** Verify registration is rejected with an observable error when last name is empty and other required fields are valid.
- **Priority:** High
- **Preconditions:** Registration UI reachable; email `ada.missln.{run_id}@example.test` unused.
- **Test Data:**
  - First name: `Ada`
  - Last name: *(empty)*
  - Email: `ada.missln.{run_id}@example.test`
  - Password: `Passw0rd`
- **Steps:**
  1. Open the registration page.
  2. Leave Last name empty; enter First name, Email and Password from Test Data.
  3. Submit the registration form.
- **Expected Result:**
  - Registration is rejected.
  - An error message is displayed and observable (content/placement not prescribed).
- **Postconditions:** No successful registration for the submitted email.
- **Automation Candidate:** Yes — same pattern as TCASE-REG-002A.

---

### TCASE-REG-002C

- **Test Case ID:** TCASE-REG-002C
- **Linked Test Condition:** TC-REG-002; TC-REG-008
- **Coverage Item:** CI-DT-MISS-EM (Decision Table R4)
- **Objective:** Verify registration is rejected with an observable error when email is empty and other required fields are valid.
- **Priority:** High
- **Preconditions:** Registration UI reachable.
- **Test Data:**
  - First name: `Ada`
  - Last name: `Tester`
  - Email: *(empty)*
  - Password: `Passw0rd`
- **Steps:**
  1. Open the registration page.
  2. Leave Email empty; enter First name, Last name and Password from Test Data.
  3. Submit the registration form.
- **Expected Result:**
  - Registration is rejected.
  - An error message is displayed and observable (content/placement not prescribed).
- **Postconditions:** No account created under an empty email identity.
- **Automation Candidate:** Yes.

---

### TCASE-REG-002D

- **Test Case ID:** TCASE-REG-002D
- **Linked Test Condition:** TC-REG-002; TC-REG-008
- **Coverage Item:** CI-DT-MISS-PW (Decision Table R5)
- **Objective:** Verify registration is rejected with an observable error when password is empty and other required fields are valid.
- **Priority:** High
- **Preconditions:** Registration UI reachable; email `ada.misspw.{run_id}@example.test` unused.
- **Test Data:**
  - First name: `Ada`
  - Last name: `Tester`
  - Email: `ada.misspw.{run_id}@example.test`
  - Password: *(empty)*
- **Steps:**
  1. Open the registration page.
  2. Leave Password empty; enter First name, Last name and Email from Test Data.
  3. Submit the registration form.
- **Expected Result:**
  - Registration is rejected.
  - An error message is displayed and observable (content/placement not prescribed).
- **Postconditions:** No successful registration for the submitted email.
- **Automation Candidate:** Yes.

---

### TCASE-REG-004

- **Test Case ID:** TCASE-REG-004
- **Linked Test Condition:** TC-REG-004; TC-REG-008
- **Coverage Item:** CI-EG-DUP-EMAIL
- **Objective:** Verify registration with an already-registered email is rejected and an error message is displayed.
- **Priority:** Critical
- **Preconditions:**
  - Registration UI reachable.
  - A customer account already exists for `{EXISTING_EMAIL}` (seeded synthetic account). Confirm existence before execution (e.g. prior successful registration or test-data setup).
  - Uniqueness comparison case/trim rules remain unknown — use an exact string match to `{EXISTING_EMAIL}` as registered.
- **Test Data:**
  - First name: `Ada`
  - Last name: `Duplicate`
  - Email: `{EXISTING_EMAIL}`
  - Password: `Passw0rd` (valid length; unrelated to existing password)
- **Steps:**
  1. Open the registration page.
  2. Enter First name, Last name, Email=`{EXISTING_EMAIL}`, and Password from Test Data.
  3. Submit the registration form.
- **Expected Result:**
  - Registration is rejected.
  - An error message is displayed and observable (do not invent exact text — R-05 / Q-BA-04).
  - The existing account remains the account associated with `{EXISTING_EMAIL}` (no silent overwrite assumed; verify by inability to complete a second successful registration for that email).
- **Postconditions:** Still only one account for `{EXISTING_EMAIL}` (as observable via failed duplicate registration / login with original credentials if available).
- **Automation Candidate:** Yes — high value for R-01; requires reliable seeded `{EXISTING_EMAIL}` fixture. Do not hard-code production emails.

---

### TCASE-REG-005

- **Test Case ID:** TCASE-REG-005
- **Linked Test Condition:** TC-REG-005; TC-REG-008
- **Coverage Item:** CI-BVA-PW-7
- **Objective:** Verify a password of length 7 is rejected with an observable error when other inputs are otherwise valid.
- **Priority:** Critical
- **Preconditions:** Registration UI reachable; email `ada.pw7.{run_id}@example.test` unused.
- **Test Data:**
  - First name: `Ada`
  - Last name: `Tester`
  - Email: `ada.pw7.{run_id}@example.test`
  - Password: `Passw0r` (exactly **7** characters)
- **Steps:**
  1. Open the registration page.
  2. Enter all fields from Test Data (password length 7).
  3. Submit the registration form.
- **Expected Result:**
  - Registration is rejected.
  - An error message is displayed and observable.
  - No complexity rules beyond length are asserted (Q-BA-02).
- **Postconditions:** No successful registration for the submitted email.
- **Automation Candidate:** Yes — clear length oracle; do not add uppercase/digit/symbol checks.

---

### TCASE-REG-006

- **Test Case ID:** TCASE-REG-006
- **Linked Test Condition:** TC-REG-006
- **Coverage Item:** CI-BVA-PW-8
- **Objective:** Verify a password of exactly 8 characters is accepted in an otherwise valid registration.
- **Priority:** High
- **Preconditions:** Registration UI reachable; email `ada.pw8.{run_id}@example.test` unused.
- **Test Data:**
  - First name: `Ada`
  - Last name: `Tester`
  - Email: `ada.pw8.{run_id}@example.test`
  - Password: `Passw0rd` (exactly **8** characters)
- **Steps:**
  1. Open the registration page.
  2. Enter all fields from Test Data (password length 8).
  3. Submit the registration form.
- **Expected Result:**
  - Registration is accepted (observable success indicator per A-04).
  - No failure error for password length is shown.
- **Postconditions:** Account exists for the submitted email; record credentials if reused.
- **Automation Candidate:** Yes — boundary acceptance case.

---

### TCASE-REG-007

- **Test Case ID:** TCASE-REG-007
- **Linked Test Condition:** TC-REG-007
- **Coverage Item:** CI-BVA-PW-9
- **Objective:** Verify a password of length 9 (above minimum) is accepted in an otherwise valid registration.
- **Priority:** Medium
- **Preconditions:** Registration UI reachable; email `ada.pw9.{run_id}@example.test` unused.
- **Test Data:**
  - First name: `Ada`
  - Last name: `Tester`
  - Email: `ada.pw9.{run_id}@example.test`
  - Password: `Passw0rdX` (exactly **9** characters)
- **Steps:**
  1. Open the registration page.
  2. Enter all fields from Test Data (password length 9).
  3. Submit the registration form.
- **Expected Result:**
  - Registration is accepted (observable success indicator per A-04).
  - No maximum-length rule is assumed or asserted.
- **Postconditions:** Account exists for the submitted email.
- **Automation Candidate:** Yes — representative above-minimum valid length; further lengths omitted to avoid redundancy.

---

### TCASE-REG-009

- **Test Case ID:** TCASE-REG-009
- **Linked Test Condition:** TC-REG-009
- **Coverage Item:** CI-ST-LOGIN
- **Objective:** Verify that after successful registration the user can log in with the registered email and password.
- **Priority:** Critical
- **Preconditions:**
  - A successful registration has completed for a known synthetic pair (from TCASE-REG-001 or TCASE-REG-006 in the same session/environment), **or** perform registration first using the Test Data below.
  - Login UI reachable at `{TOOLSHOP_BASE_URL}` / `{LOGIN_PATH}`.
  - Auto-login vs mandatory manual login is unspecified (Q-BA-05): if the SUT already authenticates the user after registration, record that observation and still verify that the same credentials can authenticate via the login entry (logout first if needed and if logout is available without inventing features).
- **Test Data:**
  - Email: credentials from the successful registration (e.g. `ada.tester.{run_id}@example.test`)
  - Password: the password used at registration (e.g. `Passw0rd`)
- **Steps:**
  1. Ensure a successful registration completed with the Test Data credentials (execute TCASE-REG-001 first if needed).
  2. If currently authenticated after registration, sign out **only if** a logout control is available; if not available, note as environment observation and proceed to open the login page in a fresh session/browser context.
  3. Open the login page.
  4. Enter the registered Email and Password.
  5. Submit the login form.
- **Expected Result:**
  - Authentication succeeds with the registered credentials (A-03).
  - An observable logged-in indicator is present (e.g. account/session state). Exact destination page is **not** prescribed (Q-BA-05).
  - Browse/order flows are not exercised (out of scope).
- **Postconditions:** User session authenticated for the registered account (as observable in the SUT).
- **Automation Candidate:** Yes — high-value end-to-end path for R-03; requires stable registration+login selectors and careful session handling. Still no automation code in this workflow.

---

## 5. Traceability Matrix

| Test Case | TC-REG-001 | TC-REG-002 | TC-REG-003 | TC-REG-004 | TC-REG-005 | TC-REG-006 | TC-REG-007 | TC-REG-008 | TC-REG-009 | Priority |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|---|
| TCASE-REG-001 | X | | X | | | | | | | High |
| TCASE-REG-002A | | X | | | | | | X | | High |
| TCASE-REG-002B | | X | | | | | | X | | High |
| TCASE-REG-002C | | X | | | | | | X | | High |
| TCASE-REG-002D | | X | | | | | | X | | High |
| TCASE-REG-004 | | | | X | | | | X | | Critical |
| TCASE-REG-005 | | | | | X | | | X | | Critical |
| TCASE-REG-006 | | | | | | X | | | | High |
| TCASE-REG-007 | | | | | | | X | | | Medium |
| TCASE-REG-009 | | | | | | | | | X | Critical |

### Condition coverage check

| Condition | Covered by | Notes |
|---|---|---|
| TC-REG-001 | TCASE-REG-001 | Happy path |
| TC-REG-002 | TCASE-REG-002A–002D | One case per missing required field (Decision Table) |
| TC-REG-003 | TCASE-REG-001 | Unique-email valid partition consolidated with happy path (no redundant second success case) |
| TC-REG-004 | TCASE-REG-004 | Duplicate email / error guessing |
| TC-REG-005 | TCASE-REG-005 | BVA length 7 |
| TC-REG-006 | TCASE-REG-006 | BVA length 8 |
| TC-REG-007 | TCASE-REG-007 | BVA length 9 |
| TC-REG-008 | TCASE-REG-002A–002D, 004, 005 | Error observability via negatives; no separate redundant case |
| TC-REG-009 | TCASE-REG-009 | Registration → login |

---

## 6. Automation Candidates Summary

| Test Case | Candidate? | Rationale |
|---|---|---|
| TCASE-REG-001 | Yes | Stable happy path; clear accept/reject oracle |
| TCASE-REG-002A–002D | Yes | Repetitive form negatives; assert rejection + error presence |
| TCASE-REG-004 | Yes | Critical R-01; needs seeded duplicate email fixture |
| TCASE-REG-005 | Yes | Clear length boundary; no invented complexity |
| TCASE-REG-006 | Yes | Minimum valid boundary |
| TCASE-REG-007 | Yes | Above-minimum representative |
| TCASE-REG-009 | Yes | Critical R-03 chain; session handling needed |

**Explicit non-implementation:** This artifact marks candidates only. No Playwright (or other) automation code is produced in Workflow 04.

---

## 7. Redundancy and Scope Notes

- TCASE-REG-001 covers both TC-REG-001 and TC-REG-003 to avoid two nearly identical success cases.
- TC-REG-008 has no dedicated case; AC-5 error observability is asserted in all designed negatives.
- Password lengths beyond 7/8/9 are omitted (smallest meaningful BVA set).
- Email syntax, password complexity, max password length, and exact error copy are **not** tested as requirements.
- Whitespace-only fields are not covered (Q-BA-03 unresolved).

## 8. Recommended Next Step

1. Independent review of `TCA-TOOLSHOP-001` by **test-reviewer**.
2. Apply corrections if required.
3. Run **QG-03**.
4. Hand over to implementation or manual execution (no auto-code unless a later workflow requests it).
