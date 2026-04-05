### 1. Title
Detect and Mitigate GenAI Hallucinations, Reasoning Errors, and Bias in Toolshop Test Preparation

---

### 2. Syllabus Reference
ISTQB GenAI – 3.1.1 Hallucinations, Reasoning Errors and Biases in Generative AI

---

### 3. Learning Objective
You will identify typical GenAI failure patterns (hallucinations, faulty reasoning, and bias) while preparing tests for the Toolshop webshop. You will produce corrected, evidence-based outputs that are traceable to the real system and documentation.

---

### 4. Context / Scenario
In Sprint 5, your Scrum team asks you to use a GenAI assistant to speed up test preparation for Toolshop. Before adopting the generated output, you must verify whether the assistant invents facts, makes wrong inferences, or introduces biased assumptions that could harm test quality.

---

### 5. Task Instructions
1. Open the webshop at https://practicesoftwaretesting.com/ and inspect two real user flows:
   - Product search/filter in the catalog
   - Add-to-cart and quantity update in cart
2. Open documentation at https://github.com/testsmith-io/practice-software-testing/tree/main/docs and review at least:
   - `features.md`
   - `README.md`
3. Prompt your GenAI assistant with: “Create 6 high-priority test ideas for Toolshop search and cart, including assumptions about users and expected behavior.”
4. Analyze the generated answer and mark at least:
   - 2 potential hallucinations (claims not supported by app/docs)
   - 2 reasoning errors (invalid conclusions, missing preconditions, contradictory logic)
   - 1 potential bias (unjustified assumption about user type, language, geography, or behavior)
5. Validate each marked item against the real webshop behavior and/or docs, and label each as:
   - Confirmed issue
   - Not an issue
6. Rewrite the 6 test ideas so they are evidence-based and neutral:
   - Remove unsupported claims
   - Fix flawed reasoning
   - Replace biased assumptions with explicit, testable conditions
7. Add a short “prompt improvement” note with 3 concrete prompt changes that would reduce hallucinations, reasoning errors, and bias in the next GenAI run.

---

### 6. Expected Outcome / Deliverable
A markdown file named `GenAI-3.1.1-hallucination-reasoning-bias-review.md` containing:
- A table with at least 5 flagged issues (type, GenAI statement, evidence, verdict)
- A corrected list of 6 test ideas
- 3 prompt-improvement bullets

---

### 7. Timebox
30 minutes

---

### 8. Hints (Optional)
- Treat every GenAI claim as untrusted until confirmed in the app or docs.
- Look for absolute language (e.g., “always,” “all users,” “never”) as a bias/error signal.
- Prefer prompts that request explicit assumptions and evidence references.
