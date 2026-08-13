# Create the 42-Vienna Test Policy from Fundamentals of Testing

## Reference to ISTQB Syllabus chapter
ISTQB FL – 1 Fundamentals of Testing

## Link to the transfer task file
https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter%201/ISTQB-FL-1_Test_Policy_42Vienna_20260706.md

---

## Outcome

# 42-Vienna Test Policy

| Field | Value |
|---|---|
| Document title | 42 Vienna Test Policy |
| Version | 1.0 |
| Author | Rashin Harisi |
| Reviewer | Rudolf Groetz |
| Created | 2026-07-31 |
| Last Updated | 2026-08-13 |
| Status | Draft |

> **Document status:** This is a proposed internal policy for testing practices in 42 Vienna projects. Official 42 subject documents and evaluation sheets take precedence if any conflict exists.

### Purpose

The purpose of this policy is to establish a consistent approach to testing 42 Vienna projects. It defines the principles, responsibilities, and minimum testing activities that support project development, submission, and peer evaluation.

The policy aims to:

- improve project quality;
- detect defects before submission;
- reduce the risk of failed peer evaluations;
- support clear and constructive communication about defects; and
- encourage repeatable and risk-based testing practices.

### Scope

This policy applies to mandatory and bonus work in 42 Vienna curriculum projects, including individual and team projects such as `push_swap`, `minishell`, `so_long`, `webserv`, and `Inception`.

The exact testing approach shall be adapted to:

- the project subject;
- the project type and technology;
- the official evaluation sheet;
- the risks associated with the implementation; and
- the tools permitted by the subject.

This policy does not replace project-specific requirements. The current official subject PDF and evaluation sheet remain the primary sources of requirements.

### References

- Current official 42 project subject PDFs
- Current official 42 peer-evaluation sheets
- ISTQB Certified Tester Foundation Level syllabus

### Policy Language

The following terms are used in this document:

- **Shall** indicates a mandatory policy requirement.
- **Should** indicates a recommended practice.
- **May** indicates an optional practice.

---

## 1. Introduction - What is Testing?

Testing is a structured set of activities used to evaluate a project, identify defects, reduce risk, and increase confidence that the project satisfies its requirements.

Testing involves more than executing a program. Actual results shall be compared with expected results derived from the official subject, evaluation sheet, and other approved requirements.

Testing may include:

- **Static testing:** evaluating work products without executing the program, such as reviewing source code, the Makefile, project structure, allowed functions, and Norminette compliance.
- **Dynamic testing:** executing the program and comparing its behaviour with expected results.
- **Verification:** checking whether the project complies with specified requirements and technical constraints.
- **Validation:** checking whether the completed project behaves as expected when used in realistic scenarios.

For example, checking whether `so_long` uses the required library is static testing. Running it with valid and invalid maps is dynamic testing.

### 1.1 Test Objectives

Testing shall aim to:

- verify compliance with the official subject PDF;
- verify that all mandatory requirements are implemented;
- verify bonus requirements when bonus work is submitted;
- identify functional and non-functional defects;
- verify correct handling of normal, boundary, invalid, and error conditions;
- detect crashes, hangs, undefined behaviour, memory errors, and resource leaks where relevant;
- verify compliance with coding and technical restrictions;
- provide evidence that the project is ready for peer evaluation; and
- reduce unnecessary rework and repeated evaluations.

Project-specific objectives shall be defined according to project risk. Examples include:

- For `push_swap`: sorting correctness, input validation, operation limits, and forbidden-function compliance.
- For `so_long`: map validation, movement, collectibles, game completion, rendering, and stable termination.
- For `minishell`: parsing, expansion, pipes, redirections, signals, built-ins, exit status, process handling, and memory management.

### 1.2 Testing and Debugging

Testing and debugging are related but separate activities:

- **Testing** reveals failures and provides information about project quality.
- **Debugging** identifies the cause of a failure and corrects the underlying defect.

When a failure is found, the project author shall:

1. reproduce the failure;
2. identify the underlying defect;
3. implement an appropriate correction; and
4. perform relevant regression testing.

After the correction, the peer validator shall perform confirmation testing to verify that the reported failure has been resolved.

## 2. Why Testing is Necessary?

Testing is necessary in 42 projects to identify defects before peer evaluation and to ensure that the project is ready for submission. Early detection of failures reduces the risk of an unsuccessful evaluation, unnecessary rework, and delays in curriculum progression.

