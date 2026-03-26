# ISTQB Foundation Level (CTFL) – Syllabus Summary

> Based on the official CTFL v4.0.1 chapter structure, with concise explanations for non-obvious points.

## 1. Fundamentals of Testing

### 1.1 What is Testing?
- Testing = **verification + validation**
  - **Verification**: are we building the product right?
  - **Validation**: are we building the right product?
- Testing includes:
  - **Static testing**: checking work products without execution
  - **Dynamic testing**: executing the software
- Testing helps find defects and reduce risk
- Testing cannot prove that no defects exist

### 1.1.1 Test Objectives
- Evaluate work products
- Verify requirements
- Validate user needs
- Build confidence
- Detect defects/failures
- Prevent defects early
- Support decisions
- Reduce risk

### 1.1.2 Testing and Debugging
- **Testing**: reveals failures/defects
- **Debugging**: analyzes cause and fixes defect

| Testing | Debugging |
|--------|----------|
| Finds defects | Fixes defects |
| Usually testers | Usually developers |
| Evaluates behavior | Corrects code/design |

**Flow:** Failure → Testing → Defect → Debugging → Fix → Retest

### 1.1.3 Why is Testing Necessary?
- Defects can cause major losses
- Early testing is cheaper
- Quality supports business goals
- Reduced risk improves trust

---

## 1.2 Why is Testing Important?

### 1.2.1 Testing’s Contribution to Success
- Improves quality
- Reduces business risk
- Supports compliance
- Enables decisions
- Builds stakeholder trust

### 1.2.2 Testing and Quality Assurance
- **Testing**: defect detection
- **Quality Assurance (QA)**: defect prevention

| Testing | QA |
|--------|----|
| Product-focused | Process-focused |
| Reactive | Proactive |
| Finds problems | Prevents problems |

### 1.2.3 Errors, Defects, Failures and Root Causes
- **Error**: human mistake
- **Defect**: flaw in a work product
- **Failure**: incorrect behavior during execution
- **Root cause**: underlying reason

**Chain:** Error → Defect → Failure

- Not every defect causes an immediate failure
- Root-cause analysis helps prevent recurrence

---

## 1.3 Testing Principles
1. Testing shows presence of defects
2. Exhaustive testing is impossible
3. Early testing saves time and cost
4. Defects cluster together
5. Tests wear out (**pesticide paradox**)
6. Testing is context dependent
7. Absence of errors is a fallacy

---

## 1.4 Test Activities, Testware and Test Roles

### 1.4.1 Test Activities and Tasks
- **Test Planning** – define scope, approach, effort
- **Test Monitoring & Control** – track status, adjust actions
- **Test Analysis** – identify what to test
- **Test Design** – derive test conditions/cases
- **Test Implementation** – prepare data, scripts, environment
- **Test Execution** – run tests, compare expected vs actual
- **Test Completion** – summarize results, archive testware

### 1.4.2 Test Process in Context
- Testing depends on context factors:
  - **Stakeholders** – needs, expectations
  - **Team** – skills, experience
  - **Business domain** – criticality, regulations
  - **Technical factors** – architecture, technology
  - **Project constraints** – scope, time, budget
  - **Organization** – structure, processes
  - **SDLC** – development model
  - **Tools** – availability, usability

- These factors influence:
  - test strategy
  - test techniques
  - automation level
  - coverage level
  - documentation detail

👉 Important:
- **Entry criteria** – start conditions
- **Exit criteria** – completion conditions


### 1.4.3 Testware
- **Testware** = artifacts created during testing

Examples by activity:

- **Planning** – test plan, schedule, risk register
- **Monitoring & Control** – progress reports
- **Analysis** – test conditions, defect reports (test basis)
- **Design** – test cases, coverage items
- **Implementation** – test scripts, test data, test suites
- **Execution** – test logs, defect reports
- **Completion** – test summary report, lessons learned

👉 Managed via configuration management

### 1.4.4 Traceability between Test Basis and Testware
- **Traceability** = linking:
  - requirements → test cases → results → defects

Purpose:
- coverage evaluation
- impact analysis
- risk assessment
- reporting support
- audit/compliance

Examples:
- requirements ↔ test cases → coverage check
- risks ↔ test results → residual risk evaluation

👉 Enables:
- better reporting
- change impact analysis
- quality assessment

### 1.4.5 Roles in Testing
Two main roles:

- **Test management role**
  - planning
  - monitoring & control
  - reporting
  - coordination

- **Testing role**
  - analysis
  - design
  - implementation
  - execution

