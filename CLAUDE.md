# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a **learning content repository** — not a software project. It contains course materials, agent configurations, and transfer tasks for the RBI Agile Engineering Foundation Program (delivered at 42 Vienna and RBI facilities). There is no build system, test runner, or deployable application here.

## Repository Structure

```
courses/
  GO-IT-Academy/
    agile-engineering-foundation-program/   # LE001/LE002 core program materials
    HolisticTestingWithGenAI-*/             # Course runs (PO, Scrum Masters, etc.)
    TestDataManagement-2026/                # Test data & data masking materials
  TestBusters-LearningLab/
    ISTQB-2026/                             # ISTQB Foundation + TAE transfer tasks
      genAI/transferTasks-Outcome/          # Learner-submitted outcomes
      testAutomationEngineer/transferTasks-Outcome/
    BMAD-ToolShop-2026/                     # BMAD planning + implementation artifacts
      _bmad-output/planning-artifacts/      # PRD, architecture, epics
      _bmad-output/implementation-artifacts/ # Story-level specs
.cursor/
  agents/       # Cursor AI agent definitions
  rules/        # Cursor rule files
  skills/       # Cursor skill definitions
.github/
  agents/       # GitHub Copilot agent definitions
```

## Content Types and Conventions

### Transfer Tasks (TRATAS)
Transfer tasks are the primary learning artifact. Structure:
1. Title
2. Syllabus Reference
3. Learning Objective
4. Context / Scenario
5. Task Instructions
6. Expected Outcome / Deliverable
7. Timebox (default: 30 min)
8. Hints (optional)

Filename convention: `ISTQB-<section>-<short-title>_<date>_<author>.md`
Example: `TAE-3.1.1_Create_Test_Automation_Strategy_Dilek_20260524.md`

### System Under Test (SUT)
All ISTQB/testing transfer tasks reference the **Toolshop** practice app:
- App: `https://practicesoftwaretesting.com/`
- Docs: `https://github.com/testsmith-io/practice-software-testing/tree/main/docs`
- Sprint5 release: `https://github.com/testsmith-io/practice-software-testing/tree/main/sprint5`

### Black Stories
Files in `.cursor/agents/blackstory-designer-agent.md` and `.cursor/rules/blackstory-rules.md` define the format. Every Black Story requires: Title, Teaser (max 3 sentences), Hidden Solution (max 3 sentences), Investigation Path, Learning Goal, Difficulty (Easy/Medium/Hard/Brutal). No supernatural explanations or random twists — always logical and realistic.

## Agent Configurations

Two types of agent configs exist in parallel:
- **`.cursor/agents/`** — Cursor IDE agents
- **`.github/agents/`** — GitHub Copilot agents

Key agents:
- `blackstory-designer-agent.md` — creates Black Stories for workshops
- `tratas-instructional-designer-agent.md` / `InstructionalDesigner-for-ISTQB.agent.md` — generates ISTQB transfer tasks
- `ISTQB-TestAutomationEngineer-Coach.agent.md` — CTAL-TAE syllabus coach (strictly syllabus-grounded, no hallucination)

## Working with Course Content

When creating or editing transfer tasks, always align output with the TRATAS structure above. When creating Black Stories, follow the rules in `.cursor/rules/blackstory-rules.md` strictly. Learner-submitted outcomes live in `transferTasks-Outcome/` subdirectories and should not be modified.
