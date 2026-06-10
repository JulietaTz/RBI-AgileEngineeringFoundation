# 1. Title

Select the Right LLM Type for Different Test Tasks

---

# 2. Syllabus Reference

ISTQB GenAI – 1.1.3 Foundation, Instruction-Tuned and Reasoning LLMs

---

# 3. Learning Objective

Differentiate model families and choose an appropriate model type for common QA activities.

---

# 4. Context / Scenario

The organization offers multiple LLM options. Testers need a practical rule set for choosing a model type based on task complexity and expected output quality.

The three QA tasks analyzed are:
- **Requirement summarization**
- **Test-case generation**
- **Defect root-cause reasoning**

---

# 5. Decision Matrix

## Task 1: Requirement Summarization

| Model Type | Precision | Consistency | Explainability | Effort |
|---|---|---|---|---|
| Foundation | Low – output is unpredictable without guidance | Low – needs prompt engineering | Low – no structure | High – requires heavy prompting |
| Instruction-Tuned | High – follows summarization instructions well | High – reliable output format | Medium – structured but no reasoning shown | Low – ready to use |
| Reasoning | Medium – may over-analyze simple summaries | Medium – adds unnecessary steps | High – explains every decision | Medium – overkill for simple task |

**✅ Decision Rule:**
> If the task is **requirement summarization**, then use an **instruction-tuned model** — it follows formatting instructions reliably and produces consistent, structured output with minimal effort.

---

## Task 2: Test-Case Generation

| Model Type | Precision | Consistency | Explainability | Effort |
|---|---|---|---|---|
| Foundation | Low – generates generic or incomplete test cases | Low – output varies significantly | Low – no rationale provided | High – requires many prompt iterations |
| Instruction-Tuned | High – produces well-structured test cases from requirements | High – follows templates consistently | Medium – output is clear but reasoning is implicit | Low – works well out of the box |
| Reasoning | High – considers edge cases and boundary conditions | Medium – may vary in depth | High – explains why each test case is relevant | Medium – worth the effort for complex features |

**✅ Decision Rule:**
> If the task is **test-case generation for complex features**, then use a **reasoning model** — it identifies edge cases and explains its logic. For standard test cases, an **instruction-tuned model** is sufficient.

---

## Task 3: Defect Root-Cause Reasoning

| Model Type | Precision | Consistency | Explainability | Effort |
|---|---|---|---|---|
| Foundation | Very Low – cannot reliably analyze logs or trace causes | Very Low – highly inconsistent | Very Low – no structured reasoning | Very High – not suitable |
| Instruction-Tuned | Medium – can identify surface-level causes if well prompted | Medium – depends on prompt quality | Low – conclusions without justification | Medium – needs careful prompting |
| Reasoning | High – systematically traces through symptoms to root cause | High – follows logical steps | High – shows full reasoning chain | Low – designed for this type of task |

**✅ Decision Rule:**
> If the task is **defect root-cause reasoning**, then use a **reasoning model** — it systematically analyzes symptoms, traces causes, and provides an explainable conclusion that the team can verify.

---

# 6. Governance Recommendation

> **Always document which model type was used, with which prompt version, and on which date** — AI-generated QA outputs (test cases, summaries, root-cause analyses) must be traceable. A tester must review and approve every AI-generated artifact before it enters the test suite or defect report. No model output should be used as-is without human validation.

---

# 7. Expected Outcome / Deliverable

A decision matrix and 3 clear model-selection rules — see Section 5 above.

---

# 8. Timebox

50 minutes
