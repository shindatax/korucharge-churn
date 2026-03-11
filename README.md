# EV Charging Network Customer Churn Prediction

> EV charging network churn prediction using XGBoost, SHAP interpretation, and cost-aware model selection (F-beta2).

Machine learning project predicting customer churn for a fictional New Zealand EV charging network (KoruCharge), developed as a portfolio project extending an earlier university case study.

---

## Objective

Identify customers likely to churn and generate actionable, operationally grounded retention strategies — not just a predictive model.

---

## Key Results

| Metric | Value |
|---|---|
| Best model | XGBoost |
| F-beta2 (primary) | 0.870 |
| Recall | 0.894 |
| AUC-ROC | 0.966 |
| AUC-PR | 0.932 |

Churn is strongly driven by:

- Charging inactivity (`weeks_since_last_charge` — top predictor by a large margin)
- Low satisfaction scores and survey non-response
- Payment failures in the past 4 weeks
- Compounded service friction (high wait time + charger faults together)

---

## Example Output

![Model comparison and ROC/PR curves](images/example_output.png)

---

## What This Project Does

| Area | Detail |
|---|---|
| Feature set | Full synthetic customer dataset (24 variables) + 3 engineered features |
| Engineered features | `fault_wait_index`, `crash_per_session`, `recency_bucket` |
| Models compared | Logistic Regression, XGBoost, LightGBM |
| Selection criterion | F-beta (β = 2) — recall-weighted, cost-aware |
| Interpretability | Variable importance + SHAP values + Logistic Regression odds ratios |
| Evaluation | ROC curve + Precision-Recall curve (more informative under class imbalance) |

### Why F-beta (β = 2) instead of AUC?

Missing a churning customer (lost lifetime value) costs more than unnecessarily contacting a retained one (small intervention cost).  
F-beta with β = 2 weights Recall twice as heavily as Precision, reflecting this asymmetry directly.

AUC is threshold-independent and useful for model comparison, but it is not tied as closely to the operational decision of who to intervene on.

---

## Recommended Actions

- **Re-engagement automation** — trigger outreach at 6 weeks of inactivity, before customers become fully inactive  
- **Billing rescue workflows** — automate recovery within 24 hours of payment failure, with grace-period options  
- **Service recovery prioritisation** — focus maintenance on stations where long waits and charger faults co-occur  

---

## Data

The original dataset (`korucharge_customers.csv`) is **not included** in this repository due to GitHub file size limits.

Instead, the repository provides:

- `korucharge_customers_metadata.csv` — variable descriptions and feature definitions used in the analysis

The dataset used in this project is **synthetic** and was created for academic purposes.

---

## Repository Structure

```
korucharge-churn/
├── korucharge_churn_analysis.qmd        # Full analysis source
├── korucharge_churn_analysis.html       # Rendered report
├── korucharge_customers_metadata.csv    # Variable descriptions
├── images/
│   └── example_output.png               # Preview image for README
└── README.md
```

---

## Tools & Techniques

- **Language:** R  
- **ML framework:** tidymodels (recipes, workflows, resampling)  
- **Models:** Logistic Regression, XGBoost, LightGBM  
- **Interpretability:** SHAP values (SHAPforxgboost), variable importance (vip), odds ratios  
- **Feature engineering:** interaction terms, ratio features, ordinal bucketing  
- **Validation:** stratified 5-fold cross-validation, Precision-Recall analysis  
- **Visualisation:** ggplot2, ggridges, patchwork  
- **Reporting:** Quarto (HTML, code-fold)

---

## Full Analysis

Render the `.qmd` file locally, or view the published HTML report here:

👉 https://shindatax.github.io/korucharge-churn/korucharge_churn_analysis.html

To enable GitHub Pages:

Repo Settings → Pages → Source: main branch → / (root)

---

## How to Run

```r
# 1. Install required packages (first run only)
# Set INSTALL_PACKAGES <- TRUE in the setup chunk

# 2. Place korucharge_customers.csv in your working directory

# 3. Render the report
quarto::quarto_render("korucharge_churn_analysis.qmd")
```

---

## About

Portfolio project based on a **BUSINFO 704 (Predictive Business Analytics)** case study at the **University of Auckland**.

All data used in this project is synthetic.

**Shinyeong Kim**
