# 🏥 Predicting NHS RTT Breach Risk Using Open National Data

> **🏆 Best Poster Award — Top 5, MSc Data Science, Middlesex University London (January 2026)**

An interpretable, open-data machine learning system that predicts which NHS provider–specialty services are likely to breach the 18-week Referral-to-Treatment (RTT) waiting time standard in the following month.

---

## 📌 Problem Statement

During 2023–24, long-wait RTT breaches remained widespread across NHS England, but risk was unevenly distributed across providers and specialties. National reporting describes overall pressure, yet local teams still need short-term signals for prioritisation and escalation.

**Goal:** Build an interpretable early-warning model using open data only, that flags provider–specialty services likely to breach next month — enabling proactive rather than reactive management.

---

## 🎯 Results at a Glance

| Metric | Score |
|--------|-------|
| Model | Hybrid XGBoost + threshold optimisation |
| ROC-AUC | **0.74** |
| Recall | **0.93** |
| F1 Score | **0.82** |
| Accuracy | 0.73 |
| Operating threshold | ~0.36 (recall-optimised) |

> High recall was deliberately prioritised — missing a deteriorating service is more costly than a false alarm.

---

## 📊 Data Sources (Open National Data)

| Dataset | Source | Description |
|---------|--------|-------------|
| RTT Incomplete Pathways | NHS England | Week-band distributions → long-wait tail |
| HES APC Provider-level | NHS Digital | Elective/emergency activity + LOS/wait proxies |

**Period covered:** 2023–24  
**Granularity:** Provider × Specialty × Month panel

---

## ⚙️ Methodology

### Pipeline overview
```
RTT monthly CSVs
    ↓ Filter to Incomplete Pathways → build Over18Weeks, PropOver18
HES provider-level APC
    ↓ Emergency/elective activity + LOS/wait proxies
Merge on provider code
    ↓ Create provider–specialty–month panel
Feature engineering
    ↓ Lagged breach + list size + seasonality + pressure proxies
Models
    ↓ Classification: Baseline → Global XGBoost → Hybrid + threshold optimisation
    ↓ Regression: Ridge → Random Forest → XGBoost
Explainability
    ↓ SHAP global + local reasons for flags
```

### Two complementary tasks
- **Classification** — *Will this service breach next month?* Supports escalation triage.
- **Regression** — *How severe will the breach be?* Supports severity ranking once flagged.

### Targets
- `PropOver18`: proportion of incomplete RTT pathways waiting >18 weeks
- `Exceeded18`: binary flag if PropOver18 > 10% (screening threshold)

---

## 🔍 Explainability (SHAP)

Top drivers identified by SHAP:

1. **Lagged breach proportion** — breach states tend to persist
2. **Waiting list size** — scale drives risk
3. **Emergency pressure proxies** — context from HES APC
4. **Seasonality** — winter and holiday effects

SHAP makes every alert defensible:
> *"Flagged because last month's long-wait tail stayed high, the list is large, and winter pressure increases risk."*

---

## 📁 Repository Structure

```
├── Codefile(M01037912).ipynb   # Full analysis notebook
├── NHS_RTT_Project Data/       # Input datasets
├── Figures/                    # All charts and SHAP plots
├── Dissertation Report.pdf     # Full written report
└── project e-poster.pdf        # Award-winning project poster
```

---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3.10-blue)
![XGBoost](https://img.shields.io/badge/XGBoost-gradient%20boosting-orange)
![SHAP](https://img.shields.io/badge/SHAP-explainability-green)
![Pandas](https://img.shields.io/badge/Pandas-data%20wrangling-lightblue)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-ML-red)
![Jupyter](https://img.shields.io/badge/Jupyter-notebook-yellow)

- **Language:** Python 3.10
- **ML:** XGBoost, Scikit-learn, Random Forest, Ridge Regression
- **Explainability:** SHAP
- **Data:** Pandas, NumPy
- **Visualisation:** Matplotlib, Seaborn
- **Environment:** Jupyter Notebook

---

## 🚀 How to Run

```bash
# Clone the repository
git clone https://github.com/saadahsan0009/Predicting-NHS-RTT-Breach-Risk-Using-Open-National-Data.git
cd Predicting-NHS-RTT-Breach-Risk-Using-Open-National-Data

# Install dependencies
pip install pandas numpy scikit-learn xgboost shap matplotlib seaborn jupyter

# Launch notebook
jupyter notebook "Codefile(M01037912).ipynb"
```

---

## ⚠️ Limitations & Blind Spots

This model uses open national data only. The following operational factors are not captured:
- Staffing levels and sickness absence
- Theatre cancellations
- Bed state and capacity
- Individual patient-level data

These blind spots mean the model is best used for **ranking and triage**, not as an automated decision rule. A higher risk score means the service *resembles past provider–specialty–months that tended to remain in breach* — not that breach is certain.

---

## 👤 Author

**Saad Ahsan**
MSc Data Science (Distinction) — Middlesex University London
MSc Electronic Engineering (Distinction) — University of Essex

📧 saadahsan0009@gmail.com
🔗 [LinkedIn](https://www.linkedin.com/in/saad-ahsan-1901sam/)
🐙 [GitHub](https://github.com/saadahsan0009)

---

## 📄 License

This project uses open NHS national data. Code is available for educational and research purposes.
