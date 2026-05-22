# How LLM Output Is Produced — A Testing Team Primer

> **Module:** ISTQB GenAI · 1.1.2 Foundations of Generative AI and LLMs  
> **Purpose:** Understand why AI output can sound right but still be wrong — and what to do about it

---

## The Example Prompt We'll Use Throughout

```
"Generate boundary value test ideas for cart quantity update."
```

We'll return to this prompt repeatedly to make each concept concrete.

---

## Part 1 — How an LLM Processes Your Prompt

### 1. Tokenization — Breaking Your Words Into Pieces

Before the model reads anything, your prompt is split into **tokens** — roughly chunks of 3–4 characters each. Words, parts of words, punctuation, and spaces all become separate tokens.

**Your prompt tokenized (approximate):**

```
["Generate", " boundary", " value", " test", " ideas", " for", 
 " cart", " quantity", " update", "."]
```

**What this means for testers:**  
The model never truly "reads" your sentence as a human would. It processes a stream of fragments. Subtle wording changes — like writing `"qty"` instead of `"quantity"` — can produce noticeably different outputs because the tokens are different. Never assume the model understood your intent; read the output critically.

---

### 2. Context Window — The Model's Short-Term Memory

The **context window** is the total amount of text the model can "hold in mind" at once — both your prompt and its own growing response. Think of it as a whiteboard with a fixed size: once it's full, older content falls off.

**In your test scenario:**  
If you attach a long requirements document before your prompt, parts of it may exceed the context window. The model will generate test ideas without having "seen" the full spec — and won't tell you it missed anything.

**What this means for testers:**  
Long prompts with multiple documents attached can silently lose earlier context. If you're feeding requirements + examples + a question, keep inputs tight and verify the output covers everything you expected it to reference.

---

### 3. Next-Token Prediction — Controlled Guessing

This is the core mechanic. At every step, the model does one thing: **predict the single most probable next token**, given everything that came before it.

```
"Generate boundary value test ideas for cart quantity update."
→ Next token: "Here"        (probability: high)
→ Next token: " are"        (probability: high)
→ Next token: " some"       (probability: high)
→ Next token: " boundary"   (probability: high)
→ Next token: " value"      (probability: high)
→ ...and so on, one token at a time
```

The model has no understanding of your application, no access to your system, and no concept of whether a test case is actually useful. It produces whatever sequence of tokens is statistically likely to follow the previous ones — based on patterns learned from training data.

**What this means for testers:**  
An output like `"Minimum: 0, Maximum: 999"` is not a domain decision — it's a statistical pattern the model has seen in similar contexts. The actual business rules for your cart (e.g., max 50 items, no negative quantities) must come from *you*, not the model.

---

### 4. Non-Determinism — Why You Get Different Answers Each Time

Models use a setting called **temperature** that adds controlled randomness to token selection. Instead of always picking the single highest-probability token, the model occasionally picks a slightly less likely one — simulating creativity and variety.

**Practical demonstration:**  
Run your cart quantity prompt three times. You may get:
- Run 1: boundary values of `0, 1, 99, 100`
- Run 2: boundary values of `-1, 0, 1, 999, 1000`
- Run 3: boundary values of `0, 1, 50, 51` with an added note about stock limits

None of these runs is "wrong" from the model's perspective. All are statistically reasonable continuations.

**What this means for testers:**  
You cannot treat a single AI response as a complete or stable answer. Re-running the same prompt can reveal gaps or contradictions. AI-generated test suites need human review every time — they are a starting point, not a specification.

---

### 5. Hallucination — Confident, Plausible, and Wrong

**Hallucination** occurs when the model generates output that sounds authoritative and well-formed but is factually incorrect, invented, or unsupported by any real source.

This happens because the model is optimising for *linguistic plausibility*, not *factual accuracy*. It has no way to check whether the test cases it generates match your actual system behaviour. It will invent boundary values, assume business rules, and fabricate edge cases — all with the same confident tone it uses for correct output.

**Example from your prompt:**

> *AI output:* "The cart quantity field should accept values from 1 to 9999. Test boundary values: 0, 1, 9998, 9999, 10000."

This sounds reasonable. But if your system actually caps cart quantity at 50 (for stock reasons), the model has invented the `9999` boundary entirely. It cannot know your system's rules.

**Key insight:** Hallucination is not a bug that will be fixed — it is a structural property of how next-token prediction works. A fluent, well-structured response is not evidence of correctness.

---

## Part 2 — Three Consequences for Testers

### 1. AI-Generated Test Cases Cannot Be Trusted Without Domain Verification

The model infers boundaries from patterns, not from your requirements. Any value, equivalence class, or condition it suggests must be checked against your actual specification. Treating AI output as a test spec — rather than a brainstorming draft — is a quality risk.

### 2. Coverage Gaps Are Invisible in the Output

The model will not say *"I didn't consider the case where the user is logged out"* — it will simply not include that case. Its output looks complete even when it isn't. Testers must apply systematic techniques (boundary value analysis, equivalence partitioning, decision tables) *on top of* the AI output to find what's missing.

### 3. Repetition Across Prompts Creates False Confidence

If multiple team members independently ask the model for test ideas on the same feature, they may get similar-looking lists and assume convergence means correctness. In reality, similar outputs reflect similar training patterns — not validation. Independent AI responses are not independent reviews.

---

## Part 3 — Quality Control Checklist

Apply these two controls **every time** AI-generated test content is used in your process.

---

### ✅ Control 1 — Trace Every Test Condition to a Source

Before a test case enters your test suite, it must be traceable to a verified requirement, acceptance criterion, or confirmed business rule.

| Step | Action |
|------|--------|
| 1 | Take each AI-suggested test condition (e.g., "max quantity = 9999") |
| 2 | Locate the requirement or specification that defines this behaviour |
| 3 | If no source exists → flag for clarification, do not assume |
| 4 | Document the source reference alongside the test case |

> **Rule of thumb:** If you cannot point to where the expected result comes from, the test case is not ready.

---

### ✅ Control 2 — Run a Structured Gap Analysis After Every AI Generation

AI output is a draft, not a complete set. After reviewing what the model produced, apply at least one systematic technique to find what it missed.

| Technique | How to apply it quickly |
|-----------|------------------------|
| **Boundary Value Analysis** | List the actual min/max/limits from your spec. Did the AI cover them? All of them? |
| **Equivalence Partitioning** | Identify valid and invalid partitions. Did the AI test both sides of every boundary? |
| **Checklist review** | Use your team's existing edge-case checklist (authentication state, empty state, concurrent updates, etc.) |

> **Rule of thumb:** If the gap analysis adds zero new test conditions, you probably didn't look hard enough.

---

## Summary

| Concept | One-line takeaway |
|---|---|
| Tokenization | The model reads fragments, not meaning — word choice matters |
| Context window | Long inputs can silently lose content — keep prompts focused |
| Next-token prediction | Every word is a statistical guess, not a domain decision |
| Non-determinism | Outputs vary — one run is never the full picture |
| Hallucination | Confident output is not correct output — always verify |

**The golden rule for testing teams using AI:**  
> *Treat every AI-generated test artifact as an unreviewed first draft from a junior colleague who has never seen your system.*

---

*Prepared for ISTQB GenAI Module 1.1.2 · Learning Objective: Describe core LLM mechanics and connect them to practical implications for test analysis and design.*
