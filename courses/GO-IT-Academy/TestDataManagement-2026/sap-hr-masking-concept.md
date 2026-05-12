# Data Masking Concept for SAP-HR Suite Schema

Date: 2026-04-12

This document provides a structured data masking concept for the SAP-HR suite schema, based on the data classification model and masking rules.

---

## Table Analysis & Column-Level Classification

### 1. Employees
| Column            | Classification | Masking Required | Technique           | Example Transformation         |
|-------------------|---------------|------------------|---------------------|-------------------------------|
| employee_id       | Identifier    | Yes              | Tokenization        | EMP001 → TKN_EMP_0001         |
| first_name        | PII           | Yes              | Pseudonymization    | Anna → NameX                   |
| last_name         | PII           | Yes              | Pseudonymization    | Müller → NameY                 |
| date_of_birth     | PII           | Yes              | Generalization      | 1985-07-23 → 1985-01-01        |
| gender            | PII           | Yes              | Generalization      | M → M                         |
| national_id       | SPII          | Yes              | Tokenization        | 123-45-6789 → TKN_NID_0001     |
| email             | PII           | Yes              | Anonymization       | anna.m@example.com → anon1@domain.test |
| phone_number      | PII           | Yes              | Redaction           | +43-123-4567 → +43-***-****    |
| hire_date         | Confidential  | Yes              | Date Shift          | 2020-03-01 → 2019-12-15        |
| termination_date  | Confidential  | Yes              | Date Shift          | 2024-01-31 → 2023-10-15        |
| employment_status | Confidential  | No               | -                   | Active → Active                |
| org_unit_id       | Identifier    | Yes              | Tokenization        | OU001 → TKN_OU_0001            |
| position_id       | Identifier    | Yes              | Tokenization        | POS001 → TKN_POS_0001          |
| manager_id        | Identifier    | Yes              | Tokenization        | EMP002 → TKN_EMP_0002          |

### 2. Organizational Units
| Column         | Classification | Masking Required | Technique           | Example Transformation         |
|---------------|---------------|------------------|---------------------|-------------------------------|
| org_unit_id   | Identifier    | Yes              | Tokenization        | OU001 → TKN_OU_0001            |
| name          | Confidential  | Yes              | Pseudonymization    | HR → DeptX                     |
| parent_org_id | Identifier    | Yes              | Tokenization        | OU002 → TKN_OU_0002            |
| location      | Confidential  | Yes              | Generalization      | Vienna → RegionX               |

### 3. Positions
| Column         | Classification | Masking Required | Technique           | Example Transformation         |
|---------------|---------------|------------------|---------------------|-------------------------------|
| position_id   | Identifier    | Yes              | Tokenization        | POS001 → TKN_POS_0001          |
| title         | Confidential  | Yes              | Pseudonymization    | Manager → TitleX               |
| org_unit_id   | Identifier    | Yes              | Tokenization        | OU001 → TKN_OU_0001            |
| job_code      | Confidential  | Yes              | Tokenization        | JOB123 → TKN_JOB_0001          |
| grade         | Confidential  | Yes              | Generalization      | A1 → GradeX                    |

### 4. Payroll
| Column           | Classification | Masking Required | Technique           | Example Transformation         |
|------------------|---------------|------------------|---------------------|-------------------------------|
| payroll_id       | Identifier    | Yes              | Tokenization        | PAY001 → TKN_PAY_0001          |
| employee_id      | Identifier    | Yes              | Tokenization        | EMP001 → TKN_EMP_0001          |
| pay_period_start | Confidential  | Yes              | Date Shift          | 2024-01-01 → 2023-10-01        |
| pay_period_end   | Confidential  | Yes              | Date Shift          | 2024-01-31 → 2023-10-31        |
| gross_pay        | Business Sensitive | Yes          | Synthetic Data      | 5000.00 → 4321.00              |
| net_pay          | Business Sensitive | Yes          | Synthetic Data      | 3500.00 → 3210.00              |
| tax_amount       | Business Sensitive | Yes          | Synthetic Data      | 1000.00 → 900.00               |
| deductions       | Business Sensitive | Yes          | Synthetic Data      | 500.00 → 400.00                |
| payment_date     | Confidential  | Yes              | Date Shift          | 2024-02-01 → 2023-11-01        |
| iban             | SPII          | Yes              | Tokenization        | AT611904300234573201 → TKN_IBAN_0001 |

