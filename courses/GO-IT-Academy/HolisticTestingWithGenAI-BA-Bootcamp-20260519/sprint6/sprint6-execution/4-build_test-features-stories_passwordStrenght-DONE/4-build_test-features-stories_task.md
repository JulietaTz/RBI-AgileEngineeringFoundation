
# Transfer Task: Test password strenght feature

## 1. Description

Design a set of **test situations** to validate the password strenght feature.

A **test situation** is an isolated condition under which the test object displays a specific behaviour that needs to be tested. Test situations are in other words isolated or individual possibilities that needs to be tested.

---

## 2. Background

You are part of a product team preparing the next sprint.

---

## 3. References

- User Story:             sprint6-input/US3206-hashed-password.md


---

## Task Description

### Part 1 

1.) Analyze the password policy
    The password policy must enforce the following rules:
    - Minimum length of 8 characters
    - At least one uppercase letter  
    - At least one lowercase letter
    - At least one numeric digit
    - At least one special character (e.g. @, #, $, %, &)

2.) Design as set of test situations with a set of passwords according to the password policy with 3 different character sets
    - Character sets for following languages (use each special character from the languages)
        - German
        - Czech
        - English
    - Invalid passwords
    - Valid passwords (with different strenght)

3.) Create a table with the password, the expected strenght level, status (valid/invalid)

4.) Use different LLM's to see the differences in the designs test situations

5.) Use the test situations for testing the password strenght feature.