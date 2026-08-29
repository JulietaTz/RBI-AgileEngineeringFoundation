# Extend the 42-Vienna Test Policy with Static Review Roles and Responsibilities

## 1. Title

Extend the 42-Vienna Test Policy with Static Review Roles and Responsibilities

## 2. Syllabus Reference

ISTQB FL – 3.2.3 Roles and Responsibilities in Reviews

| Section | Topic |
|---------|-------|
| 3.2.3   | Roles and Responsibilities in Reviews (author, moderator, reviewers, scribe, manager) |
| 3.2.2   | Review Process Activities (context needed to place static review in the 42-Project lifecycle) |
| 3.2.4   | Review Types (used to decide how formal the 42-Project static review should be) |

## 3. Learning Objective

Apply ISTQB FL 3.2.3 (Roles and Responsibilities in Reviews) by extending the existing **42-Vienna Test Policy** with a new chapter that (a) mandates a **static review** of 42-Project artifacts before peer evaluation, and (b) defines the review **roles and responsibilities**, mapped onto the 42 curriculum context (author, peer reviewer(s), optional moderator/scribe, manager).

## 4. Context / Scenario

The 42-Vienna Test Policy (see `courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter 1/ISTQB-FL-1_Test_Policy_42Vienna_20260706.md`, deliverable `testPolicy-42Vienna.md`) currently governs **peer validation** of 42-Projects — a dynamic, evaluation-based check performed by fellow students after submission.

A gap has been identified: the policy says nothing about **static testing**. Today, students go straight to peer evaluation without any structured review of the artifact itself (code, README, mandatory-part checklist, Norm compliance, Makefile, forbidden-function usage) beforehand. This causes avoidable evaluation failures (grade 0 for non-compilation, Norm errors, missing mandatory files) that a short static review would have caught earlier and cheaper.

Your task is to draft a **new chapter** for the 42-Vienna Test Policy that closes this gap by requiring a static review step and clearly assigning ISTQB-defined review roles to people in the 42 ecosystem (the author, other 42 students, and — where relevant — a supervising role).

## 5. Task Instructions

1. Review ISTQB FL 3.2.3 (Roles and Responsibilities in Reviews) and note the standard roles: **author**, **moderator (facilitator)**, **reviewers**, **scribe (recorder)**, and **manager**.
2. Read the existing 42-Vienna Test Policy (or, if unavailable in your environment, treat `testPolicy-42Vienna.md` as the parent document you are extending) to keep terminology and structure consistent.
3. Draft a new policy chapter titled **"Static Review of 42-Project Artifacts."** It must define:
   - **Trigger point**: which artifacts require static review, and when in the 42-Project lifecycle this happens (before booking a peer evaluation slot).
   - **Scope of review**: e.g. Norminette compliance, mandatory-part checklist against the subject PDF, Makefile correctness, forbidden function/library usage, README/documentation completeness, no obvious memory-management or crash risks visible from code reading.
   - **Roles and responsibilities table**: map each ISTQB role (author, moderator, reviewer(s), scribe, manager) onto a concrete 42 participant, stating their responsibilities. Explicitly address:
     - **Author** = the student who wrote the project; responsible for fixes, not for reviewing their own work.
     - **Reviewer(s)** = one or more other 42 students, independent from the author.
     - **Moderator/Scribe** = optional roles, combinable with the reviewer for solo/small reviews, but required to describe when they are used vs. skipped.
     - **Manager** = whoever owns the policy/curriculum decision (e.g. pedagogical staff or a designated lead), responsible for ensuring the review step is enforced and resourced.
   - **Independence rule**: a reviewer must not be the author, mirroring the peer-validation independence rule already in the policy.
   - **Exit condition**: what "review passed" means and how it gates progression to peer evaluation (e.g. logged findings resolved or explicitly accepted as risk).
4. Add a short **findings log template** (columns: finding, artifact/location, severity, raised by, status) for recording static review results.
5. Cross-reference this new chapter with the existing peer-validation section so the two review types (static vs. dynamic/peer evaluation) are clearly distinguished and sequenced.
6. Add or update the document control block (version, date, author, reviewer) to reflect the policy change.

## 6. Expected Outcome / Deliverable

A markdown chapter (ready to be inserted into `testPolicy-42Vienna.md`) titled **"Static Review of 42-Project Artifacts"**, containing: purpose/trigger, scope of review, a roles-and-responsibilities table (author, reviewer, moderator/scribe, manager), the independence rule, the exit condition, and a findings log template.

## 7. Constraints

- Do not modify or replace the existing peer-validation (dynamic testing) content — this task only **adds** a static review chapter and cross-references it.
- Keep the chapter specific to the 42 curriculum context; do not reuse ToolShop/commercial review examples.
- Roles may be combined for small/solo reviews, but the chapter must state explicitly which combinations are allowed and why.

## 8. Acceptance Criteria

- [ ] All five ISTQB review roles (author, moderator, reviewer, scribe, manager) are addressed, with 42-specific ownership assigned to each.
- [ ] Independence of the reviewer from the author is explicitly required.
- [ ] The chapter states which artifacts are in scope for static review and when the review must occur.
- [ ] A findings log template is included.
- [ ] The static review chapter is clearly distinguished from, and cross-referenced with, the existing peer-validation (dynamic) section.
- [ ] Document control metadata is updated.

## 9. Hints (Optional)

- For small/solo static reviews, the moderator and scribe roles can be merged into the reviewer role — state this explicitly rather than omitting the roles.
- Use a concrete 42-Project example (e.g. `push_swap` Norm/mandatory-part check, `webserv` config/README check) to illustrate the review scope.
- Keep the new chapter at the same level of formality as the rest of the policy — this is a lightweight peer/self-managed review, not a full formal inspection.

## 10. References

- ISTQB FL 3.2.2 Review Process Activities
- ISTQB FL 3.2.3 Roles and Responsibilities in Reviews
- `courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter 1/ISTQB-FL-1_Test_Policy_42Vienna_20260706.md`

## 11. Estimated Duration

NA
