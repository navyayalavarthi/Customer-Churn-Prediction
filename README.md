## 📉 Customer Churn Prediction System

![Python](https://img.shields.io/badge/Python-3.11-blue)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-1.3-orange)
![SHAP](https://img.shields.io/badge/SHAP-Explainability-purple)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboards-yellow)
![SQL](https://img.shields.io/badge/SQL-SQLAlchemy-green)

## Project Overview

Developed machine learning models to identify customers at risk of churn and support retention strategy planning across 500K+ customer records. Built end-to-end pipeline from SQL analysis through model training, SHAP explainability, K-Means segmentation, and interactive Power BI dashboards identifying $157M+ revenue at risk.

## Key Results

| Metric | Value |
|--------|-------|
| Best Model | Logistic Regression |
| ROC-AUC | 0.8462 |
| Recall | 77.54% |
| Precision | 50.61% |
| F1 Score | 61.24% |
| Total Customers | 500,000 |
| High Risk Customers | 169,000 |
| Revenue At Risk | $157M+ |

## Project Architecture
```  
WA_Fn-UseC_-Telco-Customer-Churn.csv
↓
[SQL Analysis Layer]
SQLite — 4 analytical queries
Churn by contract, internet, charges, senior
↓
[Feature Engineering Layer]
charges_per_tenure ratio
service_count (8 services)
contract_risk score
senior_no_support flag
high_spender flag
↓
[Segmentation Layer]
K-Means Clustering K=4
Elbow method for K selection
↓
[ML Model Layer]
Logistic Regression AUC 0.8462 — Best
Gradient Boosting   AUC 0.8311
↓
[SHAP Explainability]
Top churn driver per customer
Feature importance beeswarm plot
↓
[Output Layer]
churn_predictions_500k.csv
churn_predictions_500k.xlsx
↓
[Power BI Dashboard]
4 pages — 13 DAX measures
$157M+ Revenue At Risk
```
## Dataset

- **Source:** [Kaggle Telco Customer Churn](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)
- **Original records:** 7,043 customers
- **Scaled to:** 500,000 records
- **Features:** 21 columns
- **Target:** Churn Yes/No
- **Churn rate:** 26.54%

## SQL Analysis Results

```sql
-- Churn Rate by Contract Type
SELECT Contract,
       COUNT(*) AS total,
       ROUND(100.0 * SUM(CASE WHEN Churn='Yes'
             THEN 1 ELSE 0 END) / COUNT(*), 1) AS churn_rate
FROM customers
GROUP BY Contract
ORDER BY churn_rate DESC;
```
Month-to-month → 42.7% churn
One year       → 11.3% churn
Two year       →  2.8% churn
Fiber optic    → 41.9% churn
DSL            → 19.0% churn
No service     →  7.4% churn
Churned avg monthly charges    → $74.44
Retained avg monthly charges   → $61.27
## Features Engineered

| Feature | Method | Business Meaning |
|---------|--------|-----------------|
| charges_per_tenure | MonthlyCharges / (tenure+1) | Cost per loyalty month |
| service_count | Count Yes across 8 services | Engagement score |
| contract_risk | Month-to-month=2, One=1, Two=0 | Contract risk level |
| senior_no_support | SeniorCitizen AND no TechSupport | Vulnerable flag |
| high_spender | Above median MonthlyCharges | High value flag |

## Model Details

### Logistic Regression — Best Model
```python
lr = LogisticRegression(
    max_iter     = 1000,
    random_state = 42,
    class_weight = 'balanced'
)
# ROC-AUC: 0.8462
```

### Gradient Boosting
```python
gb = GradientBoostingClassifier(
    n_estimators = 200,
    max_depth    = 4,
    learning_rate= 0.1,
    random_state = 42
)
# ROC-AUC: 0.8311
```

## Customer Segmentation — K-Means K=4
```
Segment 0 → Low tenure, high charges   → Highest churn risk
Segment 1 → High tenure, low charges   → Lowest churn risk
Segment 2 → Medium tenure, medium spend → Medium churn risk
Segment 3 → New high-spend customers   → Very high churn risk
```
## SHAP Explainability

Top churn drivers identified:
contract_risk       — contract type is #1 driver
charges_per_tenure  — cost vs loyalty ratio
MonthlyCharges      — high bills drive churn
TotalCharges        — lifetime value signal
tenure              — longer stay = lower risk

## Power BI Dashboard

| Page | Description |
|------|-------------|
| Executive Summary | 500K customers, churn by region and product, $157M at risk |
| Risk Segments | 169K high risk customers by segment and region |
| Model Performance | Confusion matrix, Precision, Recall, F1, Recall gauge |
| Retention Strategy | Revenue at risk, top churn reasons, customer table |

## Project Structure
```
churn-prediction/
├── notebooks/
│   └── Churn Prediction.ipynb     # Complete ML pipeline
├── output/
│   ├── churn_distribution.png
│   ├── elbow_method.png
│   ├── model_evaluation.png
│   └── shap_summary.png
├── screenshots/
│   ├── 01_churn_executive_summary.png
│   ├── 02_churn_risk_segments.png
│   ├── 03_churn_model_performance.png
│   └── 04_churn_retention_strategy.png
├── requirements.txt
├── .gitignore
└── README.md
```
## Setup and Installation

```bash
# Clone the repository
git clone https://github.com/navyayalavarthi/churn-prediction.git
cd churn-prediction

# Create virtual environment
python -m venv venv
source venv/bin/activate
# Windows: venv\\Scripts\\activate

# Install dependencies
pip install -r requirements.txt

# Download dataset
# https://www.kaggle.com/datasets/blastchar/telco-customer-churn
# Place CSV in data/ folder

# Run notebook
jupyter notebook "notebooks/Churn Prediction.ipynb"
```

## Skills Used

Python · Scikit-learn · XGBoost · SHAP · SQL · SQLite ·
K-Means · Logistic Regression · Gradient Boosting ·
Power BI · Excel · Jupyter · Pandas · NumPy

## Author

Navya
📧 navya.yalavarthi1@gmail.com
🔗 LinkedIn: https://www.linkedin.com/in/navya-yalavarthi-b21297289/

## License

MIT License — for educational and portfolio purposes.
