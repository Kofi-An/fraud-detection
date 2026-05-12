# Financial Fraud Detection

> XGBoost fraud detection model on 284,807 real credit card
> transactions. Solves the class imbalance problem (578:1 ratio)
> using SMOTE and delivers SHAP explainability for regulatory
> compliance.

---

## Results

| Metric | Result |
|--------|--------|
| AUC-ROC | 0.9708 |
| AUC-PR | 0.8026 (479x above random baseline) |
| Recall | 75.79% |
| Precision | 93.51% |
| F1 Score | 0.8372 |
| Specificity | 99.99% |
| False alarms | 5 out of 56,651 normal transactions |
| Net benefit | $8,774 on test set (352x ROI) |

---

## The core challenge

A naive model predicting no fraud on every transaction
achieves 99.83% accuracy — and catches zero fraudsters.
Accuracy is the wrong metric entirely for imbalanced data.

This project addresses three real challenges every
fraud detection team faces daily:

**1. Class imbalance** — 492 fraud cases in 284,807
transactions (0.17%). Solved with SMOTE resampling
applied to training data only.

**2. Metric selection** — Precision-Recall AUC used as
primary metric instead of ROC-AUC. At 0.17% fraud rate
ROC is optimistic. PR-AUC of 0.8026 vs random baseline
of 0.0017 — a 479x improvement.

**3. Explainability** — SHAP values provide feature-level
explanation for every prediction. Regulatory requirement
for deployed fraud models. Black-box models cannot be
approved by risk committees.

---

## Key findings

**SHAP feature importance:**

| Rank | Feature | Mean SHAP | Interpretation |
|------|---------|-----------|----------------|
| 1 | V14 | 1.6475 | Dominant fraud signal |
| 2 | V4 | 1.2789 | Strong fraud signal |
| 3 | V12 | 0.6463 | Moderate signal |
| 4 | V8 | 0.5701 | Moderate signal |
| 5 | V1 | 0.5568 | Moderate signal |

V14 is 29% stronger than V4 — the single most
predictive feature. V1-V28 are PCA-transformed
components whose raw meaning is confidential to the
issuing bank. SHAP reveals their relative importance
regardless of the anonymisation.

**Individual fraud explanation:**

For the highest-confidence fraud transaction:
- V14 contributed +5.39 toward fraud prediction
- V8 and V18 pulled -0.58 and -0.52 away from fraud
- The fraudster mimicked legitimate behaviour in
  certain dimensions — a sophisticated fraud pattern

**Data quality finding:**

One of 5 false alarms had a fraud probability of
0.9999 with an identical SHAP signature to confirmed
fraud cases. This transaction is almost certainly
mislabelled in the original dataset — demonstrating
that SHAP explainability functions as a data quality
audit tool not just a model explanation tool.

---

## Two-pattern fraud signature

EDA revealed a classic two-pattern fraud distribution:

- **Card testing** — 13.82% of fraud transactions are
  near-zero amounts. Fraudsters verify a stolen card
  is active before making larger purchases.
- **High-value fraud** — a small number of large
  transactions pull the mean to $122.21 despite a
  median of only $9.25
- **98% of fraud transactions** drive 80% of total
  fraud value — Pareto concentration applies
- **578:1 imbalance ratio** — one of the most extreme
  in any public dataset

---

## Approach

| Step | Description |
|------|-------------|
| 1. EDA | Class imbalance, amount analysis, fraud patterns |
| 2. Deduplication | 1,081 duplicates removed before splitting |
| 3. SMOTE | Applied to training data only (578:1 → 1:1) |
| 4. XGBoost | PR-AUC optimised, 300 estimators |
| 5. Threshold | Optimal F1 cutoff analysis |
| 6. SHAP | Global importance, waterfall, dependence plots |
| 7. Business | $8,774 net benefit, 352x ROI |

---

## Tech stack

![Python](https://img.shields.io/badge/Python-3.11-blue?style=flat)
![XGBoost](https://img.shields.io/badge/XGBoost-green?style=flat)
![SMOTE](https://img.shields.io/badge/imbalanced--learn-orange?style=flat)
![SHAP](https://img.shields.io/badge/SHAP-purple?style=flat)
![scikit-learn](https://img.shields.io/badge/scikit--learn-red?style=flat)

---

## Notebooks

| Notebook | Description |
|----------|-------------|
| 01_eda.ipynb | EDA, imbalance, fraud patterns, deduplication |
| 02_smote_resampling.ipynb | SMOTE, quality verification |
| 03_xgboost_model.ipynb | Training, PR curve, business impact |
| 04_shap_explainability.ipynb | SHAP global, waterfall, data audit |

---

## How to run

```bash
git clone https://github.com/Kofi-An/fraud-detection
cd fraud-detection
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
# Download creditcard.csv from Kaggle — place in data/raw/
jupyter notebook
```

---

## Data source

ULB Machine Learning Group — Credit Card Fraud Detection
284,807 transactions | 492 fraud cases | 0.17% fraud rate
kaggle.com/datasets/mlg-ulb/creditcardfraud

---

## Limitations

- V1-V28 are PCA-transformed — raw feature meaning
  unavailable due to bank confidentiality
- Dataset contains mislabelled transactions
  identified via SHAP audit
- 2-day observation window limits temporal analysis
- Results may differ on datasets with different
  base rates or feature distributions

---

## Related projects

- [Credit Risk Scorecard](https://github.com/Kofi-An/credit-risk-scorecard)
  — AUC 0.71, $275M loss reduction
- [Portfolio Risk Dashboard](https://kofi-an-portfolio-risk-dashboard.streamlit.app)
  — Live VaR, CVaR, Monte Carlo app

---

## Author

Kofi-An | Financial Data Scientist
Open to remote roles in fraud analytics,
quant risk, and financial data science

[GitHub](https://github.com/Kofi-An) ·
[LinkedIn](https://linkedin.com/in/your-handle) ·
[Portfolio](https://kofi-an.github.io)