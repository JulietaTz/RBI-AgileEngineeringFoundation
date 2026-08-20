# **Title**

Hands-on: Hypothesize and adapt the ToolShop test automation strategy (you write the prompts)


# **Background**

The **ToolShop** is the system under test. Sprint 6 produced evidence across discovery, planning, design, build, deploy, release, and observe activities. The **Learn** stage asks you to turn that evidence into **automation priorities** and an **updated test automation strategy**, including how upcoming goals (for example **United States and China expansion** and **customer growth**) should influence what you automate, monitor, and keep manual or exploratory. This handout is intentionally **not** a 1:1 prompt: you choose how much context to paste, how to phrase questions, what output shape to request, and how to iterate. The point is to practise **prompt design** and **critical review** of model output against real sprint artefacts—not to paste a single supplied instruction into a chat.


# **Reference**


- Sprint narrative and scope: `sprint6/sprint6-input/sprint6-content.md`, `sprint6/sprint6-input/sprint6-sprintGoal.md`
- User stories (as used in the sprint): `sprint6/sprint6-input/US*.md`
- Defect list: `sprint6/sprint6-input/list-of-bugs.md`
- Current automation strategy baseline: `sprint6/sprint6-input/testAutomationStrategy.md`
- Prior T1&T5 design notes (if you produced or were given them): `sprint6-execution/3-understand_use-atdd_designT1T5-DONE/` (for example `T1T5-testdesign-description.md`)
- Example execution and observation signals: `sprint6-execution/5-deploy_run-automated-tests_analyzeLogfile-DONE/*.log`, `sprint6-execution/7-observe_monitor-errors-warnings-DONE/test*.log`


# **Tasks**

1. Assemble a **short evidence pack** for yourself (bullet list or table is enough): critical defects, high-risk workflows, regression-prone areas, production or log signals, stable versus unstable features, and business-critical journeys—**grounded in the Reference files**, not from memory alone.

2. Author **prompt A** to a model of your choice. Its purpose is to help you **synthesize** that evidence into conclusions: what worked, what created risk, where automation would add the most value, and what should stay manual or exploratory. You decide structure, tone, and whether you attach excerpts versus summaries.

3. OPTIONAL: Author **prompt B** for a **different** model or provider, or with a clearly different style (for example stricter “show your assumptions” versus “executive summary only”). Avoid reusing one long prompt verbatim; change goals or constraints on purpose.

4. Using your own reasoning **and** any model output you trust after verification, produce a **prioritised list of exactly ten** T1&T5-style tests you would automate next. For each item, capture in your own words: why automate it, which risk it mitigates, which business value it protects—emphasising business impact, regression risk, frequency, production stability, and security-sensitive behaviour where relevant.

5. Draft an **updated test automation strategy** for ToolShop that explicitly reflects sprint 7–8 direction (**US and China markets**, **customer growth**): automation sequencing, what remains exploratory or manual, and what should be **continuously monitored** or fed by feedback loops. Integrate this as edits or a replacement section to `testAutomationStrategy.md`-level thinking (you do not need to commit file changes unless your course asks for it).

6. Build **your** outcome table with exactly these columns: **feature or use case to cover**, **T1&T5 test title**, **implementation order (1–10)**, **reason**. Numbers 1–10 must match your prioritisation; tighten titles so another tester could recognise the scenario.

7. Submit **the prompt(s) as you actually used them**, the **evidence pack**, the **table**, the **strategy delta** (what changed versus the baseline and why), and a **short reflection**: what you would change in your prompts next time, and where the model was wrong or unsafe without your file-based checks.
