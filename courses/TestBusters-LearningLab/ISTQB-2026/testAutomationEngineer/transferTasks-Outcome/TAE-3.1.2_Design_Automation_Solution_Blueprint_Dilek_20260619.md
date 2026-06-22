# Toolshop Test Automation Solution Blueprint
## 1. Automation Scope
The initial automation scope is based on the identified Most Valuable Tests (MVTs) and focuses on the most important customer workflows.

The initial objective is to establish a reliable smoke regression suite that provides fast feedback and validates the most critical business workflows.

## 2. Solution Design
### 2.1 Framework Style
 - Playwright with TypeScript
 - Page Object Model (POM)
 - Data-Driven Testing (DDT)
 - Keyword-Driven Testing (KDT)

### 2.2 Folder Structure


```bash
toolshop-automation/
│
├── tests/
├── pages/
├── api/
├── test-data/
├── utils/
├── config/
├── reports/
└── screenshots/
```

### 2.3 Naming Conventions

#### Pages

- LoginPage.ts
- ProductListPage.ts
- CheckoutPage.ts

#### Tests

- login.spec.ts
- searchProduct.spec.ts
- checkout.spec.ts

#### Methods
- enterEmail()
- loginUser()
- searchProduct()
- addProductToCart()

## 3. Environment Configuration
Environment-specific settings should be externalized and configurable.  

**Examples:**  
BASE_URL  
API_BASE_URL

Configuration values should be managed through environment variables and configuration files.

## 4. Test Data Strategy
 - Product test data should be created through APIs whenever possible.
 - Unique user data should be generated during test execution.
 - Test data should be separated from test logic.
 - JSON files may be used for reusable test data.
 - Automated tests should remain independent and executable in any order.

## 5. Execution Model

### Local Execution
Executed by developers and testers on local workstations using Playwright.

### CI Execution
Executed automatically within the CI pipeline to provide rapid feedback on code changes.
Test execution results should be published to Testiny for centralized reporting and traceability.

## 6. Failure Diagnostics
The automation solution should collect diagnostic information to support root cause analysis.

Diagnostics include:
- Automatic screenshots on failure
- Playwright trace files
- Execution logs
- Test run metadata
- HTML test reports
- Test execution results stored in Testiny

## 7. Design Decisions

### DD-01: Use Playwright with TypeScript as the Primary Automation Framework

**Rationale:**  
Playwright supports both UI and API automation within a single framework, reducing tool complexity. TypeScript improves maintainability and helps identify errors earlier during development.

### DD-02: Use the Page Object Model (POM)

**Rationale:**  
Separates test logic from UI implementation details and improves maintainability.

### DD-03: Create Test Data Through APIs Whenever Possible

**Rationale:**  
API-based setup is faster, more reliable, and reduces unnecessary UI interactions.

### DD-04: Generate Unique User Data During Test Execution

**Rationale:**  
Supports repeated execution without database cleanup and prevents data conflicts.

### DD-05: Use API-Based Authentication for Tests That Do Not Require Login Validation

**Rationale:**  
Reduces execution time and keeps tests focused on the functionality under test.