Notes:
- Roles depend on context
- One person may perform multiple roles
- In Agile: roles often shared within team
In practice, one person may perform multiple roles.

---

## 1.5 Essential Skills and Good Practices in Testing

### 1.5.1 Generic Skills Required for Testing
- **Testing knowledge** – techniques, methods
- **Attention to detail** – defect detection
- **Communication skills** – reporting, collaboration
- **Analytical thinking** – problem solving
- **Technical knowledge** – tools, systems
- **Domain knowledge** – business understanding

### 1.5.2 Whole Team Approach
- Quality is a **shared responsibility**
- All team members contribute to testing
- Close collaboration improves quality

Benefits:
- better communication
- faster feedback
- improved team efficiency

### 1.5.3 Independence of Testing
- Independence improves objectivity
- Different perspectives → better defect detection

Levels:
- developer tests own work
- peer testing
- independent test team
- external testers

Benefits:
- unbiased testing
- better defect detection

Drawbacks:
- less collaboration
- possible conflicts
- developers may lose quality ownership
---

# 2. Testing Throughout the Software Development Lifecycle

## 2.1 Testing in the Context of an SDLC

### 2.1.1 Impact of SDLC on Testing
- SDLC impacts:
  - scope of testing – what is tested
  - timing – when testing occurs
  - level of detail – documentation depth
  - techniques – test design methods
  - automation – extent of automation
  - roles – responsibilities of testers

- Sequential models:
  - testing occurs in later phases
  - clear separation of levels

- Iterative/Agile models:
  - testing is continuous
  - frequent regression testing
  - fast feedback required

---

### 2.1.2 SDLC and Good Testing Practices
- Each development activity has a corresponding test activity
- Different test levels have different objectives
- Test analysis/design starts early
- Early reviews improve quality

### 2.1.3 Testing as a Driver for Development
- **TDD (Test-Driven Development)**  
  Tests are written before the code.  
  Development is guided by test cases.

- **ATDD (Acceptance Test-Driven Development)**  
  Tests are derived from acceptance criteria before implementation.  
  Focus: business expectations and acceptance conditions.

- **BDD (Behavior-Driven Development)**  
  Behavior is described in a simple business-readable format, often  
  **Given / When / Then**.  
  Focus: shared understanding of expected system behavior.

→ In all three approaches, tests are defined before implementation.

### 2.1.4 DevOps and Testing
- DevOps promotes fast delivery and continuous feedback
- Testing supports:
  - CI/CD pipelines
  - automated checks
  - rapid defect detection
  - quality gates

### 2.1.5 Shift Left
- **Shift left** = perform testing earlier in the lifecycle
- Examples:
  - review requirements early
  - write tests early
  - automate checks sooner
- Goal: find defects before they become expensive

### 2.2 Test Levels & Test types

### 2.2.1 Test Levels

- **Component (Unit) testing** – individual components
- **Component Integration testing** – interactions/interfaces
- **System integration testing** – complete integrated system
 **System integration testing** –testing interfaces of the system
- **Acceptance testing** – fitness for user/business use

### 2.2.2 Test Types
- **Functional testing** – what the system does
- **Non-functional testing** – how well it works
  - performance efficiency
  - compatibility
  - security
  - usability
  - reliability
  - maintainability
  - portability (flexibility)
  - safety
- **Black-box testing** – based on behavior/specification
- **White-box testing** – based on internal structure
- **Confirmation testing** – check that a fix works
- **Regression testing** – check for unintended side effects

### 2.3 Maintenance Testing
- Triggered by:
  - enhancements
  - bug fixes
  - environment changes
  - migration
- Regression testing is especially important here

---
# 3. Static Testing

## 3.1 Static Testing Basics

### 3.1.1 Work Products Examinable by Static Testing
Static testing can be applied to various work products, including:

- Requirements – specifications, user stories
- Design – architecture, models, diagrams
- Code – source code, scripts
- Testware – test cases, test plans
- Documentation – manuals, procedures

→ Goal: detect defects early without executing the software

---

### 3.1.2 Value of Static Testing
- Detects defects early – before execution
- Reduces cost – cheaper than fixing later defects
- Improves quality – better requirements and design
- Prevents defects – avoids error propagation
- Increases understanding – shared knowledge among team
- Finds defects not easily found by dynamic testing

→ Static testing contributes to higher product quality and lower risk

---

### 3.1.3 Differences between Static and Dynamic Testing

