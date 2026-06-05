# 1. Title

Explain How LLM Output Is Produced in a Testing Context

---

# 2. Syllabus Reference

ISTQB GenAI – 1.1.2 Foundations of Generative AI and LLMs

---

# 3. Learning Objective

Describe core LLM mechanics and connect them to practical implications for test analysis and design.

---

# 4. Context / Scenario

Sample prompt used: **"Generate boundary value test ideas for cart quantity update."**

This prompt was sent to an LLM to generate test cases for the Toolshop cart feature. Below is an explanation of how the LLM processes this prompt and what testers need to know.

---

# 5. How LLM Output Is Produced

## Tokenization

The prompt is split into small units called **tokens** — roughly words or parts of words.  
Example: `"Generate boundary value test ideas for cart quantity update"` becomes ~9 tokens.  
The LLM does not read full sentences — it works with these tokens as numbers.

**Testing implication:** Unusual words or typos in your prompt may be tokenized unexpectedly, leading to worse output quality.

---

## Context Window Usage

The LLM can only "see" a limited amount of text at once — this is called the **context window**.  
Everything inside the window (your prompt + previous messages) influences the response.  
If the context window is full, earlier content gets cut off.

**Testing implication:** In long test sessions with an AI tool, earlier requirements may be forgotten — always re-state important constraints.

---

## Next-Token Prediction

The LLM generates output **one token at a time**, always predicting the most likely next token based on what came before.  
It does not "think" or "understand" — it statistically predicts the best continuation.

**Testing implication:** The output always sounds fluent and confident — even when it is wrong. Testers must verify content, not just readability.

---

## Why Non-Determinism Can Occur

Even with the same prompt, the LLM may produce **different outputs** each time.  
This is because a temperature setting introduces randomness into the token selection process.

**Testing implication:** AI-generated test cases cannot be treated as a stable, reproducible artifact — always review and document them manually.

---

## Hallucination

The LLM may generate **plausible-sounding but incorrect information** — this is called hallucination.  
For example, it might invent a boundary value rule that does not exist in the actual requirements.

**Testing implication:** Never use AI-generated test cases without human review — especially for boundary values, which must match the real specification.

---

# 6. Consequences for Testers When Validating AI-Generated Test Content

1. **Fluency ≠ Correctness** — A well-written test case can still be factually wrong. Testers must cross-check every AI-generated test case against the actual requirements.
2. **Non-reproducibility** — The same prompt may yield different test cases on different runs, making it hard to track what was tested and when.
3. **Hidden gaps** — The LLM may miss edge cases that are not explicitly mentioned in the prompt, giving a false sense of complete test coverage.

---

# 7. Quality Controls the Team Should Always Apply

1. **Human review of every AI-generated test case** — A tester must compare each generated test case against the official requirements or acceptance criteria before adding it to the test suite.
2. **Prompt versioning and documentation** — Always save the exact prompt used, the date, and the model version so the team can trace where test cases came from and reproduce the context if needed.

---

# 8. Expected Outcome / Deliverable

A markdown note with one short process description and a practical control checklist.

---

# 9. Timebox

60 minutes
