### 1. Title
Map API Infrastructure Dependencies for Toolshop Automation

---

### 2. Syllabus Reference
ISTQB TAE – 5.1.3 Explain Test Automation Dependencies for an API Infrastructure

---

### 3. Learning Objective
Identify and manage technical dependencies that influence API automation reliability and speed.

---

### 4. Context / Scenario
API tests for Toolshop fail intermittently due to environment and data assumptions.

---

### 5. Task Instructions
1. Build a dependency map for API automation: auth, data state, services, network, secrets, and environment endpoints.
2. For each dependency, classify as internal, external, or shared.
3. Mark dependency criticality (High/Medium/Low).
4. Define one control strategy for each High dependency (mocking, contract checks, seeded data, retries, health checks).
5. Add one pre-run readiness check and one post-run cleanup rule.

---

### 6. Expected Outcome / Deliverable
An API dependency map with criticality and control strategies for robust Toolshop API automation.

---

### 7. Timebox
30 minutes

---

### 8. Hints (Optional)
State management and authentication token handling are often top causes of API test instability.
