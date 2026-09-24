# Extend the 42-Vienna Test Policy with Static Review Roles and Responsibilities

## Reference to ISTQB Syllabus chapter

ISTQB FL – 3.2.3 Roles and Responsibilities in Reviews

| Section | Topic |
|---------|-------|
| 3.2.3   | Roles and Responsibilities in Reviews (author, moderator, reviewers, scribe, manager) |
| 3.2.2   | Review Process Activities (context needed to place static review in the 42-Project lifecycle) |
| 3.2.4   | Review Types (used to decide how formal the 42-Project static review should be) |

## Link to the transfer task file

https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter%203/ISTQB-FL-3.2.2%2B3%2B4-Roles-Activities-Types-extend-test-policy.md 

## Outcome

# Static Review of 42-Project Artifacts

## Purpose and Trigger

A static review shall be performed before booking a peer evaluation for a
42 project.

The purpose of the static review is to identify defects and violations in
project artifacts before the dynamic peer-evaluation process begins.

The review shall be performed on the intended submission version of the
project after the author has completed self-testing and before the project
progresses to peer evaluation.

## Scope of Review

The static review shall cover relevant project artifacts without executing
the project functionality.

Depending on the project and its subject requirements, the review should
include:

- source code and project structure;
- mandatory requirements against the official subject PDF;
- required files and deliverables;
- Makefile structure and required targets;
- forbidden functions and libraries;
- Norm requirements where applicable;
- README and required documentation;
- configuration files where applicable; and
- obvious memory-management, resource-management, or crash risks that can
  be identified by reviewing the code.

The exact review scope shall be adapted to the requirements of the specific
42 project.

## Roles and Responsibilities

ISTQB review roles are mapped to the 42 environment as follows:

| Role | 42 Mapping | Responsibilities |
|---|---|---|
| **Manager** | Pedagogical staff or designated policy/curriculum lead | Decides that the static review is required, defines or maintains the review expectations, and ensures that the review process can be applied. The manager does not normally participate in the actual review. |
| **Author** | Student or team that developed the project | Provides the work product for review, supports clarification when necessary, evaluates the reported findings, and is responsible for correcting agreed findings. The author shall not act as an independent reviewer of their own project. |
| **Review Leader** | Student coordinating the review | Organizes the review, ensures that the required artifacts and review scope are available, and coordinates completion of the review before peer evaluation. |
| **Reviewer(s)** | One or more other 42 students | Independently examine the project artifacts, identify anomalies and possible defects, and report findings from relevant technical perspectives. |
| **Moderator (Facilitator)** | A reviewer or, when necessary, another 42 student | Facilitates the review process, keeps the review focused and effective, and supports discussion and decisions about identified findings. |
| **Scribe (Recorder)** | A reviewer or another participating student | Collects and records the identified findings, their locations, severity, responsible reporter, and current status. |

For a lightweight review of a 42 project, separate people are not required
for every role. The review leader, moderator, and scribe responsibilities
may be combined with the reviewer role.

The author and reviewer roles shall not be combined because reviewer
independence is required.

## Independence

At least one reviewer shall be independent from the project authors.

A student shall not act as the independent reviewer of their own work.
This provides an additional perspective and helps identify defects or
misunderstandings that may have been missed during development and
self-testing.

## Findings Log

Findings identified during the static review shall be recorded in a
lightweight findings log.

| Finding | Artifact / Location | Severity | Raised By | Status |
|---|---|---|---|---|
| Example: forbidden function used | `src/example.c:42` | High | Reviewer | Open |
| Example: mandatory information missing | `README.md` | Medium | Reviewer | Resolved |

The log may remain lightweight but shall contain enough information for the
author to locate, understand, and address each finding.

## Exit Condition

The static review is complete when:

- the required project artifacts have been reviewed;
- identified findings have been recorded;
- findings that prevent evaluation readiness have been resolved;
- remaining accepted findings or known risks have been explicitly recorded;
  and
- the project is considered ready to proceed to peer evaluation.

If a finding affects a mandatory project requirement or creates a risk of
immediate evaluation failure, the project should not proceed to peer
evaluation until the finding has been resolved.

## Relationship to Peer Validation

Static review and peer validation are separate activities.

The static review is performed **before peer evaluation** and examines
project work products without executing the project functionality. Its
purpose is to identify defects and requirement violations early.

Peer validation remains the independent evaluation process defined in the
existing 42-Vienna Test Policy. During peer validation, evaluators follow
the official evaluation sheet and dynamically execute and validate the
submitted project.

Therefore, the static review does not replace peer validation. It acts as
an additional quality gate before the existing peer-evaluation process.

## Document Control Update

The 42-Vienna Test Policy document control shall be updated to reflect the
addition of the static-review process:

| Field | Value |
|---|---|
| Document title | 42 Vienna Test Policy |
| Version | 1.1 |
| Author | Rashin Harisi |
| Reviewer | Rudolf Groetz |
| Last Updated | 2026-09-24 |
| Change | Added Static Review of 42-Project Artifacts |

### Learning Summary ###

In this task, I learned that review roles describe responsibilities and do
not necessarily require a different person for every role.

In the 42 environment, the project author is responsible for the work
product and for correcting agreed findings, while other 42 students can
provide an independent static review before peer evaluation.

For a lightweight review, roles such as review leader, moderator, scribe,
and reviewer can be combined. However, the reviewer must remain independent
from the author.

I also learned that static review and peer evaluation have different
purposes. Static review examines project artifacts before execution, while
the existing peer evaluation includes dynamic testing of the submitted
project.