# Scrum Test Automation Activity Map

\#\# User Journeys  
\- Journey 1: Login to account  
\- Journey 2: Search for a product    
\- Journey 3: Add product to cart

\---

\#\# Backlog Refinement

\*\*Automation Activities\*\*  
\- Define clear, testable acceptance criteria including inputs, outputs, and edge cases for login, search, and cart flows  
\- Ensure features are testable (stable selectors for login form, search results, cart actions)    
\- Identify dependencies between journeys (is login required before adding to cart)    
\- Identify test data requirements (valid/invalid users, available products)    
\- Identify automation candidates based on stability and frequency (e.g. login and search as high-value candidates)

\*\*Anti-Pattern\*\*  
\- Vague acceptance criteria

\---

\#\# Sprint Planning

\*\*Automation Activities\*\*  
\- Decide test strategy for each journey (API tests for login, UI tests for cart interactions)    
\- Estimate automation effort for login, search, and cart scenarios  
\- Define test data requirements (reusable test users and product data)    
\- Define and align test environment for automation (browsers, test environment, and configurations)   
\- Include automated tests for selected journeys in Definition of Done

\*\*Anti-Pattern\*\*  
\- Automation effort is not included in sprint estimation

\---

\#\# Daily Scrum

\*\*Automation Activities\*\*  
\- Report status of automated tests for login, search, and cart (done/planned)  
\- Raise blockers for test automation (missing test users, changing or unreliable product data)    
\- Align with development team on changes impacting automation (login API changes, UI changes in cart)    
\- Monitor automated test stability and investigate failures (flaky search results)    
\- Review automated test results from CI runs

\*\*Anti-Pattern\*\*  
\- Automation progress is not communicated within the team 

\---

\#\# Development

\*\*Automation Activities\*\*  
\- Implement automated tests for login, search, and add-to-cart scenarios (API and UI where appropriate)  
\- Create and manage test data for automation (test users, products)    
\- Integrate tests for these journeys into CI pipeline    
\- Stabilize automated tests and reduce flakiness  
\- Develop reusable automation components (login helpers, cart actions)    
\- Collaborate with developers to improve testability (stable selectors, test APIs)

\*\*Anti-Pattern\*\*  
\- Perform test automation only after development is completed

\---

\#\# Sprint Review

\*\*Automation Activities\*\*  
\- Present the results of automated tests (login with valid credentials passed, search with special characters failed)  
\- Present how many automated tests passed and how many failed (5 of 30 automated test cases failed)  
\- Give info on test coverage (edge cases for login not automated yet)  
\- Demonstrate system stability with automated test results   
\- Get feedback from stakeholders on quality

\*\*Anti-Pattern\*\*  
\- Automated test results not presented

\---

\#\# Retrospective

\*\*Automation Activities\*\*  
\- Analyse issues (search test fails because of unstable test data, cart tests fail because of changing selectors)  
\- Define improvements (introduce stable selector ids, ensure stable test data, use API tests instead of UI tests when necessary)  
\- Define action items (implement stable selector ids, prepare API call to create test data)  
\- Improve automated tests (remove hardcoded values, add reusable helper elements)  
\- Reflect on QA & DEV collaboration for early testing

\*\*Anti-Pattern\*\*  
\- Ignoring flaky tests

\---

