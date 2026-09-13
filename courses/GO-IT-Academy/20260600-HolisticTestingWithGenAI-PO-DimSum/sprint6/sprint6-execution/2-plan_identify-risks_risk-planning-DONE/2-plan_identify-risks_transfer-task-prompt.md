# **Title**

Hands-on: Risk-based test planning for the next sprint (you write the prompts)


# **Background**

The **ToolShop** is the system under test. The product team is preparing the next sprint with **limited testing capacity**, so not every user story can receive the same depth of test design and execution. You will use GenAI to support **risk assessment**, **prioritisation of testing effort**, and **comparison of model behaviour**—but this handout is intentionally **not** a 1:1 prompt. You must choose goals, context, output shape, constraints (for example MoSCoW slot counts), and follow-up questions yourself.


# **Reference**

- Course transfer task: `sprint6-execution/2-plan_identify-risks_risk-planning-DONE/2-plan_identify-risks_transfer-task.md`
- Sprint framing: `sprint6/sprint6-input/sprint6-content.md`, `sprint6/sprint6-input/sprint6-sprintGoal.md`
- User stories (inputs): `sprint6/sprint6-input/` (individual story files)
- GenAI: use approved course tooling (for example RBI Chat GPT) plus at least one other model or provider where policy allows, so comparisons are meaningful


# **Tasks**

1. Read the sprint goal and the bundled sprint content, then review the **ToolShop** user stories in `sprint6-input`. Decide which dimensions matter for **testing risk** in this sprint (for example business impact, technical complexity, integration, security, production impact, user visibility, regression).

2. Author **prompt A** for one model or provider, asking the model to help you **analyse risks** and **prioritise testing** for these stories. You define what “risk” means in the prompt, what evidence from the files the model should use, and what structured output you want.

3. Apply **MoSCoW** to the stories with these slot targets: **MUST 3**, **SHOULD 1**, **COULD 1**, **WON’T 4**. Your prompts and your own judgement should align with that portfolio; document **your** reasoning, not only the model’s.

4. OPTIONAL: Author **prompt B** for a **different** model or provider (or a clearly different instruction style). Avoid pasting one mega-prompt unchanged—adapt so you learn what changes the answers.

5. Compare outputs across runs or models on: risk picture, prioritisation, reasoning quality, identified risks, and suggested testing focus. Note disagreements with your own view.

6. Produce **your** consolidated artefact: a **table** of user stories with MoSCoW band, main testing risks, and suggested testing focus for the sprint.

7. Submit **your prompt(s)** as you actually used them, the consolidated table, and a **short reflection** on model differences and what you would change in prompting next time.
