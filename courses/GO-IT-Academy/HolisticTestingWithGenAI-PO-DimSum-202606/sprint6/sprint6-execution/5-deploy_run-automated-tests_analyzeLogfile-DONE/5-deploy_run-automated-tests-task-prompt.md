# **Title**

Hands-on: Interpret ToolShop automation logs and improve test reliability (you write the prompts)


# **Background**

The **ToolShop** is the system under test. Automated checks run in pipelines or locally and leave **execution logs**. A red run is not always a product defect: flaky locators, shared state, weak assertions, ordering assumptions, slow runs, and unclear diagnostics can distort signal for POs, analysts, testers, and developers. This handout is intentionally **not** a 1:1 prompt: you choose how you use a model (goals, log excerpts, output schema, and follow-up questions) and you own the wording of every prompt.

# **Reference**

- Log material to work from: `5-deploy_run-automated-tests_analyzeLogfile/ta*.log`

# **Tasks**

1. Open the referenced log files and skim structure, timestamps, failure markers, and any stack traces or assertion text; note what is **fact** in the log versus what you **infer** about causes.

2. Without pasting this sheet into a model, author **prompt A** for one model/provider that helps you surface **test-suite quality** issues (not only “the app broke”), e.g. instability, order dependence, missing or vague checks, duplication, runtime hotspots, or gaps versus acceptance criteria—**you** define scope, excerpts, and desired output shape.

3. OPTIONAL: Author **prompt B** for a **different** model, provider, or instruction style; avoid reusing one long prompt verbatim—adapt so you learn what changes the analysis.

4. From log evidence and model-assisted reasoning where you chose to use it, build **your** prioritized improvement list: each row should tie an **observation** from the log, a **suspected test-layer cause**, and **one concrete next step** (examples of step types: stabilize locator, replace blind waits with an explicit condition, isolate data, assert on user-visible outcome, refactor for independence—pick what fits each case).

5. Deliver a **single consolidated table** (or equivalent artefact) that a sprint could pick up as backlog hints for stabilizing automation.

6. Submit **your prompt(s)** as you actually ran them, the consolidated artefact, and a **short reflection** on what differed between approaches (depth, false positives, actionability, format) and what you would change in your prompts next time.
