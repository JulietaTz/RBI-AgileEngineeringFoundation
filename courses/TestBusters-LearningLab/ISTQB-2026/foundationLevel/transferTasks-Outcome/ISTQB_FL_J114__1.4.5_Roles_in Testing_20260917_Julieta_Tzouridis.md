# J114 - Apply Roles in Testing on Toolshop

## Reference to ISTQB Syllabus Chapter

ISTQB FL – 1.4.5 Roles in Testing

## Link to the Transfer Task File

https://github.com/JulietaTz/RBI-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter%201/ISTQB-FL-1.4.5_Roles_in_Testing_20260405.md

## Outcome

### Learning Summary

ISTQB distinguishes between two principal roles in testing:

| Role | Main Responsibility |
|---|---|
| Test Management Role | Test planning, test monitoring, test control and test completion |
| Testing Role | Test analysis, test design, test implementation and test execution |

The exact activities assigned to these roles depend on the project context, the product, the organization and the skills of the people involved.

Different people may perform these roles at different times. One person may also perform both roles.

In an Agile environment, some test management tasks may be shared within the Agile team.

For this transfer task, the **Customer Login flow** of Toolshop was selected.

---

## Selected Feature

**Feature:** Customer Login

**Application:**  
https://practicesoftwaretesting.com/

**Release Context:** Sprint 5

### Requirement / Expected Behaviour

A registered customer should be able to log in with valid credentials and access the customer account.

The Toolshop environment provides test users that can be used for this activity.

### Test Account

- **Email:** `customer@practicesoftwaretesting.com`
- **Password:** `welcome01`

---

## Roles in Testing

### 1. Testing Role

For this transfer task, I mainly perform the **Testing Role**.

According to ISTQB, the Testing Role is responsible for the technical or engineering aspects of testing.

For the Toolshop Login flow, this includes:

- analyzing the expected login behaviour
- identifying the test condition
- designing the test case
- preparing the required test data
- implementing the test
- executing the test
- comparing expected and actual results
- documenting the test result
- reporting a defect if the actual result differs from the expected result

The Testing Role therefore focuses mainly on:

- test analysis
- test design
- test implementation
- test execution

---

### 2. Test Management Role

The **Test Management Role** has overall responsibility for the test process and the coordination of testing activities.

For the Toolshop Login flow, this may include:

- defining the Login flow as part of the test scope
- determining the priority of the Login functionality
- considering login-related product risks
- planning the required testing activities
- monitoring the progress of testing
- controlling testing when changes are required
- evaluating whether sufficient testing has been performed
- completing and communicating the test status

The Test Management Role therefore focuses mainly on:

- test planning
- test monitoring
- test control
- test completion

In an Agile environment, some of these responsibilities may be shared within the Agile team.

---

## Application in an Agile / Scrum Context

The two ISTQB roles are responsibility areas and are not necessarily fixed job titles.

Different members of a Scrum team may contribute to testing activities.

| Team Member | Possible Contribution |
|---|---|
| Tester | Performs test analysis, test design, test implementation and test execution |
| Developer | Supports testing, investigates defects and implements fixes |
| Product Owner | Clarifies business requirements, expected behaviour and acceptance criteria |
| Scrum Master | Supports collaboration and helps remove process impediments |
| Agile Team / Test Management Role | Supports planning, monitoring, prioritization and completion of testing |

These team members are not additional testing roles defined by ISTQB section 1.4.5.

The two principal roles remain:

1. **Testing Role**
2. **Test Management Role**

Quality is a shared responsibility within the Agile team, while individual testing responsibilities can still be clearly identified.

---

# Mini Testing Activity

## Test Case

**Test Case ID:** TC-LOGIN-001

**Title:** Login with valid customer credentials

### Objective

Verify that a registered customer can successfully log in to Toolshop using valid credentials.

### Preconditions

- Toolshop is available.
- The customer account exists.
- The user is currently logged out.
- The Login page is accessible.

### Test Data

| Field | Value |
|---|---|
| Email | `customer@practicesoftwaretesting.com` |
| Password | `welcome01` |

---

## Test Steps

| Step | Action | Expected Result |
|---|---|---|
| 1 | Open https://practicesoftwaretesting.com/ | Toolshop homepage is displayed |
| 2 | Click **Sign in** | Login page is displayed |
| 3 | Enter the valid email address | Email is entered successfully |
| 4 | Enter the valid password | Password is entered successfully |
| 5 | Click **Login** | User is successfully authenticated |
| 6 | Check the page after login | The **My account** page is displayed and the customer account functions are accessible |

---

## Expected Result

The registered customer is successfully authenticated and redirected to the **My account** page.

The customer account and its available functions are accessible.

---

## Actual Result

The customer was successfully authenticated with the valid test credentials.

After login, the **My account** page was displayed and the customer account was accessible.

The following account functions were visible:

- Favorites
- Profile
- Invoices
- Messages

The displayed user was **Jane Doe**.

The actual result matches the expected result.

---

## Test Result

**Status:** PASS

**Evidence:** Manual execution of the Toolshop Customer Login flow.

The user was successfully authenticated and redirected to the **My account** page.

**Defect:** None.

No deviation from the expected behaviour was observed.

---

## Risk / Quality Impact

Login is an important business functionality because registered customers need authentication to access account-related features.

Possible impacts of a Login defect include:

- customers cannot access their accounts
- customers cannot access personal account information
- authenticated functionality may not be available
- checkout functionality for authenticated customers may be affected
- customer trust may be reduced

Because of this impact, the Login flow is relevant for regression testing.

---

## Role Contribution During This Activity

| Activity | ISTQB Role |
|---|---|
| Define the test scope | Test Management Role |
| Determine priority and risk | Test Management Role |
| Plan the testing activity | Test Management Role |
| Analyze the Login behaviour | Testing Role |
| Identify test conditions | Testing Role |
| Design the test case | Testing Role |
| Prepare test data | Testing Role |
| Implement the test | Testing Role |
| Execute the test | Testing Role |
| Compare expected and actual results | Testing Role |
| Document the result | Testing Role |
| Monitor testing progress | Test Management Role |
| Decide whether additional testing is required | Test Management Role |
| Complete and communicate the testing status | Test Management Role |

---

## Conclusion

This activity demonstrates that testing consists of different responsibilities and is not limited to test execution.

The **Testing Role** is mainly responsible for the technical testing activities:

- test analysis
- test design
- test implementation
- test execution

The **Test Management Role** is mainly responsible for:

- test planning
- test monitoring
- test control
- test completion

The exact distribution of these responsibilities depends on the project context.

In an Agile environment, some test management responsibilities may be performed by members of the Agile team.

Different people may perform the two roles at different times, and one person may also perform both the Testing Role and the Test Management Role.

For this transfer task, I mainly performed the **Testing Role** because I analyzed the Login flow, designed the test case, prepared the test data, executed the test and documented the result.

The test passed successfully and no defect was identified.

---

### Final Learning Summary

| Concept | One-line Takeaway |
|---|---|
| Test Management Role | Responsible mainly for planning, monitoring, control and completion of testing |
| Testing Role | Responsible mainly for analysis, design, implementation and execution |
| Roles are flexible | Different people may perform the roles depending on the project context |
| Agile context | Some test management tasks may be shared within the Agile team |
| Combined roles | One person may perform both roles |
| Testing is more than execution | Testing includes analysis, design, implementation, execution and management activities |