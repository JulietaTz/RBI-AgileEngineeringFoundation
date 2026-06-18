# Product Test Plan for ToolShop WEB

**Test Plan Level:** Product Level  
**Tribe:** E-Commerce  
**Product:** ToolShop WEB  
**Application Under Test:** Practice Software Testing Web Application   


**Document Control**

| Version | State | Date | Description | Author | Reviewed |
|----------|-----|------------|-------------|-------------|------------|
| v1 | 2026-06-18 | Released | Document structure updated and prepared for future enhancements. | Gülbin Deniz | R. Grötz |




## 1. Introduction

This Product Test Plan describes the test strategy, test scope, and planned test activities for the ToolShop WEB product.  

The test plan is derived from the overarching Test Policy and defines how testing activities are implemented at the product level.  

The objective of this test plan is to ensure that the  application is tested in a systematic, structured, and traceable manner.  

The main focus is on business-critical and supporting e-commerce use cases as defined in the test scope.


### 1.1 Timeline

| Milestone ID| Milestone | Date | Comment |
|----|-----------|------------|---------|
| M1 | Project Kick-off | 2026-06-19 | Shared understanding of objectives, roles, and scope |
| M2 | Product Test Plan Approved | 2026-06-26 | Overall test direction agreed |
| M3 | Phase 1 Readiness Review | 2026-07-24 | Czech market readiness assessed |
| M4 | Phase 2 Readiness Review | 2026-09-04 | AI readiness assessed |
| M5 | Phase 3 Readiness Review | 2026-10-16 | US West Coast readiness assessed |
| M6 | Phase 4 Readiness Review | 2026-11-27 | Japan readiness assessed |
| M7 | Final Project Review | TBD | Overall quality status and recommendation delivered |
| M8 | Investor Presentation | 2027-01-02 | Final project presentation |



## 2. Test Objectives

The main objectives of this test plan are:

- Validation of the functional requirements of the web application  
- Ensuring a stable and error-free user experience  
- Early detection of critical defects  
- Reduction of risks in key business processes (e.g. registration and checkout)  



## 3. Test Scope

### 3.1 In Scope

The test scope is derived from the defined use cases and focuses on business-critical (**MUST**) and supporting (**SHOULD**) functionalities.

The following use cases are covered by this test plan:

### MUST  
*(business-critical core functionalities required for the main e-commerce workflow)*

- Authentication
- Checkout Flow
- Customer Account
- Product Category
- Product Detail
- Product Overview
- API Functionality (REST API, GraphQL API)  

These use cases are considered business-critical as they are essential for completing a purchase.



### SHOULD  
*(supporting and non-critical functionalities)*

- Contact Form
- Rentals  



The classification of business-critical (**MUST**) and supporting (**SHOULD**) use cases is based on the project requirements and scope definition.

A detailed mapping of all relevant features is provided in the **Feature Classification Table** below.



## Feature Classification Table

| Feature | Description | Business Critical (YES/NO) |
|----------|-------------|----------------------------|
| Authentication | Login, Register, Forgot Password | YES |
| Checkout Flow | Increase/Decrease Quantity, Delete Item, Address Details, Payment Options (Basic) | YES |
| Contact Form | Basic | NO |
| Customer Account | Update Profile, Change Password, Invoices Overview, Invoice Detail, Favorites, Contact Messages | YES |
| Product Category | Product categorization and navigation | YES |
| Product Detail | Product details | YES |
| Product Overview | Product listing, Pagination, Search, Filter, Sorting | YES |
| Rentals | Rental functionality | NO |
| REST API | REST API endpoints | YES |
| REST API Documentation (Swagger) | API documentation and testing interface | NO |
| GraphQL API | GraphQL endpoints | YES |
| GraphQL Playground (GraphiQL) | GraphQL query and testing interface | NO |  




### 3.2 Out of Scope

The focus of this test plan is exclusively on functional testing of the web interface.

The following areas are not part of this test plan:

- Performance testing (e.g. load and stress testing)  
- Mobile applications (focus is on web application only)    
- Security testing (e.g. penetration testing)  
- Infrastructure and backend components outside the UI  
- Third-party integrations (if applicable)  



## 4. Test Items

The test scope focuses on the presentation layer (WEBUI) of the system, including APIs and services.



## 5. Test Strategy

### 5.1 Test Levels


| Level | Description | Comment |
|---------|-------------|---------|
| Component Testing | Verification of individual software components in isolation. | Performed by developers |
| System Testing | Validation of the complete integrated system against specified requirements. | Performed by testers |
| System Integration Testing | Validation of interactions between integrated system components through end-to-end user scenarios. | Performed by Test Engineer |
| User Acceptance Testing (UAT) | Validation of predefined acceptance criteria from a user perspective. | Performed by Test Engineer |  




 
### 5.2 Test Approach

