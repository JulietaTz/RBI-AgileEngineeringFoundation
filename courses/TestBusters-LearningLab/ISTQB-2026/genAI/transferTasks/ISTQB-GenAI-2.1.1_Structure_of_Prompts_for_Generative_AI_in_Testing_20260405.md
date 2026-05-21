### 1. Title
Transform Weak Prompts into High-Quality Test Prompts

---

### 2. Syllabus Reference
ISTQB GenAI – 2.1.1 Structure of Prompts for Generative AI in Testing

---

### 3. Learning Objective
Apply a structured prompt pattern to increase quality, consistency, and actionability of AI-generated test outputs.

---

### 4. Context / Scenario
Your team receives generic and inconsistent AI results because prompts vary by tester. You need a standard prompt structure.

---

### 5. Task Instructions
1. Review "APP: Unstructured Prompts for Test Activities" in this task.
2. Rewrite each using: role, context, task, constraints, output format, acceptance criteria.
3. Run old and new prompts.
4. Compare results on clarity, completeness, and testability.
5. Define one reusable prompt template for the team.

---

### 6. Expected Outcome / Deliverable
Before/after prompt set and one shared template as md-file.

---

### 7. Timebox
50 minutes

---

### 8. Hints (Optional)
- Use explicit output schema (table fields, numbering, priority).
- Keep constraints realistic and verifiable.

# APP: Unstructured Prompts for Test Activities

## Prompt 1: Exploratory Testing
You are testing the registration workflow of a webshop that now
supports social media login (Google, GitHub, LinkedIn).

Explore the feature freely. Try different entry points, switch between
registration methods, interrupt flows, and combine actions in unusual
ways.

Focus on discovering unexpected behavior, inconsistencies, or usability
issues. Document what you observe and what surprises you.

---

## Prompt 2: Test Design
You are responsible for designing test cases for the extended
registration feature with social media login.

Without using a predefined structure, think about what should be tested.
Consider normal usage, variations, edge cases, and failures.

Write down the test scenarios that you believe are most important to
ensure quality.

---

## Prompt 3: Risk-Based Testing
Assume the social media registration feature will go live tomorrow.

You do not have time to test everything.

Based on your understanding, identify the biggest risks related to this
feature. Focus your testing on areas that could cause the highest impact
for users or the business.

Explain what you would test first and why.