### 2.1 Testing and Quality Assurance

Testing evaluates project work products and behaviour. Quality assurance focuses on improving the development process to prevent defects.

Quality-supporting practices should be applied throughout development. Examples include:

- reading and revisiting the subject PDF;
- compiling regularly with the required flags;
- applying Norminette continuously where required;
- using version control consistently;
- reviewing code and interfaces;
- testing components before integration; and
- maintaining repeatable test data and scripts.

Quality is not created only at the end of development. Testing shall be integrated throughout the project lifecycle.

### 2.2 Errors, Defects, Failures, and Root Causes

- An **error** is a human action that produces an incorrect result.
- A **defect** is an imperfection in a work product or source code.
- A **failure** is observable incorrect behaviour when a defect is executed or encountered.
- A **root cause** is the underlying reason why a defect was introduced.

For example, a misunderstanding of the sorting logic is an error. The resulting incorrect algorithm is a defect. A `KO` result from the checker is a failure. The misunderstood algorithm requirement may be the root cause.

## 3. Testing Principles

The following principles shall guide testing:

1. **Testing can show the presence of defects, not their absence.**  
   Passing all known tests does not prove that a project has no defects.

2. **Exhaustive testing is impossible.**  
   Students shall select representative, boundary, error, and high-risk test cases.

3. **Early testing reduces rework.**  
   Requirements, the Makefile, architecture, interfaces, and error handling should be reviewed before development is complete.

4. **Defects often cluster.**  
   More testing should be directed toward complex, frequently changed, or previously defective areas.

5. **Tests require maintenance.**  
   Test cases should be reviewed and extended when the implementation or understanding of the requirements changes.

6. **Testing is context-dependent.**  
   A graphical project, shell, algorithmic project, and infrastructure project require different test approaches.

7. **Correct execution alone is insufficient.**  
   A project that appears to work but violates the subject or evaluation requirements is not evaluation-ready.

## 4. Test Activities and Tasks

Testing activities in 42 projects should follow the project lifecycle. Students first analyse the subject and requirements, then plan and design tests, prepare the test environment and data, execute self-tests, monitor results, correct identified defects, and repeat relevant tests before peer evaluation or re-evaluation.

### 4.1 Test Process in Context

Testing shall be performed iteratively throughout development.

#### 4.1.1 Test Planning

Before or during implementation, the project author or team shall:

- identify the applicable requirements;
- identify important quality risks;
- select appropriate test activities and tools;
- define responsibilities in team projects; and
- determine what evidence will be retained.

#### 4.1.2 Test Monitoring and Control

Testing progress should be monitored throughout the project. The author or team should review:

- which requirements have been tested;
- which tests passed or failed;
- which defects remain open;
- which high-risk areas need more testing; and
- whether the project is ready for peer evaluation.

#### 4.1.3 Test Analysis

The subject PDF and evaluation sheet shall be analysed to identify test conditions, including:

- mandatory and bonus functionality;
- valid inputs;
- invalid inputs;
- boundary values;
- error conditions;
- technical restrictions;
- resource-management expectations; and
- project-specific performance expectations.

#### 4.1.4 Test Design

Test cases shall include, where relevant:

- normal-use scenarios;
- boundary-value cases;
- invalid and malformed inputs;
- empty and missing inputs;
- repeated operations;
- interruption and signal scenarios;
- resource or memory failure scenarios;
- integration scenarios; and
- regression cases for previously discovered defects.

Each important test should define:

- the requirement or risk being tested;
- preconditions;
- test data or commands;
- expected result; and
- actual result.

#### 4.1.5 Test Implementation

The required test environment, test data, scripts, checklists, and tools shall be prepared before execution.

Automated scripts may be used to improve repeatability, provided that they comply with project rules. Automated results should not replace manual analysis of failures or independent peer evaluation.

#### 4.1.6 Test Execution

During execution, the tester shall:

- follow the defined test steps;
- compare actual and expected results;
- record relevant failures;
- preserve useful evidence when necessary; and
- avoid changing the expected result merely to match the implementation.

Project-appropriate tools may include Norminette, Valgrind, sanitizers, checkers, shell scripts, Docker, `curl`, browsers, or protocol clients, subject to project restrictions.

#### 4.1.7 Test Completion

Before requesting peer evaluation, the project author or team shall:

