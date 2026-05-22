
# Apply Testware on Toolshop

## 1. Title
Apply Testware on Toolshop – Product Search Regression Test


## 2. Syllabus Reference
ISTQB FL 4.0 – 1.4.2 Testware

### Definition
Testware includes all work products created and used during testing activities.

Examples of testware:
- test cases
- test data
- expected results
- execution results
- defect reports
- test reports

This activity demonstrates the creation and usage of testware on the Toolshop webshop application.



## 3. Learning Objective
Apply ISTQB FL section 1.4.2 (Testware) in a realistic webshop testing scenario and create reusable testing artifacts for the Scrum team.


## 4. Context / Scenario
The Scrum team is preparing the next Toolshop increment before release.

The selected feature flow is:
- Product Search

Website:
https://practicesoftwaretesting.com/

The goal of this activity is to create sprint-ready testware that can later be reused for:
- regression testing
- sprint testing
- automation preparation
- quality reporting


## 5. Requirement / Behavior Statement

### Requirement ID
REQ-SEARCH-001

### Requirement
The search functionality should display relevant products matching the entered keyword.

### Assumption
When the user searches for "Screwdriver", the application should display products related to screwdrivers.



## 6. Created Testware

| Testware ID | Type | Description |
|---|---|---|
| TC-SEARCH-001 | Test Case | Validates search functionality |
| TD-SEARCH-001 | Test Data | Search keyword "Screwdriver" |
| ER-SEARCH-001 | Expected Result | Relevant products are displayed |
| EX-SEARCH-001 | Execution Result | Actual observed search results |
| RN-SEARCH-001 | Risk Notes | Business and usability risks |


# 7. Test Case

## Test Case ID
TC-SEARCH-001

## Test Title
Verify that valid product keywords return matching search results

## Test Type
Regression Test

## Priority
High

## Preconditions
- User is on the Toolshop homepage
- Internet connection is available
- Product database is accessible



## Test Data

| Data ID | Input |
|---|---|
| TD-SEARCH-001 | Screwdriver |



## Test Steps

| Step | Action | Expected Result |
|---|---|---|
| 1 | Open Toolshop homepage | Homepage loads successfully |
| 2 | Click into the search field | Search field becomes active |
| 3 | Enter keyword "Screwdriver" | Keyword is visible in the search field |
| 4 | Submit search request | Search results page is displayed |
| 5 | Review displayed products | 2 Relevant screwdriver products are shown |


## Expected Result

The system should display products related to the search term "Screwdriver".

Expected 2 products:
- Phillips Screwdriver
- Mini Screwdriver

---

## Actual Result

The search returned 2 matching products:
- Phillips Screwdriver
- Mini Screwdriver

The application displayed:
- correct search keyword
- matching products
- valid search result count

---

## Execution Result

| Execution ID | Status | Notes |
|---|---|---|
| EX-SEARCH-001 | PASS | Relevant products were displayed successfully |

---

# 8. Risk / Quality Impact

| Risk ID | Risk | Impact |
|---|---|---|
| RN-001 | Incorrect search results | Users may not find products |
| RN-002 | Missing products in results | Potential sales loss |
| RN-003 | Irrelevant results | Reduced usability and customer trust |
| RN-004 | Broken search functionality | High business impact |

### Quality Impact
The search functionality is business-critical because customers use it to quickly locate products inside the webshop.

---

# 9. Evidence

### Observed Behavior
The application displayed:
- "Searched for: Screwdriver"
- 2 matching products

### Observed Products
- Phillips Screwdriver
- Mini Screwdriver

### Execution Status
PASS

---

# 10. Traceability

| Requirement | Linked Testware |
|---|---|
| REQ-SEARCH-001 | TC-SEARCH-001 |
| REQ-SEARCH-001 | TD-SEARCH-001 |
| REQ-SEARCH-001 | ER-SEARCH-001 |
| REQ-SEARCH-001 | EX-SEARCH-001 |

---

# 11. Conclusion

This activity demonstrates ISTQB FL section 1.4.2 Testware by creating reusable testing artifacts for the Toolshop webshop.

Created testware:
- supports regression testing
- improves sprint documentation
- helps future automation activities
- provides evidence for quality assurance

The artifact is sprint-ready and can be shared during:
- daily stand-ups
- sprint reviews
- QA reporting
- regression testing preparation