### 5. Time Management (Attendance)
| Column           | Classification | Masking Required | Technique           | Example Transformation         |
|------------------|---------------|------------------|---------------------|-------------------------------|
| attendance_id    | Identifier    | Yes              | Tokenization        | ATT001 → TKN_ATT_0001          |
| employee_id      | Identifier    | Yes              | Tokenization        | EMP001 → TKN_EMP_0001          |
| date             | Confidential  | Yes              | Date Shift          | 2024-01-02 → 2023-10-02        |
| attendance_type  | Confidential  | No               | -                   | Present → Present              |
| hours_worked     | Confidential  | No               | -                   | 8.00 → 8.00                    |

### 6. Addresses
| Column         | Classification | Masking Required | Technique           | Example Transformation         |
|---------------|---------------|------------------|---------------------|-------------------------------|
| address_id    | Identifier    | Yes              | Tokenization        | ADDR001 → TKN_ADDR_0001        |
| employee_id   | Identifier    | Yes              | Tokenization        | EMP001 → TKN_EMP_0001          |
| street        | PII           | Yes              | Synthetic Data      | Main St 1 → StreetX            |
| city          | PII           | Yes              | Generalization      | Vienna → CityX                 |
| postal_code   | PII           | Yes              | Format-Preserving Tokenization | 1010 → 1X1X         |
| country       | PII           | Yes              | Generalization      | Austria → CountryX             |
| address_type  | Confidential  | No               | -                   | Home → Home                    |

### 7. Emergency Contacts
| Column         | Classification | Masking Required | Technique           | Example Transformation         |
|---------------|---------------|------------------|---------------------|-------------------------------|
| contact_id    | Identifier    | Yes              | Tokenization        | EC001 → TKN_EC_0001            |
| employee_id   | Identifier    | Yes              | Tokenization        | EMP001 → TKN_EMP_0001          |
| name          | PII           | Yes              | Pseudonymization    | Peter → NameZ                  |
| relationship  | Confidential  | No               | -                   | Spouse → Spouse                |
| phone_number  | PII           | Yes              | Redaction           | +43-987-6543 → +43-***-****    |

### 8. Qualifications
| Column           | Classification | Masking Required | Technique           | Example Transformation         |
|------------------|---------------|------------------|---------------------|-------------------------------|
| qualification_id | Identifier    | Yes              | Tokenization        | Q001 → TKN_Q_0001              |
| employee_id      | Identifier    | Yes              | Tokenization        | EMP001 → TKN_EMP_0001          |
| type             | Confidential  | No               | -                   | Degree → Degree                |
| institution      | Confidential  | Yes              | Pseudonymization    | Uni Vienna → UniX              |
| date_awarded     | Confidential  | Yes              | Date Shift          | 2010-07-01 → 2009-07-01        |

### 9. User Accounts
| Column         | Classification | Masking Required | Technique           | Example Transformation         |
|---------------|---------------|------------------|---------------------|-------------------------------|
| user_id       | Identifier    | Yes              | Tokenization        | U001 → TKN_U_0001              |
| employee_id   | Identifier    | Yes              | Tokenization        | EMP001 → TKN_EMP_0001          |
| username      | Confidential  | Yes              | Tokenization        | jdoe → TKN_USER_0001           |
| password_hash | Restricted    | Yes              | Redaction           | $2a$10$... → [REDACTED]        |
| last_login    | Confidential  | Yes              | Date Shift          | 2024-03-01 → 2023-12-01        |
| is_locked     | Confidential  | No               | -                   | TRUE → TRUE                    |

### 10. Audit Log
| Column         | Classification | Masking Required | Technique           | Example Transformation         |
|---------------|---------------|------------------|---------------------|-------------------------------|
| audit_id      | Identifier    | Yes              | Tokenization        | AUD001 → TKN_AUD_0001          |
| user_id       | Identifier    | Yes              | Tokenization        | U001 → TKN_U_0001              |
| action        | Confidential  | No               | -                   | LOGIN → LOGIN                  |
| action_time   | Confidential  | Yes              | Date Shift          | 2024-03-01 08:00 → 2023-12-01 08:00 |
| details       | Confidential  | Yes              | Redaction           | ... → [REDACTED]               |

---

## Rationale & Risk Notes
- All direct and indirect identifiers are tokenized to preserve referential integrity.
- All PII/SPII is masked using irreversible or format-preserving techniques.
- Payroll and financial data is replaced with synthetic values to avoid business leakage.
- Password hashes and audit details are always redacted.
- Combination of quasi-identifiers (e.g., birth date + org unit + position) must be monitored for re-identification risk.
- Consistent tokenization is required for all FKs and PKs.

---

## Compliance
- All masking is designed to meet GDPR and DORA requirements.
- No real sensitive data is exposed in non-production environments.
- Masking preserves test utility and referential integrity.
