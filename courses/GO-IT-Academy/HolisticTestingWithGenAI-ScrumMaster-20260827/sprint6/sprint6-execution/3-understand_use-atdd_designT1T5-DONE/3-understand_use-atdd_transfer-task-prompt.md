# **Title**

Hands-on: T1T5 test design for ToolShop business use cases (you write the prompts)


# **Background**

The **ToolShop** is the system under test. The sprint team needs **acceptance-oriented test thinking** for **business use cases** that are realistically affected by the sprint’s implemented user stories. You apply the **T1T5 test-design technique** so scenarios are explicit, comparable, and traceable to product behaviour—not to vague “check everything” wishes. This handout is intentionally **not** a 1:1 model prompt: you choose goals, inputs, output shape, constraints, and follow-up questions when you involve generative AI.


# **Reference**

- T1T5 technique description: `sprint6-execution/3-understand_use-atdd_designT1T5-DONE/T1T5-testdesign-description.md`
- Official ToolShop feature list (basis for naming and scoping use cases): `sprint6-execution/3-understand_use-atdd_designT1T5-DONE/toolshop-official-feature-list.md`
- Sprint framing: `sprint6/sprint6-input/sprint6-content.md`, `sprint6/sprint6-input/sprint6-sprintGoal.md`
- Implemented user stories (pick up scope and dependencies from these): `sprint6/sprint6-input/` (individual `US*.md` files)


# **Tasks**

1. Read the sprint goal, sprint content summary, and the user stories that matter for this sprint; skim the official feature list so your **business use case** names and boundaries match product language.

2. From the official feature list, derive **candidate business use cases** (for example flows such as registration or checkout—your labels should be defensible against the feature list, not invented fantasy scope).

3. Select **five** business use cases that are **clearly influenced** by what the sprint stories change or touch; briefly justify each link (which story, which risk or behaviour shifts).

4. Study the T1T5 description until you can classify examples yourself; then design **T1T5-style tests** for your chosen scope so that your overall design includes **at least one** illustration of **each** of **T1, T2, T3, T4, and T5** (distribution across the three use cases is acceptable if you explain it).

5. Consolidate **your** final artefact (table, outline, or equivalent) with columns or fields you find useful: use case, T1T5 type, test idea, expected observable outcome, trace to feature list and user story.

9. OPTIONAL: Execute a **small subset** of the tests in a manual way (not automated!!!) against a running ToolShop and note mismatches between expected and actual results.
