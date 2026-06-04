# TestBusters-LearningLab (TBLL) - Agile Engineering Foundation Program

Learning content repository for the **Agile Engineering Programs** and related courses at the TestBusters-LearningLab,  [42 Vienna](https://42vienna.com/) and RBI trainings. This repo holds course materials, transfer tasks (TRATAS), AI agent configurations, and practice scenarios — not deployable application code.

## Repository overview 


| Area                | Purpose                                                                  |
| ------------------- | ------------------------------------------------------------------------ |
| `courses/`          | Program and workshop materials (GO-IT Academy, TestBusters Learning Lab) |
| `systemsUnderTest/` | Backlog and scenarios for practice applications (primarily **ToolShop**) |
| `templates/`        | TRATAS and prompt templates for authors                                  |
| `.cursor/`          | Cursor IDE agents, rules, and skills                                     |
| `.github/agents/`   | GitHub Copilot agent definitions                                         |
| `generated/`        | Generated prompts and artifacts                                          |


There is no build system or test runner in this repository. Learners work against external systems (e.g. [Practice Software Testing](https://practicesoftwaretesting.com/)) and submit outcomes under `transferTasks-Outcome/` folders.

## Repository structure

```
courses/
  GO-IT-Academy/
    agile-engineering-foundation-program/   # LE001, LE002 (certificates, simulations)
    HolisticTestingWithGenAI-*/             # Workshop runs (PO, Scrum Masters, …)
    TestDataManagement-2026/                # Test data classification and masking
  TestBusters-LearningLab/
    ISTQB-2026/                             # Foundation, GenAI, TAE transfer tasks
    BMAD-ToolShop-2026/                     # BMAD planning and implementation artifacts
    ISTQB-2026-old/                         # Archived materials
systemsUnderTest/
  toolShop/backlog/                         # User stories, features, defects for ToolShop
  onlineBanking/                            # Additional SUT scenarios
templates/                                  # transfer-task-template.md, prompt templates
.cursor/                                    # agents, rules, skills
.github/agents/                             # Copilot agents (ISTQB coach, instructional designer, …)
generated/prompts/
```

### Course tracks in this repo

- **Agile Engineering Foundation** — `courses/GO-IT-Academy/agile-engineering-foundation-program/` (LE001 theory, LE002 hands-on Scrum simulation)
- **ISTQB 2026** — `courses/TestBusters-LearningLab/ISTQB-2026/` with tracks:
  - `foundationLevel/` — CTFL syllabus transfer tasks (~90 tasks)
  - `genAI/` — Generative AI for software testing (~40 tasks)
  - `testAutomationEngineer/` — CTAL-TAE syllabus (~35 tasks)
- **Holistic Testing with GenAI** — workshop artifacts under `HolisticTestingWithGenAI-`*
- **Test Data Management** — masking concepts and SAP HR examples under `TestDataManagement-2026/`
- **BMAD ToolShop** — PRD, architecture, epics, and story specs under `BMAD-ToolShop-2026/_bmad-output/`

### System under test (ToolShop)

Most testing transfer tasks use the **ToolShop** practice app ([Practice Software Testing](https://practicesoftwaretesting.com/)):

- Application: [https://practicesoftwaretesting.com/](https://practicesoftwaretesting.com/)
- Documentation: [https://github.com/testsmith-io/practice-software-testing/tree/main/docs](https://github.com/testsmith-io/practice-software-testing/tree/main/docs)
- Sprint 5 reference: [https://github.com/testsmith-io/practice-software-testing/tree/main/sprint5](https://github.com/testsmith-io/practice-software-testing/tree/main/sprint5)

Extended backlog and teaching scenarios live in `systemsUnderTest/toolShop/backlog/` (features F1–F5, ESG, defects, general stories).

### Content conventions

**Transfer tasks (TRATAS)** follow a standard structure: Title, Syllabus Reference, Learning Objective, Context / Scenario, Task Instructions, Expected Outcome / Deliverable, Timebox (default 30 min), optional Hints. Filename pattern: `ISTQB-<section>-<short-title>_<date>_<author>.md`. Use `templates/transfer-task-template.md` as a starting point.

**Black Stories** (workshop puzzles) are defined in `.cursor/agents/blackstory-designer-agent.md` and `.cursor/rules/blackstory-rules.md`: Title, Teaser, Hidden Solution, Investigation Path, Learning Goal, Difficulty.

**AI agents** (Cursor and GitHub Copilot) support authors and learners — e.g. TRATAS instructional designer, ISTQB TAE coach, Black Story designer, data masking. See `.cursor/agents/` and `.github/agents/`.

Learner submissions are stored in `*/transferTasks-Outcome/` and should not be overwritten by maintainers.

---

## Program content

The program consists of learning blocks (“Lerneinheiten”). Materials for **LE001** and **LE002** are in this repository; **LE003** is described organizationally below but is not yet represented as course content here.

### Objectives

- Provide students at 42 Vienna practical knowledge of agile software development in professional environments
- Emphasize hands-on experience over theory alone
- Show how 42 curriculum skills apply to professional products and services

### Participation

- Parts are divided into sessions (1–4); a minimum number of LE001/LE002 sessions is required to advance
- Session length: 90 minutes to 4 hours
- Program time does not count as extra 42 curriculum project time
- **LE001** is for beginners with no prior agile knowledge
- Registration via 42 Vienna; unexcused absence can mean exclusion from the rest of the program
- RBI interview is required for LE002 and LE003
- Certificate from RBI after successful completion (especially LE003), usable for job applications
- Delivery at 42 Vienna Campus and/or RBI facilities; details via 42 intra and Discord

---

## LE001 — Agile Engineering: Fundamentals

Fundamental agile topics (K1–K2, Bloom: remembering and understanding). Four 90-minute lectures without practical activities.


| Module  | Topic                                                                                       |
| ------- | ------------------------------------------------------------------------------------------- |
| LE001.1 | Agile SDLC (Agile, Scrum, Extreme Programming)                                              |
| LE001.2 | Agile Requirements Engineering (epics, features, user stories)                              |
| LE001.3 | Agile Testing (exploratory testing, session-based testing, tours, specification by example) |
| LE001.4 | DevOps (CI/CD/CD)                                                                           |


**Outcomes:** Solid understanding of agile methodologies, requirements engineering, testing in agile, and DevOps integration.

**Organizational frame:** 4 weeks (1 lecture/week), max 40 participants, first come first served, no prior knowledge required.

### LE001.1 — Agile SDLC

Principles, values, and practices of Agile, Scrum, and XP.

- Understand fundamentals of agile methodologies
- Differentiate Scrum and Extreme Programming
- Identify key practices in Agile SDLC and XP

### LE001.2 — Agile Requirements Engineering

Gathering, managing, and prioritizing requirements in agile projects.

- Importance of flexible requirements (epics, features, user stories)
- Elicitation and prioritization (backlog grooming, story workshops)
- Maintaining agility when requirements change

### LE001.3 — Agile Testing

Continuous testing, ATDD, and automation in agile delivery.

- Role of testing in agile development
- ATDD, exploratory testing, testing tours
- Strategies for test automation in agile

### LE001.4 — DevOps

Development–operations integration for faster, higher-quality delivery.

- Principles and benefits of DevOps
- CI/CD tools and practices
- Cultural shift for successful adoption

---

## LE002 — Agile Engineering: Hands-On Scrum

Builds on LE001. Apply agile engineering in a real Scrum setting (K3–K4: apply and analyze). Team **Sprint Zero** on an open-source GitHub repository: build pipeline (GitHub Actions), Playwright tests, and environments (local, staging, production).

### Practices covered

**Scrum:** 3-amigo elaboration, sprint planning, backlog grooming  

**Agile testing:** Exploratory testing, pair testing, test automation

### Outcomes

Collaborate with RBI coaches in Scrum; implement backlog from Sprint Zero; prioritize work and align with product owner, scrum master, and delivery lead. Execute 2–3 sprints with story elaboration, grooming, planning, review, and retrospective. After LE002 you can discuss agile delivery with stakeholders and connect methodology to your technical background.

Simulation materials for a 42 Vienna cohort: `courses/GO-IT-Academy/agile-engineering-foundation-program/LE002/simulation-20260332-42Vienna/`.

---

## Maintainer notes

- Authoring guidance for AI assistants: see [CLAUDE.md](./CLAUDE.md)
- Cursor rules for instructional design: `.cursor/rules/instructional-designer.mdc`
- Do not commit learner-specific secrets; `.gitignore` excludes `.DS_Store` only

