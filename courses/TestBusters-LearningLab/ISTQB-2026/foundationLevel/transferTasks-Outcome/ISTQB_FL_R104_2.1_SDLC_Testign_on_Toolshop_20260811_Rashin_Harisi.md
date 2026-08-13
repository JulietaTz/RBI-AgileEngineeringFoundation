# Apply Testing in the Context of a Software Development Lifecycle (SDLC) on Toolshop

## Reference to ISTQB Syllabus chapter
ISTQB FL – 2.1 Testing in the Context of a Software Development Lifecycle (SDLC)

## Link to the transfer task file
https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter%202/ISTQB-FL-2.1_Testing_in_the_Context_of_a_Software_Development_Lifecycle_SDLC_20260405.md

## Outcome

**Artifact type:** Sprint-ready test report for the Toolshop login flow

# How the SDLC Changes Testing on Toolshop 

## Selected Feature Flow
Login

## Requirement / Behavior Reference

According to the Sprint 5 documentation, Toolshop supports user authentication (JWT), provides Login / Register pages, and exposes the `/auth` route for the authentication module.

Based on this documented functionality, I selected the following login behaviors for testing:

1. Verify that an appropriate error message is displayed when the user enters an incorrect password.
2. Verify that a successfully authenticated user is redirected to the expected account page.

## Test Observations

### Test 1 – Login with Incorrect Password

| Item | Observation |
|---|---|
| Steps | Open the login page, enter a registered email with an incorrect password, and click **Login**. |
| Input | Valid registered email + incorrect password |
| Expected Result | Login should be rejected and an appropriate error message should be displayed. |
| Actual Result | Login was rejected and the message `Invalid email or password` was displayed. |
| Risk / Quality Impact | Incorrect error handling can confuse users and make authentication problems harder to understand. |

### Test 2 – Successful Login

| Item | Observation |
|---|---|
| Steps | Open the login page, enter valid credentials, and click **Login**. |
| Input | Valid registered email + valid password |
| Expected Result | The user should be authenticated and redirected to the expected account page. |
| Actual Result | The user was successfully authenticated and redirected to the user profile page. |
| Risk / Quality Impact | A failure can prevent authenticated users from accessing their account and related functionality. |

- **Sequential SDLC :** V-Model
- **Agile SDLC :** Agile uses an iterative and incremental development model in which testing is performed continuously within each iteration.

| Question | Sequential SDLC | Agile SDLC (Toolshop) | Reflection |
|----------|-----------------|-----------------------|------------|
| **When can dynamic testing start?** | After the login feature is implemented and executable. | As soon as a working login increment is available during the sprint. | The login feature can be tested much earlier in Agile. |
| **When can static testing happen?** | During reviews of login requirements, design, and code. | Continuously through user story, acceptance criteria, and code reviews. | Early reviews help prevent login defects before implementation. |
| **How detailed is test documentation?** | Detailed test plans and test cases prepared in advance. | Lightweight documentation focused on user stories, acceptance criteria, and automated tests. | Documentation stays focused on what the team needs. |
| **Which test techniques fit best?** | Test cases designed from documented requirements. | Test cases updated and executed continuously during each sprint. | Agile allows continuous improvement of login tests as the feature evolves. |
| **How much test automation is needed?** | Moderate automation, mainly for stable functionality. | High automation for login, API, and regression testing. | Automated login tests provide fast feedback after every sprint. |
| **What test levels can you use?** | Component, integration, system, and acceptance testing. | The same test levels performed throughout each sprint. | The login feature is verified repeatedly as it evolves. |
| **What is the tester's main role?** | Verify that the completed login feature meets the documented requirements. | Collaborate with developers and provide continuous quality feedback throughout the sprint. | Quality is a shared responsibility in Agile. |
| **How much regression testing is needed?** | After significant changes and before release. | After every sprint or whenever the login functionality changes. | Frequent regression testing helps ensure new changes do not break login. |
---

## Sprint-ready Conclusion

Toolshop favors test automation and lightweight documentation because frequent Agile changes require fast regression feedback while keeping documentation focused, maintainable, and useful to the team.

## Learning Summary

| Concept | One-line takeaway |
|---|---|
| SDLC impact | The development lifecycle influences when and how testing is performed. |
| Sequential SDLC | Testing activities are more separated and often happen after larger development phases. |
| Agile SDLC | Testing is performed continuously during each sprint. |
| Static testing | Requirements, acceptance criteria, and code can be reviewed before or during implementation. |
| Dynamic testing | Working increments can be tested as soon as they are available. |
| Automation | Frequent Agile changes increase the value of automated regression testing. |
| Documentation | Agile favors lightweight, focused, and maintainable test documentation. |