- **Static Testing**
  - no code execution
  - focuses on work products (requirements, design, code)
  - finds defects directly
  - examples: reviews, static analysis

- **Dynamic Testing**
  - requires execution of software
  - focuses on behavior of system
  - detects failures caused by defects
  - compares expected vs actual results

Key difference:
- static → defect detection
- dynamic → failure detection

→ Both are complementary and should be used together

## 3.2 Feedback and Review Process

### 3.2.1 Benefits of Early and Frequent Stakeholder Feedback
- Improves understanding – shared knowledge
- Detects defects early – lower cost
- Reduces rework – fewer late fixes
- Builds consensus – aligned expectations
- Supports risk reduction – early visibility

→ Early feedback improves product quality and efficiency

---

### 3.2.2 Review Process Activities
- **Planning**  
  Define scope, objectives, roles, and schedule

- **Review initiation (Kick-off)**  
  Distribute materials, explain goals and process

- **Individual review**  
  Reviewers examine work product and find defects

- **Communication and analysis**  
  Discuss findings, clarify issues, agree on defects

- **Fixing and reporting (Rework & Follow-up)**  
  Fix defects and verify corrections

---

### 3.2.3 Roles and Responsibilities in Reviews
-  **Manager**  
  Decides what to be reviewed, provides resources (staff & timn)

- **Author**  
  Creates the work product, fixes defects

- **Reviewer**  
  Examines product, identifies defects

- **Moderator (Facilitator)**  
  Leads review, ensures process is followed

- **Scribe (Recorder)**  
  Documents findings and decisions

- **Review leader**  
  Takes responsibility who will be involved, organizing when & where 
---

### 3.2.4 Review Types
- **Informal review**  
  No defined process, flexible

- **Walkthrough**  
  Author-led explanation, feedback session

- **Technical review**  
  Expert evaluation, technical focus

- **Inspection**  
  Formal process, defined roles, entry/exit criteria

---

### 3.2.5 Success Factors for Reviews
- Clear objectives – defined purpose
- Appropriate review type – context fit
- Trained participants – skilled reviewers
- Management support – time/resources
- Adequate time – thorough review
- Good communication – effective collaboration
---

# 4. Test Analysis and Design

## 4.1 Test Techniques Overview
Test techniques are used to design test cases systematically.

- **Black-box techniques (specification-based)**  
  Based on requirements and expected behavior (no code knowledge)

- **White-box techniques (structure-based)**  
  Based on internal structure of the system (code, logic)

- **Experience-based techniques**  
  Based on tester experience, intuition, and knowledge

→ Combining techniques improves test effectiveness

---

## 4.2 Black-Box Test Techniques

### 4.2.1 Equivalence Partitioning (EP)
- Inputs are divided into **equivalence partitions (classes)**
- Each partition is expected to behave similarly
- Tests use **one representative value per partition**

Types:
- valid partitions
- invalid partitions

→ Reduces number of test cases while maintaining coverage

---

### 4.2.2 Boundary Value Analysis (BVA)
- Focuses on **boundary values of partitions**
- Defects often occur at edges

Typical values:
- minimum
- just above minimum
- nominal value
- just below maximum
- maximum

→ Often used together with equivalence partitioning

---

### 4.2.3 Decision Table Testing
- Used for systems with **complex business rules**
- Tables represent:
  - conditions (inputs)
  - actions (outputs)

→ Ensures all combinations are tested

---

### 4.2.4 State Transition Testing
- Used when system behavior depends on **state and events**
- Models:
  - states
  - transitions
  - events

Tests include:
- valid transitions
- invalid transitions

→ Useful for workflows, UI states, protocols

---

## 4.3 White-Box Test Techniques

### 4.3.1 Statement Testing and Statement Coverage
- Ensures each executable statement is tested

Coverage formula:
statement coverage = executed statements / total statements

→ Basic structural coverage

---

### 4.3.2 Branch Testing and Branch Coverage
- Ensures each branch (decision outcome) is tested

Coverage formula:
branch coverage = executed branches / total branches

→ Stronger than statement coverage

---

### 4.3.3 Value of White-Box Testing
- Identifies:
  - unreachable code
  - missing logic paths
  - structural weaknesses

→ Complements black-box testing

---

## 4.4 Experience-Based Test Techniques

### 4.4.1 Error Guessing
- Based on:
  - past defects
  - common mistakes
  - domain knowledge

→ Often informal but effective

---

### 4.4.2 Exploratory Testing
- Simultaneous:
  - learning
  - test design
  - test execution

→ Flexible, adaptive approach