- verify mandatory requirements;
- review unresolved defects;
- rerun critical and previously failed tests;
- confirm that the required files are present;
- perform a clean build using the required commands;
- verify coding and technical restrictions;
- ensure the repository is in the intended submission state; and
- assess whether the project is evaluation-ready.

### 4.2 Testware

Testware includes the work products and supporting assets used for testing.

Depending on the project, testware may include:

- requirement checklists;
- test conditions and test cases;
- command lists;
- input and configuration files;
- valid and invalid maps;
- expected-output files;
- shell scripts or custom testers;
- memory-checking commands;
- defect reports;
- regression-test lists; and
- peer-evaluation preparation notes.

Testware should be stored in a clear location and maintained with the project when permitted.

### 4.3 Traceability

Each mandatory requirement should be traceable to one or more test conditions and test results.

Traceability may be maintained through:

- a checklist based on the subject PDF;
- comments or identifiers in test scripts;
- a simple requirements-to-tests document;
- a defect log linked to the affected requirement; or
- documented peer-evaluation preparation notes.

The format may remain lightweight, but it shall be understandable to the project author or team.

### 4.4 Roles in Testing

#### 4.4.1 Project Author

The project author shall:

- understand the applicable requirements;
- plan and perform self-testing;
- prepare the test environment and test data;
- investigate and correct defects;
- retest corrections during self-testing and perform relevant regression testing; and
- prepare the project for peer evaluation.

#### 4.4.2 Peer Evaluator

The peer evaluator shall:

- remain independent from the project implementation;
- follow the official evaluation sheet;
- verify compliance with the subject;
- execute appropriate representative tests;
- report observed failures objectively; and
- record the evaluation outcome according to the official process.

#### 4.4.3 Evaluatee

During peer evaluation, the evaluatee shall:

- present the submitted project;
- provide the required repository and environment;
- demonstrate relevant functionality;
- answer technical questions honestly; and
- avoid modifying the submitted project except where the official evaluation process explicitly permits it.

## 5. Skills and Good Practices

Effective testing requires technical and communication skills. Students should develop the ability to:

- interpret subject requirements;
- derive test conditions;
- identify boundary and edge cases;
- use project-appropriate testing tools;
- distinguish symptoms from root causes;
- report defects clearly;
- test corrections;
- recognise regression risk; and
- communicate respectfully during peer evaluation.

### 5.1 Whole Team Approach

For collaborative projects, the whole team shares responsibility for quality.

The team shall:

- agree on testing responsibilities;
- review interfaces between components;
- test integrated behaviour;
- communicate defects constructively;
- retest corrections; and
- jointly assess evaluation readiness.

Responsibility for one component does not remove responsibility for the quality of the integrated project.

### 5.2 Independence of Testing

Self-testing is required during development, but it may be influenced by the author's assumptions.

Independent peer evaluation provides an additional perspective and may reveal defects or misunderstandings missed during self-testing. Therefore:

- project authors shall perform self-testing;
- team members should cross-test one another's components where practical; and
- formal peer evaluation shall be conducted according to the official 42 evaluation process.

Independent testing complements self-testing; it does not replace it.

## 6. Peer Validation by 42 Students

### 6.1 Purpose

Peer validation verifies whether the submitted project meets the mandatory requirements of the official subject and the peer-evaluation criteria.

### 6.2 Independence

Peer validation shall be performed by other 42 students who did not author the submitted project. The project author may perform self-testing before submission, but shall not validate their own project.

### 6.3 Inputs

Peer validation shall be based on:

- the official project subject PDF;
- the submitted repository;
- the Makefile and mandatory deliverables;
- the official peer-evaluation sheet; and
- project-specific requirements and restrictions.

### 6.4 Activities

During peer validation, evaluators shall:

- verify mandatory requirements;
- execute functional tests;
- test relevant edge and error cases;
- check forbidden functions or behaviour;
- use project-specific evaluation tools where required; and
- report observed failures.

After defects are fixed, relevant tests should be repeated to confirm the correction.

### 6.5 Outcomes

The evaluation outcome determines whether the project successfully passes the peer evaluation or requires rework and a new evaluation.
Failed requirements and observed failures should be clearly documented for correction.

### 6.6 Constructive Communication

Defects and failures shall be reported against the project requirements and observed behaviour, not against the project author.
Communication during evaluation should remain clear, objective, and constructive.