| Test Approach | Description | Comment |
|---------------|-------------|---------|
| Risk-based testing | Testing activities are prioritized based on identified business and technical risks. | Focus on business-critical functionality |
| Requirements-based testing | Tests are derived from requirements, user stories and acceptance criteria. | Ensures traceability between requirements and tests |
| Use case-based testing | The focus is on validating user interactions and business-critical end-to-end processes of the web application. | Focus on end-to-end business workflows |
| User Acceptance Testing (UAT) | Validation of predefined acceptance criteria from a user perspective. | Executed by Test Engineer |
| CRUD & POISED for REST APIs | For REST API testing, CRUD and POISED are used to support the systematic validation of API functionality, data handling, error handling, and interface behavior. | Detailed API testing is covered in the Test Automation Strategy |
| Regression testing | Regression testing is performed after changes or bug fixes to ensure that existing functionality remains stable. | Executed after bug fixes and releases |



### 5.3 Test Design Approach (T1–T5)

| Category | Description | Minimum Coverage |
|-----------|-------------|------------------|
| T1 | Happy Path (standard case) | MUST and SHOULD |
| T2 | Alternative Scenarios | When alternative flows exist |
| T3 | Exception Cases | MUST |
| T4 | Negative Testing | MUST |
| T5 | Misuse Scenarios | When robustness or misuse risks exist |



### 5.4 Test Design Techniques

The following test design techniques are applied:

- Equivalence Partitioning  
- Boundary Value Analysis  
- Decision Table Testing  
- State Transition Testing  
- Scenario-based testing  
- Exploratory testing  
- Use-Case based (T1T5)



### 5.5 Traceability

Test cases are linked to corresponding user stories and acceptance criteria to ensure traceability.



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



## 6. Test Environment

Tests are performed in the following environment:

- Application: Practice Software Testing (Web)  
- Browser: Modern web browsers (e.g. Firefox)  
- Operating System: Windows-based systems  
- Internet connection required  

**URL:** https://practicesoftwaretesting.com  



## 7. Test Data

Typical test data is used to cover different application scenarios:

- User data (valid and invalid)  
- Password combinations with varying complexity  
- Product-related data  
- Shopping cart and checkout data  

Test data should be designed in a way that tests remain repeatable despite periodic database resets.



## 8 Test Automation

Detailed test automation activities are described in the separate Test Automation Strategy document.

REST API, REST API Documentation (Swagger), GraphQL API and GraphQL Playground (GraphiQL) are part of the product scope. Detailed API testing is covered separately in the Test Automation Strategy.

Reference:
- [Test Automation Strategy](https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/TTT426-the-testproject/testwareTTT426/testAutomationStrategy.md)


## 9. Roles and Responsibilities

| Role | Who | Responsibility |
|------|------|------|
| Test Project Lead | Gülbin Deniz | Coordination of test planning activities, project communication and maintenance of the Product Test Plan |
| QA Lead / Test Policy Owner | Julieta Tzouridis | Definition, maintenance and review of the Test Policy |
| Test Automation Lead | Dilek Firat | Definition and maintenance of the Test Automation Strategy and automation-related activities |
| Test Designer / Analyst | Ângela Simões Krainer | Test analysis, test design and support of test execution activities |
| Test Designer / Analyst | Serife Cicek | Test analysis, test design and support of test execution activities |
| Test Designer / Analyst | Jane Doe | Test analysis, test design and support of test execution activities |
| QE Specialist / Mentor / Reviewer | Rudolf Grötz | Mentoring, review activities and quality assurance guidance |



## 10. Entry and Exit Criteria

### 10.1 Entry Criteria

The test process starts when:

- Relevant requirements and user stories are defined and available  
- Test cases based on the T1–T5 test design approach are created  
- The test environment is ready and accessible  
- Required test data is available  



### 10.2 Exit Criteria

The test process is considered complete when:

- All planned test cases have been executed  
- No critical or blocking defects remain open  
- Test results are fully documented  
- A test summary report has been created  



## 11. Risks and Assumptions

### Risks

| Risk ID | Risk | Impact | Probability | Mitigation |
|--------|------|--------|------------|-----------|
| R1 | Errors in user registration may prevent access | High | Medium | Validation & boundary testing |
| R2 | Checkout defects may lead to revenue loss | High | Medium | E2E & regression testing |
| R3 | Invalid validation may cause security issues | High | Medium | Negative testing |
| R4 | Pricing/tax errors may cause inconsistencies | High | Medium | Boundary testing |



### Assumptions

- The test environment is stable and available  
- Requirements and user stories are correctly defined  
- External dependencies (e.g. services or APIs) function as expected  



## 12. Test Deliverables 

| Deliverable              | Description                                          | Storage Location                           |
| ------------------------ | ---------------------------------------------------- | ------------------------------------------ |
| Product Test Plan        | Product-level test planning document                 | `testwareTTT426/testPlan-product-WEB.md`   |
| Test Policy              | Project-wide testing principles and governance       | `testwareTTT426/testPolicy-ToolShop.md`    |
| Test Automation Strategy | Test automation approach and implementation details  | `testwareTTT426/testAutomationStrategy.md` |
| Test Design (T1–T5)      | Test case design based on the T1–T5 model            | `testwareTTT426/testDesign-T1T5.md`        |
| Test Design (POISED)     | API-oriented test design using POISED                | `testwareTTT426/testDesign-POISED.md`      |
| Defect Reports           | Defect tracking and issue management                 | GitHub Issues                              |
| Test Summary Report      | Summary of test execution results and quality status | `testwareTTT426/` (to be created)          |
