# Test Data Specialist Agent


## Data Classification Model

### 🔴 SPII (Sensitive Personal Information)
Highly sensitive personal data requiring strongest protection.

**Examples:**
- Credit card numbers, CVV
- IBAN, banking data
- Social security numbers
- Health data
- Authentication secrets

**Default Strategy:**
- Anonymization or Tokenization
- Prefer full irreversibility

---

### 🟠 PII (Personally Identifiable Information)
Data that can identify a person directly or indirectly.

**Examples:**
- Name, email, phone number
- Address, date of birth
- IP address (in many contexts)

**Default Strategy:**
- Pseudonymization or Anonymization

---

### 🟡 Confidential / Restricted Data
Critical system or security-related data.

**Examples:**
- Password hashes
- API keys
- Security configurations

**Default Strategy:**
- Redaction or Synthetic Replacement

---

### 🔵 Business Sensitive Data
Data critical for competitive or financial reasons.

**Examples:**
- Revenue, profit, margins
- Cost price, supplier contracts
- Sales forecasts

**Default Strategy:**
- Obfuscation or Generalization

---

### 🟣 Behavioral Data
User behavior and interaction data.

**Examples:**
- Session logs
- Clickstreams
- Purchase history

**Default Strategy:**
- Pseudonymization
- Aggregation if needed

---

### ⚪ Identifiers (Direct / Indirect)
Technical identifiers enabling linkage.

**Examples:**
- customer_id
- session_id
- device_id

**Default Strategy:**
- Pseudonymization
- Tokenization (if cross-system)

---

### 🟢 Operational / System Data
Technical system-level data.

**Examples:**
- Logs
- Monitoring metrics
- System events

**Default Strategy:**
- Usually no masking
- Mask only if combined with PII

---

### 🟤 Analytical / Derived Data
Aggregated or computed data.

**Examples:**
- KPIs
- Conversion rates
- Aggregated reports

**Default Strategy:**
- Aggregation thresholds
- Generalization

---

### ⚫ Public Data
Non-sensitive, publicly available data.

**Examples:**
- Product descriptions
- Public catalog data

**Default Strategy:**
- No masking required


