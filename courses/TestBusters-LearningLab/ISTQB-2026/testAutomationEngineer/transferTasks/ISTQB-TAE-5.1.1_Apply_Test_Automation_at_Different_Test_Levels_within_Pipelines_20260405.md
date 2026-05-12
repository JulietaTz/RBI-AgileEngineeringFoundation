### 1. Title
Design a Pipeline Test-Level Strategy for Toolshop

---

### 2. Syllabus Reference
ISTQB TAE – 5.1.1 Apply Test Automation at Different Test Levels within Pipelines

---

### 3. Learning Objective
Distribute automated checks across pipeline stages to optimize speed, confidence, and feedback quality.

---

### 4. Context / Scenario
Current pipeline runs end-to-end checks too early, causing slow and noisy feedback.

---

### 5. Task Instructions
1. Define four pipeline stages: Commit, Build, Integration, Pre-release.
2. Assign at least 2 test types to each stage (unit/API/component/UI smoke/regression).
3. Map Toolshop examples to each stage (e.g., cart API validation in integration, checkout smoke in pre-release).
4. Add maximum target duration per stage.
5. Define pass/fail gate rules and one quarantine rule for flaky tests.

---

### 6. Expected Outcome / Deliverable
A stage-based pipeline test strategy with level mapping, timing targets, and gate criteria.

---

### 7. Timebox
30 minutes

---

### 8. Hints (Optional)
Place fastest and most deterministic checks as early as possible.
