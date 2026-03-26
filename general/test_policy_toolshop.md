# Test Policy – ToolShop (Based on ISTQB FL & ISO 29119)

## 1. Purpose

This Test Policy defines the principles, roles, processes, and artifacts for software testing in the ToolShop project.

The goal is to:
- ensure product quality  
- detect defects early  
- reduce risks  
- support reliable releases  

Testing is integrated into the Software Development Life Cycle (SDLC) and follows ISTQB best practices.

---

## 2. Roles and Responsibilities

### QA Engineer
- Design and execute test cases  
- Report defects  
- Ensure test coverage  

### Developer
- Perform unit testing  
- Fix defects  
- Support test execution  

### Product Owner
- Define requirements and acceptance criteria  
- Prioritize defects  
- Validate business needs  

### Test Manager (if defined)
- Define test strategy  
- Coordinate testing activities  
- Monitor test progress  

### Team (Agile Principle)
- Shared responsibility for quality  
- Collaborate on testing activities  

---

## 3. Test Process (ISTQB / ISO 29119)

### 3.1 Test Planning
Defines:
- scope  
- risks  
- test approach  

Output:
- Test Plan  

---

### 3.2 Test Analysis
Identifies:
- what needs to be tested  

Defines:
- test conditions (based on requirements and risks)

---

### 3.3 Test Design
Defines:
- how to test  

Activities:
- create test cases  
- apply test techniques  

Techniques:
- Equivalence Partitioning (EP)  
- Boundary Value Analysis (BVA)  

---

### 3.4 Test Implementation
Prepares:
- test data  
- test environment  

---

### 3.5 Test Execution
Activities:
- run test cases  
- compare actual vs expected results  
- assign pass/fail  

Also:
- log results  
- identify defects  
- analyze deviations  

---

### 3.6 Test Evaluation
Checks:
- if acceptance criteria are met  

If not:
- corrective actions required  

---

### 3.7 Test Closure
Final activities:
- summarize results  
- document lessons learned  

---

## 4. Mapping: Test Activities → Artifacts

| Test Activity        | Artifact                          | Description |
|---------------------|----------------------------------|------------|
| Test Planning       | Test Plan                        | Defines scope, strategy, risks |
| Test Analysis       | Test Conditions                  | Defines what to test |
| Test Design         | Test Specification               | High-level test structure |
| Test Design         | Test Cases                       | Detailed test steps |
| Test Implementation | Test Data                        | Input data for testing |
| Test Execution      | Test Report                      | Results of testing |
| Test Execution      | Defect Report                    | Documented defects |
| Test Evaluation     | Test Summary                     | Quality assessment |
| Test Closure        | Final Test Report                | Overall results |

---

## 5. Test Artifacts

### Test Plan
Defines:
- scope  
- objectives  
- strategy  
- risks  

---

### Test Specification
Describes:
- test scenarios  
- test conditions  
- coverage  

---

### Test Case
Includes:
- steps  
- inputs  
- expected results  

---

### Defect Report
Includes:
- description  
- reproduction steps  
- severity  

---

### Test Report
Summarizes:
- executed tests  
- pass/fail results  
- defects  

---

### Test Data
Defines:
- input values  
- valid/invalid data  
- edge cases  

---

## 6. Test Design Techniques

### Equivalence Partitioning (EP)
- divide input into valid/invalid groups  
- test one value per group  

### Boundary Value Analysis (BVA)
- test minimum, maximum, and edge values  

👉 Goal:
- minimize test cases  
- maximize coverage  

---

## 7. Test Levels

### Unit Testing
- performed by developers  
- tests individual components  

### Integration Testing
- tests interaction between components  

### System Testing
- tests complete system  

### Acceptance Testing
- validates business requirements  

---

## 8. Key Concepts (ISTQB)

- Testing shows presence of defects, not absence  
- Exhaustive testing is impossible  
- Early testing reduces cost  
- Defects cluster  
- Testing is context-dependent  

---

## 9. Defect vs Failure

- **Defect** → issue in system/code  
- **Failure** → visible incorrect behavior  

---

## 10. Test Execution Core Rule

👉 Always:
- compare **actual vs expected results**  
- assign **pass/fail**  
- log and analyze deviations  

---

## 11. Compliance and Data

- Test data must be safe and controlled  
- Personal data must comply with **GDPR**  
- System reliability considerations may follow **DORA principles**  

---

## 12. Summary

This Test Policy ensures:
- structured testing process  
- clear roles and responsibilities  
- defined artifacts  
- alignment with ISTQB and ISO 29119  

It supports continuous improvement and high product quality in the ToolShop project.