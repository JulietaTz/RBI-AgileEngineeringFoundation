# Selecting Prompting Techniques — Short Exercise

**Syllabus Reference**
ISTQB GenAI — 2.2.5 Selecting Context-Appropriate Prompting Techniques for Software Testing

**Learning Objective**
Quickly choose a suitable prompting technique for simple testing tasks and produce a one-line sample prompt.

**Context / Scenario**
You are a QA team member for the Toolshop webshop. The team wants fast, low-effort ways to use GenAI assistants for common small testing tasks during a sprint.

**Task Instructions**
Work individually for 15 minutes. For each of the three mini tasks below do the following:
- Name the primary need in one phrase (precision / repetition / summarization / reasoning).
- Pick one prompting technique (few‑shot, constrained output, chunking/RAG, role prompt, chain‑of‑thought).
- Write a single-line sample prompt tuned to the task and list one recommended model setting (e.g., temperature 0.0).

Mini tasks:
1) Price spot checks: generate 5 concise test inputs to validate checkout totals for discounts and taxes.
2) Accessibility checklist item: produce a short WCAG checklist (3 items) for a product details page.
3) Reviews summary: summarize sentiment for 50 recent reviews into 3 bullet points.

**Expected Outcome / Deliverable**
Three short entries (one per mini task) containing:
- Primary need
- Chosen technique
- One-line sample prompt
- Model setting

Example deliverable (one line per task):
- Task 1 — Precision; technique: constrained JSON + few‑shot; prompt: "Return 5 JSON test cases with fields id, inputs, expected_total for these discount rules: ..."; setting: temperature 0.0

**Timebox**
15 minutes total.

**Hints**
- Keep prompts terse and explicit. Use temperature 0.0 for deterministic outputs. For lists, request JSON or CSV for easy validation.
