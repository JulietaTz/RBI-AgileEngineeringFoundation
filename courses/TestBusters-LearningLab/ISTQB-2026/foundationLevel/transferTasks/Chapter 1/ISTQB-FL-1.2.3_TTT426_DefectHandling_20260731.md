# Define How the TTT426 Team Handles Bugs & Defects

## 1. Title

Define How the TTT426 Team Handles Bugs & Defects

## 2. Syllabus Reference

ISTQB FL – 1.2.3 Errors, Defects, Failures, and Root Causes

## 3. Learning Objective

Apply ISTQB FL section 1.2.3 concepts (error, defect, failure, root cause) to the TTT426 project context by describing and structuring the defect handling approach used by the TTT426 / QualityQuest team — covering discovery, documentation, assessment, resolution, and release decisions.

## 4. Context / Scenario

The TTT426 team (trading as QualityQuest Consulting) is testing the ToolShop web application at https://holtesting.practicesoftwaretesting.com.

During sprint activities, defects are continuously found, reported, and managed. New team members need a clear, concise reference that describes exactly how the team handles defects — from the moment a failure is observed to the moment the fix is confirmed and the release decision is made.

The team's Test Policy (QualityQuest Test Policy, section 2.2.7) and the Zero-Bug Policy (section 2.2.7.1) are the authoritative sources. The bug documentation format is defined in `/testwareTTT426/bugDrivenTestAutomation.md`. GitHub Issues is the approved defect tracking tool.

## 5. Task Instructions

1. Read the relevant sections of the QualityQuest Test Policy:
   - Section 2.2.7 Defect Management
   - Section 2.2.7.1 Zero-Bug Policy
   - Section 3.4 Defect Report

2. Map the ISTQB FL 1.2.3 terms to the TTT426 context:
   - **Error** – what kind of human mistake causes defects in the ToolShop project?
   - **Defect** – how does the team define and classify a defect?
   - **Failure** – when does a defect become an observable failure in ToolShop?
   - **Root Cause** – how does the Zero-Bug Policy address root cause analysis?

3. Describe the TTT426 defect lifecycle as a numbered process from discovery to closure. Include the role responsible at each step and the expected artifact.

4. Describe the severity classification rules and the corresponding release criteria (which defects block a release, which require PO approval, which may be deferred).

5. Describe how GitHub Issues is used: what fields must be populated when filing a defect?

6. Write a one-paragraph summary of the Zero-Bug Policy in your own words — suitable for onboarding a new team member.

## 6. Expected Outcome / Deliverable

A sprint-ready markdown document containing:

- ISTQB term mapping table (Error / Defect / Failure / Root Cause → TTT426 example)
- Numbered defect lifecycle (step, responsible role, artifact)
- Severity classification table with release criteria
- GitHub Issue required fields checklist
- Zero-Bug Policy summary paragraph (in own words)

## 7. Timebox
NA

## 8. Hints (Optional)

- Source documents to consult:
  - `courses/TestBusters-LearningLab/ISTQB-2026/TTT426-the-testproject/testwareTTT426/testPolicy_QualityQuest.md` (sections 2.2.7, 2.2.7.1, 3.4)
  - `courses/TestBusters-LearningLab/ISTQB-2026/TTT426-the-testproject/testwareTTT426/bugDrivenTestAutomation.md`
  - `courses/TestBusters-LearningLab/ISTQB-2026/TTT426-the-testproject/backlogTTT426/US1300-TestPolicy-ZeroBugPolicy-Adjustments.md`
- Do not invent severity levels or resolution rules. Derive them from the policy documents.
- The bug example `bug102-invalid-firstname-account-creation.md` in the `testbasisTTT426/issues/` folder is a concrete reference for the required GitHub Issue format.
- Keep the term mapping grounded: use a real ToolShop feature flow as the example (e.g., account registration, product search, checkout).

## 9. References

- ISTQB FL Syllabus v4.0.1 – Section 1.2.3 Errors, Defects, Failures, and Root Causes
- QualityQuest Test Policy – Sections 2.2.7, 2.2.7.1, 3.4
- `testwareTTT426/bugDrivenTestAutomation.md`
- `backlogTTT426/US1300-TestPolicy-ZeroBugPolicy-Adjustments.md`
- ToolShop application: https://holtesting.practicesoftwaretesting.com

## Estimated Duration

30 minutes
