# 🏥 Healthcare Utilisation Analytics
### Understanding Doctor Visit Patterns — Australian Health Survey

![Python](https://img.shields.io/badge/Python-3.13-blue?logo=python)
![pandas](https://img.shields.io/badge/pandas-2.x-150458?logo=pandas)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

---

## Overview

An **end-to-end healthcare utilisation analysis** of the Australian Health Survey dataset
(Cameron & Trivedi, 1998). The project covers the full Data Analyst workflow — from raw data
through quality audit, cleaning, EDA, statistical testing, and actionable recommendations.

**Business questions answered:**
1. Who visits doctors most frequently and why?
2. Do chronic conditions increase healthcare utilisation?
3. Does insurance type affect visit rates — and are there equity gaps?
4. Which demographic and health factors are most strongly associated with doctor visits?

---

## Dataset

| Property | Value |
|----------|-------|
| Source | Cameron & Trivedi (1998), *Regression Analysis of Count Data* |
| Raw rows | 5,190 |
| Clean rows | 3,870 (after removing 1,320 duplicate rows = 25.4%) |
| Columns | 12 original + 7 engineered features |
| Time period | 1977–78 Australian Health Survey |
| Target variable | `visits` — number of GP visits in a 2-week recall window |

---

## Key Findings

| # | Finding | Evidence |
|---|---------|----------|
| 1 | **74.4% of respondents had zero doctor visits** in the recall window | Zero-inflated count; skewness = 4.16 |
| 2 | **Females visit 42% more than males** | 29.6% vs 20.9% visit rate; Mann-Whitney p<0.001 |
| 3 | **Chronic conditions show a dose-response** | None: 20.2% → Non-Limiting: 27.0% → Limiting: 36.6%; Kruskal-Wallis H=69.5, p<0.001 |
| 4 | **Days of reduced activity is the strongest predictor** | Spearman ρ = +0.315, p<0.001 |
| 5 | **Age gradient is significant** | Older Adults (55–72): 34.4% vs Young Adults (19–34): 20.9%; H=78.5, p<0.001 |
| 6 | **Equity anomaly** — Govt (Low-Income) group has a 10.3% visit rate — *lower than the uninsured (19.3%)* | Possible access barriers despite formal coverage; n=194 |
| 7 | **Income has a weak negative association** | Spearman ρ = −0.083, p<0.001 |

---

## Project Structure

```
Healthcare/
├── Healthcare_Analytics_Enhanced.ipynb   ← Main analysis notebook (8 stages)
├── DoctorVisits - DA.csv                 ← Raw dataset
├── fig_visits_distribution.png           ← Target variable chart
├── fig_visits_by_gender.png
├── fig_visits_by_chronic.png
├── fig_visits_by_insurance.png
├── fig_visits_by_age.png
├── fig_visits_by_illness.png
├── fig_correlation_heatmap.png
├── fig_reduced_income_dist.png
└── healthcare_dashboard.png              ← 6-panel summary dashboard
```

---

## Notebook Stages

| Stage | Contents |
|-------|----------|
| **1 — Data Understanding** | Dataset context, full data dictionary, business questions |
| **2 — Data Quality Audit** | 11-point quality scorecard; nulls, duplicates, range checks, business rule validation |
| **3 — Cleaning & Feature Engineering** | Deduplication, column decoding, 7 engineered features, post-cleaning assertions |
| **4 — EDA (9 cells)** | Univariate distributions, bivariate comparisons, heatmap, predictors profiled |
| **5 — Statistical Analysis** | Mann-Whitney U, Kruskal-Wallis, Spearman ρ, Chi-Square — all with interpretation |
| **6 — Storytelling** | Executive summary, high/low utiliser profiles, 5 evidence-based recommendations |
| **7 — Dashboard** | 6-panel publication-quality summary figure |
| **8 — Interview Prep** | 15 common DA interview questions answered with project-specific responses |

---

## Methods & Statistical Choices

The target variable (`visits`) is **zero-inflated** (74.4% zeros) and **right-skewed** (skewness = 4.16).
Standard parametric tests assume normality and would be invalid here.

| Task | Method chosen | Why |
|------|---------------|-----|
| 2-group comparison | Mann-Whitney U | Non-normal data, no normality assumption |
| 3+ group comparison | Kruskal-Wallis | Non-parametric one-way ANOVA equivalent |
| Numeric association | Spearman ρ | Rank-based; handles skewed count data |
| Categorical × Categorical | Chi-Square | Independence test for two categorical variables |

---

## Feature Engineering

| New Column | Source | Logic |
|------------|--------|-------|
| `age_years` | `age` | `age × 100` → human-readable years (19–72) |
| `income_aud` | `income` | `income × 10,000` → AUD bracket midpoints |
| `age_group` | `age_years` | pd.cut into 3 clinical brackets |
| `insurance_type` | `private`, `freepoor`, `freerepat` | Priority merge of 3 mutually exclusive binary columns |
| `chronic_status` | `nchronic`, `lchronic` | Ordered: None < Non-Limiting < Limiting |
| `any_visit` | `visits` | Binary flag: visited at least once |
| `income_zero_flag` | `income` | Flags 71 zero-income rows for monitoring |

---

## Limitations

- **Cross-sectional design** — no causal conclusions can be drawn
- **2-week recall window** — may not represent annual utilisation patterns
- **Historical data** (1977–78 Australia) — findings may not generalise to modern healthcare systems
- **25.4% duplicate rows** removed before analysis — cause of duplication is unknown
- **Confounding** present — e.g., the Veterans/Elderly group is both older and has higher chronic disease burden, making it impossible to isolate the insurance effect using bivariate analysis alone

---

## Tech Stack

- **Python 3.13** — pandas, numpy, matplotlib, seaborn, scipy
- **Jupyter Notebook**
- Statistical tests: `scipy.stats` (mannwhitneyu, kruskal, spearmanr, chi2_contingency)

---

## How to Run

```bash
# Clone or download the repository
# Install dependencies
pip install pandas numpy matplotlib seaborn scipy jupyter

# Launch notebook
jupyter notebook Healthcare_Analytics_Enhanced.ipynb
```

---

## Author

**Tripti** | Entry-Level Data Analyst  
Portfolio project demonstrating end-to-end data analysis skills on a real-world healthcare dataset.
