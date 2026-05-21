# Search Functionality Test Design Document (T1–T5)

## Management Summary
This document defines a structured T1–T5 test design for the search functionality of Toolshop.

The objective is to ensure comprehensive and maintainable test coverage for the search feature across:
- Standard cases
- Alternative cases
- Exception cases
- Negative cases
- Misuse cases

---

# Feature Overview

**Feature Under Test:** Search Functionality  
**Application:** Toolshop  
**Base URL:** https://practicesoftwaretesting.com  
**Business Importance:** High  
**Primary Goal:** Ensure users can reliably find products under normal and abnormal conditions.

---

# Test Design Strategy

## Coverage Model
- **T1:** Standard Cases
- **T2:** Alternative Cases
- **T3:** Exception Cases
- **T4:** Negative Cases
- **T5:** Misuse Cases

---

# Test Case Template

## Test Case Structure
- Test Case ID
- T-Level
- Test Name
- Priority (High / Medium / Low)
- Automation Candidate (Yes / No)
- Preconditions
- Input Data
- Test Steps
- Expected Result
- Risks / Notes

---

# T1 – Standard Cases

## TC-T1-01
**T-Level:** T1  
**Test Name:** T1_search_hammer_6productsFound  
**Priority:** High  
**Automation Candidate:** Yes  

**Preconditions:**  
Product catalog contains multiple searchable products matching "hammer"

**Input Data:**  
Search term: Hammer

**Test Steps:**  
 1. Navigate to "/"
 2. Enter "hammer" into search field
 3. Click "Search" button

**Expected Result:**  
- Text visible "6 products found for 'hammer'"

**Risks / Notes:**  

 - There could be inconsistencies with shown message and listed articles. 
 - Stable test data needed.
-----


## TC-T1-02
**T-Level:** T1  
**Test Name:** T1_search_thorHammer_1productFound  
**Priority:** High  
**Automation Candidate:** Yes  

**Preconditions:**  
Product catalog contains "Thor hammer"

**Input Data:**  
Search term: Thor hammer

**Test Steps:**  
 1. Navigate to "/"
 2. Enter "Thor hammer" into search field
 3. Click "Search" button


**Expected Result:**  
- Text visible "1 product found for 'Thor hammer'"

**Risks / Notes:**  

---

# T2 – Alternative Cases

## TC-T2-01
**T-Level:** T2  
**Test Name:** T2_search_mer_7productsFound  
**Priority:** Medium  
**Automation Candidate:** Yes  

**Preconditions:**  
Product catalog contains multiple products with name "hammer"

**Input Data:**  
Search term: mer

**Test Steps:**  
1. Navigate to "/"
2. Enter "mer" into search field
3. Click "Search" button  

**Expected Result:**  
- Text visible "7 products found for 'mer'"

**Risks / Notes:**  
Product with the name "sledgehammer" only appears on partial searches.

---

## TC-T2-02
**T-Level:** T2  
**Test Name:** T2_search_HaMmer_6productsFound  
**Priority:** Medium  
**Automation Candidate:** Yes  

**Preconditions:**  
Product catalog contains multiple products with name "hammer"

**Input Data:**  
Search term: HaMmer

**Test Steps:**  
1. Navigate to "/"
2. Enter "HaMmer" into search field
3. Click "Search" button 

**Expected Result:**  
- Text visible "6 products found for 'HaMmer'"

**Risks / Notes:**  

---

# T3 – Exception Cases

## TC-T3-01
**T-Level:** T3  
**Test Name:** T3_search_specialCharacterInput_noProductsDisplayed  
**Priority:** Medium  
**Automation Candidate:** Yes  

**Preconditions:**  
Product catalog contains searchable products

**Input Data:**  
Search term: @#$%

**Test Steps:**  
1. Navigate to "/"
2. Enter "@#$%" into search field
3. Click "Search" button  

**Expected Result:**  
- Text visible "There are no products found."
- No product should be listed

**Risks / Notes:**  
Searches with different special characters should not behave differently than others.

---

## TC-T3-02
**T-Level:** T3  
**Test Name:** T3_search_longSearchInput_noProductsDisplayed  
**Priority:** Medium  
**Automation Candidate:** Yes  

**Preconditions:**  
Product catalog contains searchable products

**Input Data:**  
Search term: industrial heavy duty cordless electric hammer drill

**Test Steps:**  
1. Navigate to "/"
2. Enter "industrial heavy duty cordless electric hammer drill" into search field
3. Click "Search" button 

**Expected Result:**  
- Text visible "There are no products found."
- No product should be listed

**Risks / Notes:**  
It should be defined how long of an input is allowed and the expected behaviour. 
Search results should be consistent with backend API response.

---

# T4 – Negative Cases

## TC-T4-01
**T-Level:** T4  
**Test Name:** T4_search_emptyInput_allProductsDisplayed  
**Priority:** Medium  
**Automation Candidate:** No  

**Preconditions:**  
Product catalog contains searchable products

**Input Data:**  
No input data

**Test Steps:**  
1. Navigate to "/"
2. Make sure the search field is empty
3. Click "Search" button  

**Expected Result:**  
- All 45 products are returned

**Risks / Notes:**  
There is an "X" button which returns all products but it is not an obvious workflow for users.

---

## TC-T4-02
**T-Level:** T4  
**Test Name:** T4_search_lightSaber_noProductsDisplayed  
**Priority:** Medium  
**Automation Candidate:** Yes  

**Preconditions:**  
Product catalog does not contain "light saber"

**Input Data:**  
Search term: light saber

**Test Steps:**  
1. Navigate to "/"
2. Enter "light saber" into search field
3. Click "Search" button  

**Expected Result:**  
- Text visible "0 products found for 'light saber'"
- No products listed

**Risks / Notes:**  


---

# T5 – Misuse Cases

## TC-T5-01
**T-Level:** T5  
**Test Name:** T5_search_SQLInjectionInput_noProductsDisplayed  
**Priority:** High  
**Automation Candidate:** Yes  

**Preconditions:**  
Application is accessible

**Input Data:**  
Search term : ' OR '1'='1

**Test Steps:**  
1. Navigate to "/"
2. Enter "' OR '1'='1" into search field
3. Click "Search" button

**Expected Result:**  
- Text visible "There are no products found."
- No unintended data is exposed
- Input is treated as plain text

**Risks / Notes:**  
- Potential SQL injection vulnerability

---

## TC-T5-02
**T-Level:** T5  
**Test Name:** T5_search_scriptInjectionInput_noProductsDisplayed  
**Priority:** High  
**Automation Candidate:** Yes  

**Preconditions:**  
Application is accessible

**Input Data:**  
Search term: `<script>alert('test')</script>`

**Test Steps:**  
1. Navigate to "/"
2. Enter `<script>alert('test')</script>` into search field
3. Click "Search" button

**Expected Result:**  
- Text visible "There are no products found."
- Script is not executed  
- No alert or popup is displayed

**Risks / Notes:**  
- Potential Cross-Site Scripting (XSS) vulnerability

---

# Test Data Considerations
- Define stable product data for known searches
- Prevent unexpected product catalog changes from affecting test results
- Ensure test data supports independent and isolated future automation

---

# Coverage Summary

## Covered Areas
- Standard searches
- Partial matches
- Empty input
- Invalid searches
- Special characters
- Search robustness
- Potentially problematic inputs
- Automation readiness