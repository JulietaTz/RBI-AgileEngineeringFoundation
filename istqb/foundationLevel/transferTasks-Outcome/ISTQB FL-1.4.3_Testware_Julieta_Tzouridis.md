# Apply Traceability between the Test Basis and Testware on Toolshop

## 1. Title
Apply Traceability between the Test Basis and Testware – Product Search

## 2. Syllabus Reference
ISTQB FL – 1.4.4 Traceability between the Test Basis and Testware

## 3. Selected Feature Flow
Product Search

Website:
https://practicesoftwaretesting.com/

## 4. Test Basis

Requirement / Behavior Statement:

The search functionality should display products that match the entered search keyword.

Assumption:
When the user searches for "Screwdriver", the result list should show screwdriver-related products.

## 5. Created Testware

| Testware ID | Testware Type | Description |
|---|---|---|
| TC-SEARCH-001 | Test Case | Verify search results for keyword "Screwdriver" |
| TD-SEARCH-001 | Test Data | Search keyword: Screwdriver |
| ER-SEARCH-001 | Expected Result | Relevant screwdriver products are displayed |
| EX-SEARCH-001 | Execution Result | Search returned matching screwdriver products |

## 6. Traceability Matrix

| Test Basis ID | Requirement / Behavior | Linked Testware | Coverage |
|---|---|---|---|
| REQ-SEARCH-001 | Search should return products matching the entered keyword | TC-SEARCH-001, TD-SEARCH-001, ER-SEARCH-001, EX-SEARCH-001 | Covered |

## 7. Test Case

### Test Case ID
TC-SEARCH-001

### Test Title
Verify that the search function returns relevant products for "Screwdriver"

### Preconditions
- User is on the Toolshop homepage
- Product data is available
- Internet connection is working

### Test Data
| Input Keyword |
|---|
| Screwdriver |

### Test Steps

| Step | Action | Expected Result |
|---|---|---|
| 1 | Open Toolshop homepage (https://practicesoftwaretesting.com/) | Homepage loads successfully |
| 2 | Click into the search field | Search field is active |
| 3 | Enter "Screwdriver" | Keyword is visible in the search field |
| 4 | Click Search | Search results page is displayed |
| 5 | Review listed products | 2 Products related to screwdriver are shown |

## 8. Actual Result

The search returned 2 products for "Screwdriver":

- Phillips Screwdriver
- Mini Screwdriver

Execution Status: PASS

## 9. Evidence

Observation from Toolshop:
The page displayed the text "Searched for: Screwdriver" and showed 2 matching products.

## 10. Risk / Quality Impact

| Risk | Impact |
|---|---|
| Requirement is not linked to testware | Missing test coverage may not be noticed |
| Search test is not traceable | Scrum team cannot easily prove what was tested |
| Search returns wrong products | Users may not find relevant products |
| Missing products in search results | Possible loss of sales and lower user satisfaction |

## 11. Conclusion

This activity demonstrates traceability between the test basis and testware.

The requirement REQ-SEARCH-001 is linked to:
- test case
- test data
- expected result
- execution result

This makes it clear which requirement was tested and what evidence supports the result. The artifact can be used in the current sprint for review, regression testing, and quality reporting.