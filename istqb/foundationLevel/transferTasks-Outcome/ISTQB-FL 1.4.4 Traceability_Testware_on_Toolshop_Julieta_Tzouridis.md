# Apply Traceability between the Test Basis and Testware on Toolshop

## 1. Title

Apply Traceability between the Test Basis and Testware on Toolshop – Product Search

## 2. Syllabus Reference

ISTQB FL – 1.4.4 Traceability between the Test Basis and Testware


## 3. Learning Objective

Apply ISTQB FL section 1.4.4 (Traceability between the Test Basis and Testware) in a realistic webshop testing situation and create a sprint-ready testing artifact.


## 4. Context / Scenario

The Scrum team is preparing the next Toolshop increment before release.

Selected feature flow: Product Search

Website:
[https://practicesoftwaretesting.com/]

Selected keyword: "Screwdriver"

The purpose of this activity is to demonstrate traceability between the test basis and created testware.


# 5. Test Basis

## Requirement ID

REQ-SEARCH-001

## Requirement

The search functionality should display products matching the entered keyword.

## Assumption

When the user searches for "Screwdriver", the webshop should display relevant screwdriver-related products.


# 6. Created Traceability Artifacts

| Artifact ID    | Artifact Type       | Description                      |
| -------------- | ------------------- | -------------------------------- |
| REQ-SEARCH-001 | Requirement         | Search functionality requirement |
| TC-SEARCH-001  | Test Case           | Product search validation test   |
| TD-SEARCH-001  | Test Data           | Search keyword "Screwdriver"     |
| ER-SEARCH-001  | Expected Result     | Relevant products are displayed  |
| EX-SEARCH-001  | Execution Result    | Actual observed search results   |



# 7. Test Case

## Test Case ID

TC-SEARCH-001

## Test Title

Verify that the product search returns relevant products for the keyword "Screwdriver"

## Test Type

Functional Regression Test

## Preconditions

* User is on the Toolshop homepage (https://practicesoftwaretesting.com/)
* Internet connection is available
* Product data is accessible



# 8. Test Data

| Test Data ID  | Input       |
| ------------- | ----------- |
| TD-SEARCH-001 | Screwdriver |


# 9. Test Steps

| Step | Action                      | Expected Result                         |
| ---- | ----------------------------| --------------------------------------- |
| 1    | Open Toolshop homepage (https://practicesoftwaretesting.com/)| Homepage loads successfully             |
| 2    | Click on the search field   | Search field becomes active             |
| 3    | Enter keyword "Screwdriver" | Keyword appears in the search field     |
| 4    | Execute search              | Search results page is displayed        |
| 5    | Review displayed products   | Relevant screwdriver products are shown |

---

# 10. Expected Result

## Expected Result ID

ER-SEARCH-001

The application should display products related to the keyword "Screwdriver".

Expected 2 Products:

* Phillips Screwdriver
* Mini Screwdriver

# 11. Execution Result

## Execution Result ID

EX-SEARCH-001

## Actual Result

The search returned matching screwdriver-related products.

Observed products:

* Phillips Screwdriver
* Mini Screwdriver

## Execution Status

PASS


# 12. Traceability Matrix

## Traceability Matrix ID

TM-SEARCH-001

| Test Basis     | Linked Testware | Coverage Status |
| -------------- | --------------- | --------------- |
| REQ-SEARCH-001 | TC-SEARCH-001   | Covered         |
| REQ-SEARCH-001 | TD-SEARCH-001   | Covered         |
| REQ-SEARCH-001 | ER-SEARCH-001   | Covered         |
| REQ-SEARCH-001 | EX-SEARCH-001   | Covered         |


# 13. Traceability Coverage Explanation

The requirement REQ-SEARCH-001 is fully linked to:

* test case
* test data
* expected result
* execution result

This traceability helps the Scrum team understand:

* which requirement was tested
* which test artifacts belong to the requirement
* which results provide evidence of testing
* whether coverage exists for the requirement


# 14. Risk / Quality Impact

| Risk                        | Impact                                      |
| --------------------------- | ------------------------------------------- |
| Missing traceability        | Untested requirements may not be identified |
| Missing coverage visibility | Scrum team may not know what was validated  |
| Incorrect search behavior   | Reduced usability and possible sales loss   |
| Missing test evidence       | Difficulties during audits or reviews       |

The search functionality is business-critical because users rely on it to quickly locate products.


# 15. Conclusion

This activity demonstrates ISTQB FL 1.4.4 Traceability between the Test Basis and Testware.

The requirement REQ-SEARCH-001 was linked to multiple test artifacts:

* test case
* test data
* expected result
* execution result
* traceability matrix

The created artifact is sprint-ready and can be used for:

* sprint reviews
* regression testing
* QA reporting
* coverage discussions
* future automation preparation

The artifact improves transparency, requirement coverage visibility, and quality assurance within the Scrum team.
