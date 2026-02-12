# Financial Risk Decisioning Framework

## 📌 Overview

This project develops a credit risk decisioning framework to evaluate whether a company should be approved for lending.

Instead of treating the task as a simple classification problem, the solution translates predictive modeling outputs into financial impact using Expected Credit Loss (ECL), and defines a clear approve/decline policy.

The framework integrates:

- Probability of Default (PD) estimation
- Risk tier segmentation (A/B/C/D)
- Company-level explainability (SHAP-based)
- Expected Credit Loss computation
- Portfolio-level approval strategy

---

## 🎯 Problem Statement

The objective is to:

1. Predict financial distress (default proxy).
2. Generate a structured risk profile per company:
   - `pd` (Probability of Default)
   - `risk_tier` (A/B/C/D segmentation)
   - `top_reasons` (Top 3 risk drivers)
3. Define an ECL-based decision rule.
4. Report portfolio-level financial impact.

---

## 📊 Methodology

### 1️⃣ Data Exploration

- Identified class imbalance (~3% distress rate).
- Analyzed financial ratios such as ROA.
- Observed non-linear relationships between profitability and bankruptcy risk.

---

### 2️⃣ Model Development

- **Baseline:** Logistic Regression  
- **Improved Model:** XGBoost (to capture non-linear patterns)

Evaluation metrics:

- ROC-AUC
- PR-AUC (important for imbalanced data)
- Cross-validation
- Calibration curve
- Brier score

Final model selected: **XGBoost**

---

### 3️⃣ Risk Tiering Framework

Predicted PD values were segmented into four tiers based on percentiles:

- Tier A – Lowest risk
- Tier B – Low-moderate risk
- Tier C – Moderate-high risk
- Tier D – Highest risk

Observed bankruptcy rate increases monotonically from Tier A to Tier D.

Tier D exhibits significantly elevated distress (~12.7%) compared to overall portfolio (~3%).

---

### 4️⃣ Risk Profile Output

For each company, a structured risk profile is generated:

- **PD:** Predicted probability of default
- **Risk Tier:** A/B/C/D classification
- **Top Reasons:** Top 3 SHAP-based drivers influencing prediction

This enhances transparency and supports defensible credit decisions.

---

### 5️⃣ Expected Credit Loss (ECL)

ECL is computed as:

ECL = PD × LGD × EAD

Assumptions:

- EAD = USD 10,000
- LGD = 60%

---

### 6️⃣ Decision Policy

Approval rule:

> Approve if ECL < USD 100

Portfolio Results (Test Set):

- Approval Rate: ~82.7%
- Total Expected Loss: ~USD 25.8k
- Expected Defaults: ~4.3 companies
- Distress Recall: ~92.7%

This demonstrates a balanced trade-off between growth and capital protection.

---

## 📈 Key Insights

- Profitability (ROA-related features) strongly influences bankruptcy risk.
- Risk is effectively concentrated in the highest PD tier.
- Probability calibration supports reliable ECL estimation.
- Financially grounded thresholds provide more meaningful decisions than fixed PD cutoffs.

---

## 🛠 Installation

Clone the repository:

```bash
git clone https://github.com/Sarumathi17/fintech-credit-risk-decisioning.git
cd fintech-credit-risk-decisioning
```
Install dependencies:

```bash
pip install -r requirements.txt
```
--- 

## 🚀 Run Notebook

Open:

```bash
fintech_risk_assignment.ipynb
```
---

## ⚠️ Challenges & Mitigation

### 1. Class Imbalance

The dataset contains a low default rate (~3%), making accuracy an unreliable metric.

To address this:

- Focused on PR-AUC instead of relying solely on ROC-AUC
- Evaluated recall for the distressed class
- Analyzed confusion matrix to monitor false negatives

---

### 2. Translating Model Output into Business Decisions

Predicted probabilities alone do not provide actionable lending decisions.

To address this:

- Converted PD into Expected Credit Loss (ECL)
- Defined an ECL-based approval threshold
- Evaluated portfolio-level metrics (approval rate, expected loss, expected defaults)

---

### 3. Probability Calibration

Since PD directly influences ECL, probability reliability is critical.

To ensure stability:

- Evaluated calibration curve
- Computed Brier score
- Confirmed reasonable alignment between predicted and observed default rates

---

### 4. Model Interpretability

Tree-based models offer strong performance but reduced interpretability.

To improve transparency:

- Applied SHAP for local explanations
- Generated top 3 risk drivers per company
- Created structured risk profiles for defensible decisioning

---

## 📌 Conclusion

This project demonstrates how predictive modeling can be translated into an actionable credit decision framework by integrating risk estimation, segmentation, explainability, and financial impact analysis.
