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
| Symbolic AI   |  | |  |
| Classical ML  |  | |  |
| Deep Learning |  | |  |
| Generative AI |  | |  |

### Final Decision



| Paradigm      | Toolshop testing use case                                | Main strength                                      | Caution/risk                    |
|---------------|----------------------------------------------------------|----------------------------------------------------|---------------------------------|
| Symbolic AI   | Validating search results based on rules (e.g., “drill” returns relevant products, filters work correctly) |Clear rule-based and consistent output |Cannot understand complex search (intent) |
| Classical ML  | Analyzing search history and click data to identify popular products and trends|Learns patterns from data and supports prioritization |May produce incorrect insights with insufficient data |
| Deep Learning | Understanding semantic meaning of search queries to improve relevance of results|Can capture semantic relationships between query and products |Hard to explain how results are generated |
| Generative AI | Generating search test cases (e.g., typos, empty search, long queries), test data, and edge cases|Fast and broad test case generation |May produce irrelevant or incorrect test cases without review |

I would use Generative AI first for requirement-to-test-case generation because it can quickly produce initial test cases from requirements, helping to speed up test design and provide a strong starting point for refinement.

Search process:
Symbolic AI → “Do the search results match the rules?” - rule checking

ML → “This search query is often high-risk or frequently fails” - data analysis and prediction

Deep Learning → “The user intent behind this query is unclear or unusual” - complex pattern detection

GenAI → “Let me generate test cases for this search flow” - content generation (e.g., writing tests)

---

### 6. Expected Outcome / Deliverable
One markdown note containing the completed 4-row table and one final decision sentence.

---

### 7. Timebox
30 minutes

---

### 8. Hints (Optional)
- Stay concrete: write examples for your chosen Toolshop flow only.
- If unsure, start with Generative AI for draft test ideas and note one review risk.
