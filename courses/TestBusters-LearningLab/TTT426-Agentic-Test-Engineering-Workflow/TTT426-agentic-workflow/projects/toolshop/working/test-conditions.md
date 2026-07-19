---
artifact_id: TCN-TOOLSHOP-001
title: ToolShop Test Conditions - Registration Increment
artifact_type: test-conditions
project: toolshop
version: 0.1
status: draft
created_by: test-analyst
created_at: 2026-07-17T14:07:28+02:00
source_inputs:
  - projects/toolshop/working/test-context.md
  - projects/toolshop/working/test-basis-analysis.md
  - projects/toolshop/input/registration-user-story.md
  - skills/derive-test-conditions/SKILL.md
assumptions:
  - A-01: Preliminary risks R-01–R-05 from TCX-TOOLSHOP-001 are the risk basis (no formal PRA artifact yet).
  - A-02: “Invalid input” for negative conditions is limited to classes directly implied by AC-1–AC-3 (missing required data, duplicate email, password length < 8) unless clarified.
  - A-03: Email syntax/format and password complexity beyond minimum length are not mandatory requirements for this draft.
  - A-04: Login success for AC-4 means authentication with the newly registered credentials succeeds; UI destination details are unspecified.
open_questions:
  - Q-01: SUT environment/version still unknown.
  - Q-BA-01 to Q-BA-05: see test-basis-analysis.md
---

# Test Conditions

## Coverage Summary

| Source | Covered by Test Condition IDs |
|---|---|
| AC-1 | TC-REG-001, TC-REG-002, TC-REG-008 |
| AC-2 | TC-REG-003, TC-REG-004 |
| AC-3 | TC-REG-005, TC-REG-006, TC-REG-007 |
| AC-4 | TC-REG-009 |
| AC-5 | TC-REG-002, TC-REG-004, TC-REG-005, TC-REG-008 |
| R-01 (duplicate email) | TC-REG-003, TC-REG-004 |
| R-02 (weak password) | TC-REG-005, TC-REG-006, TC-REG-007 |
| R-03 (login after registration) | TC-REG-009 |
| R-04 (invalid input / errors) | TC-REG-002, TC-REG-004, TC-REG-005, TC-REG-008 |
| R-05 (ambiguous error expectations) | TC-REG-008 (notes); design must not invent message text |

Risk note: R-01–R-05 are preliminary context risks, not scored classes from a formal product risk analysis. Priorities below reflect analyst judgement from those risks and AC criticality.

---

## TC-REG-001

- **Test Condition ID:** TC-REG-001
- **Title:** Successful registration with required fields
- **Source Requirement:** AC-1; US-REG-001
- **Product Risk:** Supports mitigation of blocked registration journey (related to R-03 pathway)
- **Test Level:** System
- **Test Type:** Functional — positive
- **Priority:** High
- **Description:** Verify that a visitor can successfully register when first name, last name, email and password are provided in a valid combination consistent with AC-1–AC-3 (unique email; password length ≥ 8).
- **Suggested Test Technique:** Equivalence Partitioning (valid input partition)
- **Notes and Open Questions:** Do not invent additional mandatory fields or UI chrome. Exact “success” presentation (message, redirect) is unspecified — design should use an observable registration-success indicator available in the SUT without inventing requirements.

---

## TC-REG-002

- **Test Condition ID:** TC-REG-002
- **Title:** Rejection when required registration data is missing
- **Source Requirement:** AC-1; AC-5
- **Product Risk:** R-04
- **Test Level:** System
- **Test Type:** Functional — negative
- **Priority:** High
- **Description:** Verify that registration is rejected and an error message is displayed when one or more of the required fields (first name, last name, email, password) is omitted / empty, treating emptiness as invalid per AC-5 in conjunction with AC-1.
- **Suggested Test Technique:** Equivalence Partitioning (invalid / empty partitions); optionally Decision Table for field combinations
- **Notes and Open Questions:** Assumption A-02: emptiness is treated as invalid. Whitespace-only and max-length behaviour are not specified (Q-BA-01 / Q-BA-03).

---

## TC-REG-003

- **Test Condition ID:** TC-REG-003
- **Title:** Registration accepted for a previously unused email
- **Source Requirement:** AC-2; AC-1
- **Product Risk:** R-01 (positive counterpart)
- **Test Level:** System
- **Test Type:** Functional — positive
- **Priority:** High
- **Description:** Verify that registration succeeds when the submitted email is not already associated with an existing customer account (uniqueness satisfied).
- **Suggested Test Technique:** Equivalence Partitioning (unique-email valid partition)
- **Notes and Open Questions:** Uniqueness comparison rules (case, trim) unknown (analysis finding on AC-2). Use a clearly unused synthetic email.

---

## TC-REG-004

- **Test Condition ID:** TC-REG-004
- **Title:** Duplicate email rejected with error message
- **Source Requirement:** AC-2; AC-5
- **Product Risk:** R-01
- **Test Level:** System
- **Test Type:** Functional — negative / exception
- **Priority:** Critical
- **Description:** Verify that attempting to register with an email already used by an existing account is rejected and an error message is displayed.
- **Suggested Test Technique:** Error Guessing / Equivalence Partitioning (duplicate-email invalid partition)
- **Notes and Open Questions:** Critical coverage for R-01. Do not invent exact message text (R-05 / Q-BA-04). Confirm case-sensitivity of uniqueness before hardening oracles.

