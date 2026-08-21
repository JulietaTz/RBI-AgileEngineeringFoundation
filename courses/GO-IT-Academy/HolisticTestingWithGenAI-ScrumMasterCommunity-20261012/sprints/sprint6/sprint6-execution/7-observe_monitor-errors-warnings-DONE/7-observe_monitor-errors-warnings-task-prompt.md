# **Title**

Hands-on: Observability and production payment logs (you write the prompts)


# **Background**

The **ToolShop** is the system under test. A new **payment gateway** is live in **production**; non-production environments only offered a dummy integration, so critical validation and monitoring reasoning must lean on **production evidence**. Available artefacts include **execution logs** that mix automated checks, payment activity, errors, timings, and success or failure signals. This handout is intentionally **not** a 1:1 prompt: you must choose goals, scope, output structure, and follow-up questions yourself when you involve a generative assistant.


# **Reference**

- Sprint context: `sprint6-input/sprint6-content.md`, `sprint6-input/sprint6-sprintGoal.md`
- User stories: `sprint6-input/USxx.md`
- Log material for this theme: `sprint6-execution/7-observe_monitor-errors-warnings-DONE/7-observe_monitor-errors-warnings/test*.log`


# **Tasks**

1. Read the sprint materials and relevant user stories so you know what “good” payment behaviour and release expectations mean for ToolShop in this sprint.

2. Inspect the provided `test*.log` files yourself first; note what signal types appear (pass/fail, errors, amounts, currencies, timings, environment-specific hints).

3. Author **prompt A** for one model or provider, aimed at turning raw log evidence into **structured analytical insight** (you define what “structured” means and which hypotheses the model should check).

4. OPTIONAL: Author **prompt B** for a **different** model, provider, or instruction style; avoid pasting one long prompt unchanged—adapt so you learn what changes the depth or quality of the analysis.

5. Ensure your prompting strategy eventually supports both **technical log interpretation** (patterns, anomalies, recurring failures, timeouts, integration symptoms) and **product or release reasoning** (business and customer impact, reliability, risks, release posture).

6. Produce **your** consolidated artefact: a concise **test or observability summary** suitable for stakeholders, including an explicit **release recommendation** among *Go*, *Go with Risks*, or *No-Go*, each tied to evidence from the logs—not generic boilerplate.

7. Submit **the prompts as you actually used them**, your consolidated artefact, and a **short reflection** on what worked in prompting, what misled the model, and what you would change before the next production incident or release gate.

8. List **which log files** your conclusions relied on.
