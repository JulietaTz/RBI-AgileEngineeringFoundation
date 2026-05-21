# 🧪 Test Techniques Overview — Extended Cheat Sheet

---

## 🔍 What are Test Techniques?

Test techniques help testers to:
- identify **what to test** (test conditions)
- define **how to test** (test cases)

👉 Goal:
Create a **small but effective set of tests** with good coverage.

They support:
- systematic testing
- risk reduction
- better defect detection

---

# 🔲 Black-Box Testing (Specification-based)

## 🎯 Focus
👉 **What the system should do**

Tests are derived from:
- requirements
- user stories
- specifications

---

## 🧠 Key Idea
The tester does **not look at the code**.  
Testing is based only on **expected behavior**.

➡️ If the implementation changes, tests still remain valid (as long as behavior stays the same).

---

## 💡 Why use it?
- validates business requirements  
- ensures system behaves correctly from user perspective  
- independent from developers  

---

## 🛠️ Techniques

### 1. Equivalence Partitioning
👉 Divide inputs into groups with similar behavior  

**Example:**
Age field (0–100)
- valid: 1–100  
- invalid: <0, >100  

➡️ Test only one value per group

---

### 2. Boundary Value Analysis
👉 Focus on edge values (where bugs often occur)

**Example:**
Allowed: 1–100  
Test:
- 0, 1, 100, 101  

---

### 3. Decision Table Testing
👉 Test combinations of conditions

**Example:**
Login:
| Username | Password | Result |
|---------|---------|--------|
| valid | valid | success |
| valid | invalid | error |

---

### 4. State Transition Testing
👉 Test behavior based on system states

**Example:**
Login system:
- logged out → login → logged in  
- locked after 3 failed attempts  

---

## 📌 Example (Login)
- valid password → ✅ Success  
- invalid password → ❌ Error  

---

## ⚠️ Limitation
- does NOT verify internal logic  
- may miss implementation defects  

---

# ⚙️ White-Box Testing (Structure-based)

## 🎯 Focus
👉 **How the system works internally**

---

## 🧠 Key Idea
Tests are based on:
- code structure
- logic paths
- control flow

➡️ Requires understanding of the implementation

---

## 💡 Why use it?
- ensures code is fully tested  
- identifies hidden logic errors  
- provides measurable coverage  

---

## 🛠️ Techniques

### 1. Statement Coverage
👉 Ensure every line of code is executed

**Example:**
Each statement runs at least once

---

### 2. Branch Coverage
👉 Ensure all decision outcomes are tested

**Example:**
```python
if (x > 10):
    print("OK")
else:
    print("Not OK")


    ## Example

```python
if (x > 10):
    print("OK")
else:
    print("Not OK")
```

Test cases:
- `x = 11` → true branch
- `x = 5` → false branch

---

## 🔄 Comparison at a Glance

| Aspect | Black-Box | White-Box | Experience-Based |
|--------|-----------|-----------|------------------|
| Based on | Requirements / Specifications | Code / Structure | Knowledge / Experience |
| Focus | What the system does | How the system works | Where defects are likely |
| Knowledge of Code | Not required | Required | Not required |
| Main Goal | Validate functionality | Verify logic & coverage | Find unexpected defects |
| Strength | User perspective, implementation independent | Measures coverage, ensures paths are tested | Detects hidden / hard-to-find issues |
| Best Used | Functional & acceptance testing | Unit testing, code-level testing | Exploratory, ad-hoc, risk areas |
---

# 📍 When to Use (Expanded)

## 🔲 Black-Box
👉 Validate functionality from a **user perspective**

Use when:
- testing requirements  
- verifying user stories  
- acceptance testing  

---

## ⚙️ White-Box
👉 Verify **logic, code quality, and coverage**

Use when:
- unit testing  
- checking conditions and branches  
- ensuring all paths are executed  

---

## 🧠 Experience-Based
👉 Explore, discover and find **hidden issues**

Use when:
- requirements are unclear  
- time is limited  
- testing high-risk areas  

---

# ⭐ Best Practice

👉 **Combine all three techniques**

Because:
- Black-box → ensures correct behavior  
- White-box → ensures correct implementation  
- Experience-based → finds what others miss  

---

# 🎯 Key Takeaway (Important!)

**No single technique is enough.**

👉 Using only one technique leads to:
- gaps in coverage  
- missed defects  

👉 Combining techniques leads to:
- better defect detection  
- higher software quality  
- more confidence in the system  

---

# 💡 Final Insight

Smart testing is NOT about:
❌ running more tests  

👉 It is about:
✔ using the **right techniques**  
✔ in the **right context**  

