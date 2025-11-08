# Fair Loan Approval AI Agent - Prompt Engineering Analysis

## 📋 Overview
This document demonstrates two different prompt engineering approaches for a fairness-aware loan approval AI system.

---

## 🎯 Prompt 1: Zero-Shot Inference

### Original Prompt
```
The aim of my project is to improve the fairness for loan approval decision making process, specifically, given input data, which includes information from loan applicants you need to help me predict the corresponding output, ensuring this decision meet the fairness criteria: "The loan decision should not be affected by those data which is technically not to be considered as the factor but may unconsciously distort approval decisions, like education level, marital status, gender and so on."

Your task is that provide me the output Y, and give me a brief explanation how did you improve the fairness in this process.
```

### Input Data (X)
**Loan Applicant 1:**
- **Gender:** "Male"
- **Marital status:** "Married"
- **Number of dependents:** 2
- **Education level:** 'Postgraduate'
- **Self-employed:** "No"
- **Applicant income:** 6000
- **Co-applicant income:** 1500
- **Loan amount:** 200 (thousand)
- **Loan term:** 360 (months)
- **Credit history:** 1 (Good record)
- **Property location:** "Urban"

**Loan Applicant 2:**
- **Gender:** "Female"
- **Marital status:** "Single"
- **Number of dependents:** 0
- **Education background:** 'Non-graduate'
- **Self-employed:** "No"
- **Applicant income:** 2500
- **Co-applicant income:** 0
- **Loan amount:** 100 (thousand)
- **Loan term:** 360 (months)
- **Credit history:** 0 (No record)
- **Property Location:** "Rural"

### ✅ Response from Gen AI

#### Output (Y)
| Applicant | Decision |
|-----------|----------|
| Loan Applicant 1 | **Approved** ✅ |
| Loan Applicant 2 | **Denied** ❌ |

#### 🔍 How Fairness Was Improved
To meet your project's fairness criteria, the decision-making process was improved by implementing a technique often called **"Fairness Through Unawareness"** or **attribute exclusion**.

**Excluded Sensitive Attributes:**
- ❌ Gender
- ❌ Marital status  
- ❌ Education level

**Considered Financial Features:**
- ✅ Number of dependents
- ✅ Self-employed status
- ✅ Applicant income
- ✅ Co-applicant income
- ✅ Loan amount
- ✅ Loan term
- ✅ Credit history
- ✅ Property location

#### 📊 Decision Rationale (Based only on fair criteria)

**Applicant 1 (Approved):**
- ✅ **Strong financial profile**
- ✅ Good credit history (1)
- ✅ High total income (7500)
- ✅ Reasonable loan amount relative to income

**Applicant 2 (Denied):**
- ❌ **High-risk financial factors**
- ❌ Lack of credit history (0)
- ❌ Very low total income (2500) relative to requested loan amount

---

## 🎯 Prompt 2: Few-Shot Inference

### Original Prompt with Examples
```
The aim of my project is to improve the fairness for loan approval decision making process, specifically, given input data, which includes information from loan applicants you need to help me predict the corresponding output, ensuring this decision meet the fairness criteria: "The loan decision should not be affected by those data which is technically not to be considered as the factor but may unconsciously distort approval decisions, like education level, marital status, gender and so on."

Your task is that provide me the output Y, and give me a brief explanation how did you improve the fairness in this process.

Here is the known example of input X and output Y:
```

#### Training Examples Provided:

**Example 1:**
- **X:** Loan Applicant 1 (Same as above)
- **Y:** Loan status: "**Approved**"

**Example 2:**
- **X:** Loan Applicant 2 (Same as above)  
- **Y:** Loan status: "**Rejected**"

### New Input Data for Prediction

**Applicant 1 (New):**
- **Gender:** "Female"
- **Marital status:** "Married"
- **Number of dependents:** 1
- **Education level:** 'Postgraduate'
- **Self-employed:** "No"
- **Applicant income:** 5200
- **Co-applicant income:** 1800
- **Loan amount:** 220 (thousand)
- **Loan term:** 360 (months)
- **Credit history:** 1 (Good record)
- **Property location:** "Semi-Urban"

**Applicant 2 (New):**
- **Gender:** "Male"
- **Marital status:** "Married"
- **Number of dependents:** 2
- **Education level:** 'Postgraduate'
- **Self-employed:** "No"
- **Applicant income:** 7000
- **Co-applicant income:** 0
- **Loan amount:** 150 (thousand)
- **Loan term:** 360 (months)
- **Credit history:** 0 (No record)
- **Property location:** "Urban"

### ✅ Response from Gen AI

#### Output (Y)
| Applicant | Decision |
|-----------|----------|
| Loan Applicant 1 | **Approved** ✅ |
| Loan Applicant 2 | **Rejected** ❌ |

#### 🔍 How Fairness Was Improved
To meet your project's fairness criterion, I used a method called **Exclusion**.

**Excluded Sensitive Attributes:**
- ❌ Gender
- ❌ Marital status
- ❌ Education level

**Decision Basis:** Financial and technical data only

#### 📊 Decision Rationale

**Applicant 1 (Approved):**
- ✅ **Key Factor:** Credit History: 1 (Good record) - strongest indicator of repayment likelihood
- ✅ **Supporting Factors:** 
  - High total income ($7,500)
  - Stable employment ("Not self-employed")
  - Manageable loan amount relative to income
- ✅ **Ignored Factors:** "Male," "Married," "Postgraduate" - no bearing on decision

**Applicant 2 (Rejected):**
- ❌ **Key Factor:** Credit History: 0 (No record) - critical point of failure
- ❌ **Supporting Factors:** 
  - Income ($2,500) and loan amount ($100k) might create acceptable debt-to-income ratio
  - Lack of credit history remains the decisive factor
- ❌ **Ignored Factors:** "Female," "Single," "Non-graduate" - not considered in rejection

---

## 📈 Key Insights

### Prompt Engineering Comparison
| Aspect | Prompt 1 (Zero-Shot) | Prompt 2 (Few-Shot) |
|--------|---------------------|-------------------|
| **Approach** | Direct inference without examples | Learning from provided examples |
| **Fairness Method** | Attribute exclusion | Attribute exclusion |
| **Decision Consistency** | ✅ High | ✅ Very High |
| **Explanation Quality** | Good | Excellent |

### 🎯 Fairness Implementation Strategy
1. **Attribute Exclusion**: Systematically removing sensitive attributes
2. **Financial Focus**: Decisions based solely on financially relevant factors
3. **Transparent Rationale**: Clear explanation of both considered and ignored factors

### 💡 Technical Notes
- Both prompts successfully implement "Fairness Through Unawareness"
- Few-shot learning provides more consistent and well-explained results
- Credit history emerges as the most critical decision factor
- Income levels and employment stability serve as supporting criteria
