# Selecting Context-Appropriate Prompting Techniques for Given Test Tasks

**Syllabus Reference**
ISTQB GenAI — 2.2.5 Selecting Context-Appropriate Prompting Techniques for Software Testing

**Learning Objective**
Learners will practice selecting and tailoring prompting techniques (e.g., few‑shot, chain‑of‑thought, constrained output formats, RAG/chunking, templates) to suit different software testing tasks and justify their choices with concise rationales and sample prompts.

**Context / Scenario**
You are part of a Scrum testing team working on the Toolshop webshop (https://practicesoftwaretesting.com/). During the sprint review the PO asks the QA team to use GenAI assistants to accelerate test design, analysis, and reporting. Different test tasks have different needs (precision, repeatability, large context, or investigative reasoning). Your team must pick the prompting technique(s) best suited to each task, produce a short sample prompt (or prompt template), and explain why your choice fits the task.

Related documentation: https://github.com/testsmith-io/practice-software-testing/tree/main/docs (use the sprint5 features for realistic context).

**Task Instructions**
1. Work individually for 20 minutes, then discuss choices in a 10‑minute group review. For each test task below do the following items A–D.
   A. Identify the primary challenge (precision / repetitive structure / large context / investigative reasoning / structured output).
   B. Select 1–2 prompting technique(s) you consider most appropriate (e.g., few‑shot templates, chain‑of‑thought, constrained JSON/CSV output, RAG/chunking, prompt chaining, role/system messages, temperature control).
   C. Produce a concise sample prompt or prompt template tuned for the chosen technique(s). Include any recommended model settings (temperature, max tokens) and output format (JSON, CSV, Markdown table).
   D. Give a one‑sentence rationale and one line of risk and mitigation (e.g., hallucination risk → add context or validation checks).

2. Complete items A–D for each of the five test tasks below.

Test tasks (pick real Toolshop features from sprint5 where helpful):
- Task 1 — Price & Tax Calculation Test Cases (Checkout engine, multi‑currency): requires high precision and machine‑readable test cases.
- Task 2 — Accessibility Exploratory Charters (WCAG checks across product pages): repetitive structured outputs (charter template repeated per page).
- Task 3 — User Reviews Analysis (500+ reviews for a product): large context summarization and trend extraction.
- Task 4 — Regression Test Matrix from User Stories (map acceptance criteria → regression tests): structured mapping, tabular output.
- Task 5 — Investigate Intermittent CI Failure (flaky test logs + stack traces): requires deep reasoning and hypothesis generation.

3. Assemble your deliverable (see below) and be prepared to present a 2‑minute summary for one chosen task during the group review.

**Expected Outcome / Deliverable**
For each task (1–5) deliver a short entry containing:
- **Primary challenge:** (one phrase)
- **Chosen technique(s):** (one or two)
- **Sample prompt/template:** (3–6 lines)
- **Model/settings & output format:** (e.g., temperature 0.0, JSON schema with fields [id,title,steps])
- **Rationale (1 line)** and **Risk & Mitigation (1 line)**

Prefer a compact table or five short bullet blocks (one per task). Example deliverable item:

- **Task 1 — Price & Tax Calculation**
  - Primary challenge: precision, deterministic outputs
  - Chosen technique(s): constrained JSON output + few‑shot examples
  - Sample prompt: "You are a testing assistant. Given the following cart items and tax rules, produce a JSON array of test cases with fields: id, inputs, expected_total. Example: ... [provide 2 examples]. Now generate 8 test cases covering edge cases."
  - Model/settings & format: temperature 0.0, max_tokens 400, JSON
  - Rationale: deterministic JSON easier to validate automatically.
  - Risk & Mitigation: risk — incorrect tax rules → mitigation — include authoritative tax rule snippet and validate with calculator.

**Timebox**
Total: 30 minutes (20 minutes individual work, 10 minutes group discussion). Do not exceed unless facilitator agrees.

**Hints**
- For repetitive structured outputs use a small few‑shot template plus a placeholder list to generate many items consistently.
- For large review corpora use chunking + RAG or iterative summarization (extract key sentences per chunk, then synthesize).
- For investigative tasks prefer chain‑of‑thought or stepwise prompting and feed in relevant logs and reproduction steps; ask for hypotheses ranked by likelihood.
- When you need machine‑readable outputs, require a strict JSON/CSV schema in the prompt and set temperature to 0.0 to reduce variability.
- Always include a short validation step in your prompt (e.g., "Return also a one‑line verification summary with counts of cases generated").

**Facilitator Notes (optional)**
- Encourage short, actionable sample prompts rather than long essays. Keep examples realistic by referencing Toolshop features (checkout, product pages, reviews).
- Use the group review to compare choices and surface tradeoffs: accuracy vs creativity, speed vs verifiability.

---

Filename convention: ISTQB-GenAI-2.2.5_Selecting_Context-Appropriate_Prompting_Techniques_for_Test_Tasks_YYYYMMDD.md
