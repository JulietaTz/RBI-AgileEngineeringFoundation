# SAP-HR Suite – Example Database Schema

This schema models the core entities and relationships for a typical SAP-HR (Human Resources) suite. It covers employee master data, organizational structure, payroll, time management, and related HR processes.

---

## 1. Employees
| Column            | Data Type      | Description                       |
|-------------------|---------------|-----------------------------------|
| employee_id       | VARCHAR(20)   | Unique employee identifier        |
| first_name        | VARCHAR(100)  | First name                        |
| last_name         | VARCHAR(100)  | Last name                         |
| date_of_birth     | DATE          | Date of birth                     |
| gender            | CHAR(1)       | Gender (M/F/D)                    |
| national_id       | VARCHAR(50)   | National ID / SSN                 |
| email             | VARCHAR(255)  | Work email                        |
| phone_number      | VARCHAR(50)   | Work phone                        |
| hire_date         | DATE          | Hire date                         |
| termination_date  | DATE          | Termination date                  |
| employment_status | VARCHAR(20)   | Active, Terminated, Retired, etc. |
| org_unit_id       | VARCHAR(20)   | FK to org_units                   |
| position_id       | VARCHAR(20)   | FK to positions                   |
| manager_id        | VARCHAR(20)   | FK to employees (self-ref)        |

---

## 2. Organizational Units
| Column         | Data Type     | Description                |
|----------------|--------------|----------------------------|
| org_unit_id    | VARCHAR(20)  | Unique org unit identifier |
| name           | VARCHAR(100) | Org unit name              |
| parent_org_id  | VARCHAR(20)  | Parent org unit (nullable) |
| location       | VARCHAR(100) | Location                   |

---

## 3. Positions
| Column         | Data Type     | Description                |
|----------------|--------------|----------------------------|
| position_id    | VARCHAR(20)  | Unique position identifier |
| title          | VARCHAR(100) | Position title             |
| org_unit_id    | VARCHAR(20)  | FK to org_units            |
| job_code       | VARCHAR(20)  | Job code                   |
| grade          | VARCHAR(20)  | Pay grade                  |

---

## 4. Payroll
| Column           | Data Type     | Description                        |
|------------------|--------------|------------------------------------|
| payroll_id       | VARCHAR(20)  | Unique payroll record              |
| employee_id      | VARCHAR(20)  | FK to employees                    |
| pay_period_start | DATE         | Pay period start                   |
| pay_period_end   | DATE         | Pay period end                     |
| gross_pay        | DECIMAL(12,2)| Gross pay                          |
| net_pay          | DECIMAL(12,2)| Net pay                            |
| tax_amount       | DECIMAL(12,2)| Tax withheld                       |
| deductions       | DECIMAL(12,2)| Other deductions                   |
| payment_date     | DATE         | Date paid                          |
| iban             | VARCHAR(34)  | Bank account (IBAN)                |

---

## 5. Time Management (Attendance)
| Column           | Data Type     | Description                        |
|------------------|--------------|------------------------------------|
| attendance_id    | VARCHAR(20)  | Unique attendance record           |
| employee_id      | VARCHAR(20)  | FK to employees                    |
| date             | DATE         | Attendance date                    |
| attendance_type  | VARCHAR(20)  | Present, Sick, Vacation, etc.      |
| hours_worked     | DECIMAL(5,2) | Hours worked                       |

---

## 6. Addresses
| Column         | Data Type     | Description                |
|----------------|--------------|----------------------------|
| address_id     | VARCHAR(20)  | Unique address identifier  |
| employee_id    | VARCHAR(20)  | FK to employees            |
| street         | VARCHAR(255) | Street address             |
| city           | VARCHAR(100) | City                       |
| postal_code    | VARCHAR(20)  | Postal code                |
| country        | VARCHAR(100) | Country                    |
| address_type   | VARCHAR(20)  | Home, Mailing, etc.        |

---

## 7. Emergency Contacts
| Column         | Data Type     | Description                |
|----------------|--------------|----------------------------|
| contact_id     | VARCHAR(20)  | Unique contact identifier  |
| employee_id    | VARCHAR(20)  | FK to employees            |
| name           | VARCHAR(100) | Contact name               |
| relationship   | VARCHAR(50)  | Relationship to employee   |
| phone_number   | VARCHAR(50)  | Contact phone              |

---

## 8. Qualifications
| Column           | Data Type     | Description                |
|------------------|--------------|----------------------------|
| qualification_id | VARCHAR(20)  | Unique qualification ID    |
| employee_id      | VARCHAR(20)  | FK to employees            |
| type             | VARCHAR(100) | Degree, Certification, etc.|
| institution      | VARCHAR(100) | Granting institution       |
| date_awarded     | DATE         | Date awarded               |

---

## 9. User Accounts
| Column         | Data Type     | Description                |
|----------------|--------------|----------------------------|
| user_id        | VARCHAR(20)  | Unique user account        |
| employee_id    | VARCHAR(20)  | FK to employees            |
| username       | VARCHAR(100) | Login name                 |
| password_hash  | VARCHAR(255) | Hashed password            |
| last_login     | TIMESTAMP    | Last login timestamp       |
| is_locked      | BOOLEAN      | Account locked flag        |

---

## 10. Audit Log
| Column         | Data Type     | Description                |
|----------------|--------------|----------------------------|
| audit_id       | VARCHAR(20)  | Unique audit record        |
| user_id        | VARCHAR(20)  | FK to user_accounts        |
| action         | VARCHAR(100) | Action performed           |
| action_time    | TIMESTAMP    | When action occurred       |
| details        | TEXT         | Additional details         |

---

# Relationships
- employees.manager_id → employees.employee_id (self-reference)
- employees.org_unit_id → org_units.org_unit_id
- employees.position_id → positions.position_id
- positions.org_unit_id → org_units.org_unit_id
- payroll.employee_id → employees.employee_id
- attendance.employee_id → employees.employee_id
- addresses.employee_id → employees.employee_id
- emergency_contacts.employee_id → employees.employee_id
- qualifications.employee_id → employees.employee_id
- user_accounts.employee_id → employees.employee_id
- audit_log.user_id → user_accounts.user_id