---

### 4.4.3 Checklist-Based Testing
- Uses predefined checklists
- Ensures:
  - consistency
  - completeness

→ Useful for regression and reviews

---

## 4.5 Collaboration-Based Test Approaches

### 4.5.1 Collaborative User Story Writing
- Requirements created collaboratively
- Improves shared understanding

---

### 4.5.2 Acceptance Criteria
- Define conditions that must be met
- Used as basis for acceptance tests

---

### 4.5.3 Acceptance Test-Driven Development (ATDD)
- Tests created before implementation
- Based on acceptance criteria
- Involves stakeholders

→ Focus: business requirements

---

### 4.5.4 Behavior-Driven Development (BDD)
- Behavior described in natural language
- Format: **Given / When / Then**

→ Improves communication between technical and business teams

---

# 5. Managing the Test Activities

## 5.1 Test Planning
- Define **scope** – what will be tested
- Define **objectives** – goals of testing
- Select **test approach** – strategy, techniques
- Estimate **effort** – time and resources
- Assign **resources** – people, tools
- Plan **schedule** – timeline and milestones
- Define **entry criteria** – conditions to start testing
- Define **exit criteria** – conditions to stop testing

→ Test planning ensures structured and controlled testing

---

### 5.1.2 Test Estimation
- Estimate:
  - effort (time needed)
  - resources (people/tools)
  - cost

Factors:
- complexity
- risks
- team experience
- historical data

---

### 5.1.3 Test Case Prioritization
- Prioritize tests based on:
  - risk level
  - business importance
  - critical functionality

→ High-risk areas tested first

---

## 5.2 Risk Management

- **Product risks**
  - quality issues
  - failures in production

- **Project risks**
  - schedule delays
  - resource shortages
  - budget problems

**Risk-based testing:**
- identify risks
- assess likelihood & impact
- prioritize testing accordingly

→ Focus testing where failure impact is highest

---

## 5.3 Test Monitoring, Control and Completion

### Test Monitoring
- Track:
  - progress vs plan
  - test execution status
  - defect status

### Test Control
- Compare actual vs planned
- Take corrective actions:
  - re-plan testing
  - adjust priorities
  - allocate resources

### Test Completion
- Evaluate exit criteria
- Create test summary report
- Archive testware
- Identify lessons learned

---

### Metrics (Examples)
- test progress (% completed)
- defect density
- defect detection rate
- coverage (requirements, code)

→ Metrics support decision-making

---

## 5.4 Configuration Management
- Maintain **version control** for testware
- Ensure **consistency** of test environments
- Support **traceability** (requirements ↔ tests)
- Enable **reproducibility** of test results

→ Ensures controlled and reliable testing

---

## 5.5 Defect Management

Typical lifecycle:
- New
- Assigned
- Fixed
- Retested
- Closed

A good defect report:
- clear description
- reproducible steps
- expected vs actual results
- severity/priority
- supporting evidence (logs, screenshots)

→ Enables efficient defect resolution

---

# 6. Test Tools

## 6.1 Tool Support for Testing

- **Test management tools**  
  planning, tracking, reporting

- **Static testing tools**  
  reviews, static analysis

- **Test design tools**  
  generate/manage test cases

- **Test data preparation tools**  
  create and manage test data

- **Test execution tools**  
  automate test execution

- **Coverage tools**  
  measure structural coverage

- **Defect management tools**  
  log and track defects

- **CI/CD tools**  
  support continuous testing and feedback

---

## 6.2 Benefits and Risks of Test Automation

### Benefits
- faster execution
- repeatability
- consistency
- supports regression testing
- reduces manual effort

### Risks
- high initial cost
- maintenance effort
- unrealistic expectations
- tool misuse
- dependency on tools

---

## 6.3 Introducing and Using Tools in an Organization

- Define objectives and requirements
- Select appropriate tools
- Run pilot project (proof of concept)
- Train users
- Define usage standards and processes
- Integrate tools into workflow
- Maintain and improve tool usage

→ Successful tool adoption requires planning and training

---

## Quick Cheat Sheet

| Area | Key Focus |
|------|----------|
| Principles | 7 testing principles |
| Levels | Unit → Integration → System → Acceptance |
| Review type | Inspection = most formal |
| Black-box | EP, BVA, Decision Tables, State Transitions |
| White-box | Statement vs Branch Coverage |
| Risk | Product risk vs Project risk |
| Regression | Check side effects after change |
| Confirmation | Check the fix itself |

---

## End of Summary