---

## TC-REG-005

- **Test Condition ID:** TC-REG-005
- **Title:** Password shorter than eight characters rejected
- **Source Requirement:** AC-3; AC-5
- **Product Risk:** R-02
- **Test Level:** System
- **Test Type:** Functional — negative / boundary
- **Priority:** Critical
- **Description:** Verify that a password with fewer than eight characters is rejected and an error message is displayed.
- **Suggested Test Technique:** Boundary Value Analysis (length below minimum, e.g. 7) and Equivalence Partitioning (too-short partition)
- **Notes and Open Questions:** Explicit coverage for R-02. No complexity rules are specified — do not add uppercase/digit/symbol requirements.

---

## TC-REG-006

- **Test Condition ID:** TC-REG-006
- **Title:** Password of exactly eight characters accepted (length boundary)
- **Source Requirement:** AC-3; AC-1
- **Product Risk:** R-02
- **Test Level:** System
- **Test Type:** Functional — positive / boundary
- **Priority:** High
- **Description:** Verify that a password of exactly eight characters meets the stated minimum length rule and can be used in an otherwise valid registration.
- **Suggested Test Technique:** Boundary Value Analysis (minimum valid length = 8)
- **Notes and Open Questions:** Upper bound not specified — do not invent a maximum. Character-set constraints unknown.

---

## TC-REG-007

- **Test Condition ID:** TC-REG-007
- **Title:** Password longer than eight characters accepted (above minimum)
- **Source Requirement:** AC-3; AC-1
- **Product Risk:** R-02
- **Test Level:** System
- **Test Type:** Functional — positive / boundary
- **Priority:** Medium
- **Description:** Verify that a password longer than eight characters is accepted under the “at least eight characters” rule when other registration inputs are valid.
- **Suggested Test Technique:** Boundary Value Analysis / Equivalence Partitioning (valid length above minimum, e.g. 9)
- **Notes and Open Questions:** Consolidates above-minimum valid length; avoid redundant cases beyond representative values at design time.

---

## TC-REG-008

- **Test Condition ID:** TC-REG-008
- **Title:** Invalid registration input produces a visible error message
- **Source Requirement:** AC-5
- **Product Risk:** R-04; R-05
- **Test Level:** System
- **Test Type:** Functional — negative
- **Priority:** High
- **Description:** Verify that when registration input is invalid (as established by AC-linked invalid classes such as missing required data, duplicate email, or password length < 8), the system rejects the registration and displays an error message that is observable to the user.
- **Suggested Test Technique:** Equivalence Partitioning; exploratory check only for message presence/observability — not for unspecified wording
- **Notes and Open Questions:** Addresses R-05 by requiring observability of an error without prescribing content/location. Overlaps intentional with TC-REG-002/004/005 for the rejection aspect; this condition emphasises the error-message obligation of AC-5. Exact text/placement remain open (Q-BA-04).

---

## TC-REG-009

- **Test Condition ID:** TC-REG-009
- **Title:** Login possible after successful registration
- **Source Requirement:** AC-4
- **Product Risk:** R-03
- **Test Level:** System
- **Test Type:** Functional — positive / end-to-end within increment
- **Priority:** Critical
- **Description:** Verify that after a successful registration, the user can log in using the registered email and password.
- **Suggested Test Technique:** State/transition style scenario (registered → authenticate); alternatively use-case based end-to-end check limited to registration→login
- **Notes and Open Questions:** Assumption A-04: success means credential authentication succeeds. Auto-login vs manual login and post-login destination are unspecified (Q-BA-05). Browse/order flows remain out of scope.

---

## Explicitly Not Derived as Mandatory Conditions

The following were considered and **not** turned into mandatory conditions because they are not stated in the test basis:

- Email RFC/syntax validation beyond uniqueness
- Password complexity (uppercase, digits, symbols)
- Maximum password length
- Specific error message wording or field-level vs form-level UI layout
- GDPR/legal constraints beyond generic synthetic-data guardrails
- Non-functional performance, accessibility or localisation requirements

If clarified later, add new conditions in a new artifact version.

## Traceability Matrix

| Test Condition | AC-1 | AC-2 | AC-3 | AC-4 | AC-5 | R-01 | R-02 | R-03 | R-04 | R-05 |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| TC-REG-001 | X | | | | | | | | | |
| TC-REG-002 | X | | | | X | | | | X | |
| TC-REG-003 | X | X | | | | X | | | | |
| TC-REG-004 | | X | | | X | X | | | X | |
| TC-REG-005 | | | X | | X | | X | | X | |
| TC-REG-006 | X | | X | | | | X | | | |
| TC-REG-007 | X | | X | | | | X | | | |
| TC-REG-008 | | | | | X | | | | X | X |
| TC-REG-009 | | | | X | | | | X | | |

## Design Hints (not test cases)

Suitable techniques for later Workflow 04 (tutorial-aligned): Equivalence Partitioning, Boundary Value Analysis (password length), Decision Table for validity combinations, Error Guessing for already-registered email. Do not create detailed test cases in this artifact.
