# Title of the transfer task

1.1.3 – Select the Right LLM Type for Different Test Tasks

---

# Reference to ISTQB Syllabus Chapter

ISTQB GenAI – 1.1.3 Foundation, Instruction-Tuned and Reasoning LLMs

---

# Link to the Transfer Task File

https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks/ISTQB-GenAI-1.1.3_Foundation_Instruction-Tuned_and_Reasoning_LLMs.md

---

# Outcome

The organization offers multiple LLM options. For sprint planning, the team needs a clear rule for which model type to use per QA task. Three representative tasks were analyzed: requirement summarization, test-case generation, and defect root-cause reasoning.

## Decision Matrix

| QA Task | Foundation LLM | Instruction-Tuned LLM | Reasoning LLM | Recommendation |
|---|---|---|---|---|
| Requirement summarization | ❌ Unpredictable | ✅ Ideal | ➖ Overkill | Instruction-Tuned |
| Test-case generation | ❌ Too generic | ✅ Good for standard | ✅ Ideal for complex | Depends on complexity |
| Defect root-cause reasoning | ❌ Not suitable | ➖ Surface-level only | ✅ Ideal | Reasoning |

## Decision Rules

**If** the task is requirement summarization → **use Instruction-Tuned LLM** — consistent, structured output with minimal effort.

**If** the task is standard test-case generation → **use Instruction-Tuned LLM** — follows templates reliably.

**If** the task is complex test-case generation or defect root-cause analysis → **use Reasoning LLM** — identifies edge cases and explains its logic step by step.

## Governance Recommendation

Always document which model type was used, with which prompt version, and on which date. Every AI-generated artifact must be reviewed and approved by a tester before it enters the test suite or defect report.

---

# Learning Summary

| Concept | One-line takeaway |
|---|---|
| Foundation LLM | Broad knowledge but unpredictable — not suitable for direct QA use without heavy prompting |
| Instruction-Tuned LLM | Follows instructions reliably — best for structured, repetitive tasks |
| Reasoning LLM | Thinks step by step — essential for complex analysis and edge case detection |
| Requirement summarization | Instruction-Tuned wins — consistent and low effort |
| Test-case generation | Instruction-Tuned for standard; Reasoning for complex features |
| Defect root-cause analysis | Reasoning LLM — shows full reasoning chain, results are verifiable |
| Governance | Document model, prompt version and date — human approval always required |

*Treat every AI-generated test artifact as an unreviewed first draft from a junior colleague who has never seen your system.*
