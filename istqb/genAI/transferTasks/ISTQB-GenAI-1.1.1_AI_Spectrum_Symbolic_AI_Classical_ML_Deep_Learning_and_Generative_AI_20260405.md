### 1. Title
Quick AI Spectrum Mapping for Toolshop Testing

---

### 2. Syllabus Reference
ISTQB GenAI – 1.1.1 AI Spectrum: Symbolic AI, Classical Machine Learning, Deep Learning and Generative AI

---

### 3. Learning Objective
Identify, in a simple and practical way, when each AI paradigm is useful for testing in Toolshop. Produce a lightweight comparison your Scrum team can use immediately.

---

### 4. Context / Scenario
During sprint planning, your team wants to use AI support in Toolshop testing but is unsure which paradigm fits which task. You create a quick decision aid focused on one real Toolshop feature.

### Selected Use Case

E1.1 – CO2 Impact Rating Display (Toolshop)

---

### 5. Task Instructions
1. Open https://practicesoftwaretesting.com/ and choose one feature flow (e.g., search, login, or checkout).
2. Create a table with exactly four rows: Symbolic AI, Classical ML, Deep Learning, Generative AI.
3. For each row, fill only these three fields:
   - One testing use case for your chosen Toolshop flow
   - One main strength
   - One caution/risk
4. Add one sentence: Which paradigm would you use first for requirement-to-test-case generation in this sprint, and why?
5. Use this starter template to keep it fast:


| Paradigm      | Toolshop testing use case                                | Main strength                                      | Caution/risk                    |
|---------------|----------------------------------------------------------|----------------------------------------------------|---------------------------------|
| Symbolic AI   | Validate CO2 rating per product using predefined rules   | Clear rules enable simple, deterministic validation| Only detects predefined errors  |
| Classical ML  | Detect unusual CO2 values                                | Identifies anomalies in data                       | Requires large amounts of data  |
| Deep Learning | Detect the Eco symbol correctly in the UI                | Detects errors in complex patterns                 | Requires careful preparation    |
| Generative AI | Generate edge cases for CO2 rating                       | Quickly generates many alternative test cases      | Results must be reviewed        |

### Final Decision

I would start with Symbolic AI for this use case, as the CO2 rating can be validated using predefined rules.  

---

### 6. Expected Outcome / Deliverable
One markdown note containing the completed 4-row table and one final decision sentence.

---

### 7. Timebox
15 minutes

---

### 8. Hints (Optional)
- Stay concrete: write examples for your chosen Toolshop flow only.
- If unsure, start with Generative AI for draft test ideas and note one review risk.
