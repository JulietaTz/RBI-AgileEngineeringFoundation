# Product Test Plan for ToolShop WEB

**Test Plan Level:** Product Level  
**Tribe:** E-Commerce  
**Product:** ToolShop WEB  
**Application Under Test:** Practice Software Testing Web Application  
**Standard:** ISO/IEC/IEEE 29119  

**Tester:** Gülbin Deniz  
**QA Lead / Test Policy Owner:** Julieta Tzouridis  
**Reviewer / Mentor:** Rudolf Grötz  

**Date:** March 2026  

---

## 1. Introduction

This Product Test Plan describes the test strategy, test scope, and planned test activities for the ToolShop WEB product.  

The test plan is derived from the overarching Test Policy and defines how testing activities are implemented at the product level.  

The objective of this test plan is to ensure that key use cases of the web application are tested in a systematic, structured, and traceable manner.  

The focus is on business-critical and supporting e-commerce use cases as defined in the test scope.

---

## 2. Test Objectives

The main objectives of this test plan are:

- Verification of the functional requirements of the web application  
- Ensuring a stable and error-free user experience  
- Early detection of critical defects  
- Reduction of risks in key business processes (e.g. registration and checkout)  

---

## 3. Test Scope

### 3.1 In Scope

The following use cases are covered by this test plan:

#### MUST (business-critical core functionalities required for the main e-commerce workflow):

- User registration  
- User login  
- Shopping cart management  
- Checkout process  
- Currency and tax handling  
- Sustainability-related use cases in the web frontend  

These use cases are considered business-critical as they are essential for completing a purchase.

#### SHOULD (supporting and non-critical functionalities)

- Product search  
- Product catalog (display of products)  
- Product availability (e.g. stock status)  
- Additional use cases according to the product backlog  

---

### 3.2 Out of Scope

The focus of this test plan is exclusively on functional testing of the web interface.

The following areas are not part of this test plan:

- Performance testing (e.g. load and stress testing)  
- Mobile applications (focus is on web application only)  
- API testing (separate test level, not part of this plan)  
- Security testing (e.g. penetration testing)  
- Infrastructure and backend components outside the UI  
- Third-party integrations (if applicable)  

---

## 4. Test Items

The test scope focuses on the presentation layer of the system.

Covered components include:

- Angular-based web frontend  
- UI elements and user interactions  
- Client-side validation and input handling  
- UI-driven business logic (e.g. cart and pricing behavior)  

Interactions with backend services (REST API) are tested indirectly through UI-based scenarios but are not the primary focus of this test plan.

---

## 5. Test Strategy

### 5.1 Test Levels

Testing is performed on different levels:

- System Testing (main focus of this test plan)  
- System Integration Testing (covered indirectly via UI interactions)  
- User Acceptance Testing (UAT)  

System testing implicitly covers interactions between components from a user perspective.  

System integration aspects are therefore tested indirectly via end-to-end user interactions through the UI.  

Dedicated integration tests at API or service level are out of scope for this test plan.  

User Acceptance Testing (UAT) is included as a test level, where predefined acceptance criteria are validated, even if executed by testers on behalf of business stakeholders.

---

### 5.2 Test Approach

The testing approach is based on:

- Risk-based testing  
- Requirements-based testing  
- Use case-based testing  

The focus is on validating user interactions and business-critical end-to-end processes of the web application.  

Additionally, regression testing is performed after changes or bug fixes to ensure that existing functionality remains stable.

---

### 5.3 Test Design Approach (T1–T5)

Test cases are designed using the T1–T5 test design model:

- T1: Happy path (standard case)  
- T2: Alternative scenarios  
- T3: Exception cases  
- T4: Negative testing  
- T5: Misuse scenarios  

Each user story is covered by a structured set of test cases based on this model.

For business-critical use cases (MUST, as defined in Section 3.1 Test Scope), at least T1, T3, and T4 test cases are defined.  

For supporting use cases (SHOULD), at least one T1 test case is defined.

Misuse cases (T5) are considered to validate system robustness and to simulate unintended or erroneous user behavior (e.g. invalid inputs or misuse of UI flows).

---

### 5.4 Test Techniques

The following test techniques are applied:

- Equivalence Partitioning  
- Boundary Value Analysis  
- Decision Table Testing  
- State Transition Testing  
- Scenario-based testing  
- Exploratory testing  

---

### 5.5 Traceability

Test cases are linked to corresponding user stories and acceptance criteria to ensure traceability.

---

### 5.6 Test Reporting and Defect Management

Test progress and results are continuously monitored and tracked using appropriate tools (e.g. GitHub Issues, Pull Requests, and Trello).

Defects are systematically documented, tracked, and prioritized based on their severity and business impact.

This ensures transparency of the test process and supports effective decision-making throughout the testing lifecycle.

Defects include:

- Clear description  
- Steps to reproduce  
- Expected results  
- Actual results  
- Severity level  

---

## 6. Test Environment

Tests are performed in the following environment:

- Application: Practice Software Testing (Web)  
- Browser: Modern web browsers (e.g. Firefox)  
- Operating System: Windows-based systems  
- Internet connection required  

**URL:** https://practicesoftwaretesting.com  

---

## 7. Test Data

Typical test data is used to cover different application scenarios:

- User data (valid and invalid)  
- Password combinations with varying complexity  
- Product-related data  
- Shopping cart and checkout data  

Test data should be designed in a way that tests remain repeatable despite periodic database resets.

---

## 8. Roles and Responsibilities

### Tester (Product Level – Gülbin Deniz)

- Creation of the test plan  
- Creation of test cases (T1–T5)  
- Execution of tests  
- Documentation of test results  

### QA Lead / Test Policy Owner (Tribe Level – Julieta Tzouridis)

- Definition and maintenance of the Test Policy  
- Ensuring adherence to testing standards  
- Support in strategic testing decisions  

### Reviewer / Mentor (Rudolf Grötz)

- Review of the test plan  
- Feedback and quality assurance  

---

## 9. Entry and Exit Criteria

### 9.1 Entry Criteria

The test process starts when:

- Relevant requirements and user stories are defined and available  
- Test cases based on the T1–T5 test design approach are created  
- The test environment is ready and accessible  
- Required test data is available  

---

### 9.2 Exit Criteria

The test process is considered complete when:

- All planned test cases have been executed  
- No critical or blocking defects remain open  
- Test results are fully documented  
- A test summary report has been created  

---

## 10. Risks and Assumptions

### Risks

| Risk ID | Risk | Impact | Probability | Mitigation |
|--------|------|--------|------------|-----------|
| R1 | Errors in user registration may prevent access | High | Medium | Validation & boundary testing |
| R2 | Checkout defects may lead to revenue loss | High | Medium | E2E & regression testing |
| R3 | Invalid validation may cause security issues | High | Medium | Negative testing |
| R4 | Pricing/tax errors may cause inconsistencies | High | Medium | Boundary testing |

---

### Assumptions

- The test environment is stable and available  
- Requirements and user stories are correctly defined  
- External dependencies (e.g. services or APIs) function as expected  

---

## 11. Test Deliverables

The following artifacts are created:

- Product Test Plan  
- Test cases (T1–T5)  
- Test execution reports  
- Defect reports (e.g. via GitHub Issues)  
- Test summary report  

These artifacts support transparency, traceability, and quality assurance throughout the testing process.