### 6.7 Subject-Specific Tooling

Evaluators shall use subject-specific tools where required or provided, such as `checker_OS` for `push_swap`, an IRC client for `ft_irc`, or a browser for web projects.

## 7. Additional Testing Guidance

### 7.1 Risk-Based Testing

Testing effort shall be prioritised according to risk.

Higher priority should be given to areas that:

- can cause an immediate evaluation failure;
- can crash or hang the program;
- can cause undefined behaviour or memory corruption;
- handle complex parsing, concurrency, processes, networking, or resource ownership;
- have changed recently;
- have produced defects previously;
- affect many other components; or
- are difficult to recover from during evaluation.

Limited time shall not be used as a reason to omit all testing. When time is limited, critical and high-risk requirements shall be tested first.


### 7.2 Defect Management

Important defects found during self-testing or team testing should be documented sufficiently to support reproduction and correction.

A defect record should contain:

- a concise title;
- the affected project or component;
- environment information when relevant;
- preconditions;
- steps to reproduce;
- expected result;
- actual result;
- severity or impact;
- supporting evidence when useful; and
- current status.

Defects shall be communicated objectively. Reports shall describe the behaviour and its relationship to the requirement, not criticise the person who wrote the code.

A defect may be considered closed only after the correction has been confirmed through retesting. Relevant regression testing shall also be performed.

### 7.3 Peer-Evaluation Readiness

A project is ready to be submitted for peer evaluation when, at minimum:

- the intended submission version is committed and accessible;
- the project builds successfully from a clean state;
- mandatory files and targets are present;
- mandatory requirements have been tested;
- relevant invalid and boundary cases have been tested;
- no known critical defect remains unresolved;
- required coding standards and function restrictions have been checked;
- relevant memory and resource checks have been performed;
- critical regression tests pass; and
- the author or team can explain the implementation and known limitations.

Passing self-tests does not guarantee a successful peer evaluation.

### 7.4 Policy Review and Improvement

This policy should be reviewed periodically and updated when:

- official 42 evaluation practices change;
- recurring project risks are identified;
- useful testing techniques or tools are adopted;
- feedback is received from students or evaluators; or
- the policy contains unclear, outdated, or impractical requirements.

Changes should be versioned and reviewed before publication.

## Appendix A: Project Examples

### A.1 `minishell`

Suggested focus areas include:

- quoting and tokenisation;
- environment-variable expansion;
- pipes and redirections;
- heredoc behaviour;
- built-in commands;
- exit status;
- signals in interactive mode;
- invalid syntax;
- file-descriptor handling;
- child-process handling; and
- memory leaks.

Bash may be used as a behavioural reference where the subject requires Bash-compatible behaviour. Differences explicitly allowed by the subject shall not be reported as defects.

### A.2 `push_swap`

Suggested focus areas include:

- empty input;
- one or more arguments;
- duplicate numbers;
- non-numeric input;
- integer overflow and underflow;
- already sorted input;
- reverse-sorted input;
- random datasets;
- generated operation validity;
- checker result;
- operation-count expectations; and
- memory handling.

### A.3 `so_long`

Suggested focus areas include:

- valid and invalid map shapes;
- required map components;
- enclosure by walls;
- inaccessible collectibles or exits;
- player movement;
- move counting;
- texture and window handling;
- keyboard and window-close events;
- successful completion; and
- clean termination without leaks.

### Example Test Prompts

- **push_swap:** Does invalid input print `Error` on stderr? Does the generated instruction sequence pass the checker?
- **minishell:** Does the shell handle invalid commands and syntax errors without crashing or quitting unexpectedly?
- **so_long:** Does the game reject an invalid map and terminate cleanly without crashes or memory leaks?

## Learning Summary

| Concept | One-line takeaway |
|---|---|
| Testing in 42 | Testing is more than running the project; results must be checked against the subject requirements. |
| Testing and debugging | Testing identifies failures, while debugging identifies and fixes their causes. |
| Testing and QA | Testing evaluates the project, while QA improves the development process. |
| Test process | ISTQB test activities can be mapped to the lifecycle of a 42 project. |
| Independence | Self-testing supports development, while peer evaluation provides an independent perspective. |
| Peer validation | Other 42 students validate project compliance before curriculum completion. |

> *ISTQB Chapter 1 concepts were mapped to how projects are developed, tested, and peer-evaluated at 42 Vienna.*