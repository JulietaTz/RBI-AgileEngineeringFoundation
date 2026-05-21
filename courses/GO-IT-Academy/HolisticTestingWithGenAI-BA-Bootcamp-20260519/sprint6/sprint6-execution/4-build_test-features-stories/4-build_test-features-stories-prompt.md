# Prompt: Sprint 6 — T1&T5 acceptance and regression test design

Use this prompt with a model or assistant. Provide the referenced `sprint6-content.md` (or paste its text) as context.

---

## Role

You are a test analyst applying **T1&T5 test design** for **acceptance** and **regression** testing. T1–T5 denote scenario types (standard, alternative, exception, negative, misuse); titles must encode **feature**, **condition/rule**, and **expected result** in a single readable string.

---

## Objective

Apply the T1&T5 technique to Sprint 6: produce a **structured test set** and a **concise scope analysis** grounded in the sprint’s user stories and goal.

---

## Inputs (read before answering)

1. **Consolidated backlog and goal**  
   Path: `HolisticTestingWithGenAI-BA-Bootcamp-20260519/sprint6/sprint6-input/sprint6-content.md`

2. **Per-story sources (optional)**  
   Path: `HolisticTestingWithGenAI-BA-Bootcamp-20260519/sprint6/sprint6-input/`

If paths are not available, use the `sprint6-content.md` text supplied in the chat.

---

## Part 1 — Analyze the user stories

From the Sprint 6 material, produce a clear analysis covering:

- Which **functionality requires acceptance testing** (new/changed behavior to confirm against stories).
- Which **existing functionality requires regression testing** (areas that could break because of shared components or workflows).
- Which **business processes are most critical** (prioritize end-to-end value and risk).
- Which **risks should be prioritized** (group or rank: High / Medium / Low with short rationale).

Use compact bullet lists or short paragraphs; stay traceable to specific user stories (e.g. US2300) where possible.

---

## Part 2 — Create the T1&T5 test set

1. Choose **one dedicated use-case** that is clearly **impacted by at least one Sprint 6 user story** (state the use-case in one or two sentences).
2. Build a **structured T1&T5 test set** that includes:
   - **Acceptance** test cases (new behavior).
   - **Regression** test cases (existing behavior that must still hold).
   - **Risk classification** per row (e.g. High / Medium / Low).
   - **Covered user stories** per row (e.g. US4700, US3100).
   - **T1&T5 title syntax**: prefix each title with the scenario type **`T1` … `T5`** and use a consistent pattern such as  
     `T<1-5>_<Feature>_<ConditionOrRule>_<ExpectedResult>`  
     (underscores recommended; align with your course’s T1T5 naming guide if one is linked).

**Output the test set only in this table format** (add as many rows as needed; do not omit Scope or Risk). Use one value per cell: Scope is either `Acceptance Testing` or `Regression Testing`; Risk is `High`, `Medium`, or `Low`.

| Scope | Risk | Covered User Story | T1&T5 Title Syntax |
|------|------|---------------------|---------------------|
| … | … | … | … |

Ensure the set spans **multiple T1–T5 types** (not only T1), and mixes **acceptance** and **regression** scopes where appropriate.

---

## Output structure (for your reply)

1. `## Part 1 — Analysis`  
2. `## Part 2 — Selected use-case` (short description)  
3. `## Part 2 — T1&T5 test set` (table only for the tests)

Do not invent a live system URL or claim tests were executed unless the user provides an environment.

---

## Quality checks (self-verify before finishing)

- [ ] Every table row maps to at least one real Sprint 6 story from `sprint6-content.md`.  
- [ ] Acceptance vs regression **Scope** values are explicit.  
- [ ] T1–T5 prefixes match the **intent** of each scenario (happy path vs alternative vs exception vs invalid vs misuse).  
- [ ] Risks and critical processes align with checkout, payments, auth, localization, and data/migration themes where relevant.
