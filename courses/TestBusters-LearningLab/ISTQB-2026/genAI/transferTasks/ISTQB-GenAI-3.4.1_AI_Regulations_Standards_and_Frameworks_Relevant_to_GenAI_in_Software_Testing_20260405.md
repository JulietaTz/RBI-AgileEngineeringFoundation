### 1. Title
Create a GenAI Compliance Test Checklist for Toolshop AI Recommendations

---

### 2. Syllabus Reference
ISTQB GenAI – 3.4.1 AI Regulations, Standards and Frameworks Relevant to GenAI in Software Testing

---

### 3. Learning Objective
You will translate AI regulations, standards, and frameworks into concrete software testing checks for a real webshop feature. You will produce a practical compliance-oriented test checklist that can be used in Sprint test planning.

---

### 4. Context / Scenario
Your Scrum team is preparing the Toolshop AI Tool Advisor feature for an upcoming release. As tester, you must ensure GenAI-related risks are addressed using recognized guidance (regulations, standards, frameworks) before the sprint review.

---

### 5. Task Instructions
1. Open Toolshop at https://practicesoftwaretesting.com/ and inspect one recommendation-relevant user flow (e.g., product discovery/search/filter and product detail navigation).
2. Open the Toolshop documentation repository at https://github.com/testsmith-io/practice-software-testing/tree/main/docs and review available product/feature documentation (e.g., `README.md`, `features.md`, API-related docs if present).
3. Open the sprint-5 baseline at https://github.com/testsmith-io/practice-software-testing/tree/main/sprint5 to understand current implementation scope.
4. Use these four references as your compliance lens:
   - EU AI Act (regulatory obligations for AI systems)
   - NIST AI Risk Management Framework (Govern, Map, Measure, Manage)
   - ISO/IEC 42001 (AI management system)
   - ISO/IEC 23894 (AI risk management)
5. Create a table with 8 test checks for the Toolshop AI recommendation context:
   - 2 checks linked to regulatory obligations (EU AI Act)
   - 2 checks linked to risk management controls (NIST AI RMF)
   - 2 checks linked to organizational/process controls (ISO/IEC 42001)
   - 2 checks linked to AI risk treatment (ISO/IEC 23894)
6. For each check, define:
   - Applicable framework/regulation
   - Compliance intent (what must be true)
   - Concrete test activity in Toolshop (what you will do)
   - Expected evidence/artifact (log, UI behavior, documentation link, defect)
7. Identify the 3 highest-priority checks for Sprint 5 and justify priority using risk impact and likelihood.

---

### 6. Expected Outcome / Deliverable
A markdown file named `GenAI-3.4.1-toolshop-ai-compliance-checklist.md` containing:
- A table with 8 compliance-oriented test checks mapped to the four references
- A prioritized top-3 list with short risk-based rationale
- At least 2 evidence links to Toolshop docs/pages you used

---

### 7. Timebox
30 minutes

---

### 8. Hints (Optional)
- If a requirement is abstract, convert it into a verifiable question: “What evidence shows this control is implemented?”
- Prefer checks that can reveal both product defects and process gaps.
- Keep each check atomic: one intent, one test activity, one expected evidence type.
