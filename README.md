# 📊 Customer Churn Analysis & Customer Intelligence

An end-to-end churn analysis project built with **Python (Pandas, NumPy, Matplotlib, Seaborn)** and **SQLite**. The project takes raw, messy relational data from a SQLite database, cleans and merges it, engineers churn-related features, and surfaces key business KPIs and visual insights to understand *why* customers are leaving and *who* is most at risk.

## 🎯 Project Overview

The dataset simulates a subscription business with three related tables:
- **Customer** — demographic details (name, country, state, gender, date of birth)
- **Subscription** — plan details, billing, contract type, cancellations, CLTV, and churn score
- **Support** — complaint history, escalations, and CSAT scores

The goal is to combine these sources into a single analytical dataset and answer core retention questions: How many customers are churning? Which plans, contracts, and regions are most affected? Does support experience (complaints/escalations) predict churn?

## 🛠️ Tech Stack

- **Python** — pandas, numpy
- **Visualization** — matplotlib, seaborn
- **Database** — SQLite3 (querying, table creation, aggregation via SQL)
- **Environment** — Jupyter Notebook

## 🔧 What the Notebook Does

### 1. Data Import
- Connects to `customer_churn.db` and dynamically loads every table into a corresponding DataFrame (`df_db_customer`, `df_db_subscription`, `df_db_support`) using `sqlite_master`.

### 2. Data Cleaning
- Renamed inconsistent column names for clarity.
- Dropped mostly-empty / irrelevant columns (`interests`, `pincode`, `col_1`, `comment`).
- Converted string date columns to proper `datetime` types across all three tables.
- Standardized inconsistent category labels (e.g., `Men` → `Male`).
- Imputed missing `country` values using a `state → country` mapping derived from non-null records.
- Identified and removed duplicate support records before merging, using a derived `complaint_count`.

### 3. Feature Engineering
- **`churn_flag`** — binary flag derived from whether `cancellation_date` is populated.
- **`age`** — calculated from date of birth.
- **`tenure_days`** — days active, calculated differently for churned vs. active customers.
- **`churn_risk`** — categorical bucket (`low` / `med` / `high`) derived from `churn_score` thresholds.
- Merged all three cleaned tables into a single master DataFrame and exported it to `exported_churn_data.csv`.

### 4. Key Metrics (KPIs)
| Metric | Value |
|---|---|
| Churn Rate | 28.57% |
| Retention Rate | 71.43% |
| Avg. Revenue Per User (ARPU) | ₹18.85 |
| Avg. Customer Tenure | ~1,537 days |
| Revenue at Risk | ₹73.94K |
| Escalation Rate | 19.05% |
| Avg. Complaints per User | 0.43 |
| Correlation: Escalations vs. Churn | 0.77 |

Additional breakdowns computed: churn rate by plan type, churn by state (with revenue & user counts), and churn by subscription/acquisition type.

### 5. Visualization
- **Matplotlib** — monthly churn trend (time series), churn rate by plan type, churn rate by state.
- **Seaborn** — correlation heatmap (plan type, contract type, churn score, churn flag, churn risk, escalations), pairplot for multivariate relationships, and a `catplot`/FacetGrid comparing monthly charges across plan type, gender, and churn risk.
- A manual Matplotlib heatmap is also included to demonstrate building one without Seaborn.

### 6. Pivot Tables
- Churn rate by plan type using `pd.pivot_table`.
- Multi-metric pivot table combining churn rate, unique customer count, and total revenue by plan type.

### 7. SQL in Python
- Demonstrates creating a new SQLite database and table from scratch.
- Inserting records via parameterized `INSERT` statements.
- Reading data back into pandas with `pd.read_sql`.
- Running an aggregate `GROUP BY` SQL query directly against SQLite.

## 📈 Key Insights

- **Basic plan users churn the most** (60%) compared to Premium (14.3%) and Standard (22.2%) — suggesting entry-level plan users are the most flight-risk segment.
- **Escalations strongly correlate with churn** (r = 0.77) — customers who escalate a complaint are far more likely to cancel.
- **Churn score is a strong leading indicator**, correlating at 0.86–0.93 with actual churn outcome and risk bucket, validating it as a useful early-warning signal.
- Referral-acquired customers account for the largest share of churned revenue, warranting a closer look at onboarding/retention for that acquisition channel.

## 📂 Files

| File | Description |
|---|---|
| `churn_analysis.ipynb` | Full analysis notebook (cleaning → features → EDA → SQL) |
| `customer_churn.db` | Source SQLite database (customer, subscription, support tables) |
| `exported_churn_data.csv` | Final merged & cleaned dataset used for analysis |

## 🚀 How to Run

```bash
pip install pandas numpy matplotlib seaborn
jupyter notebook churn_analysis.ipynb
```

## 🔮 Possible Next Steps

- Build a predictive churn model (logistic regression / XGBoost) using `churn_score`, `escalations`, `plan_type`, and `tenure_days` as features.
- Automate the pipeline into a scheduled ETL job feeding a live dashboard (e.g., Power BI / Tableau).
- Segment customers by CLTV vs. churn risk to prioritize retention spend.

---
*Built as a portfolio project to demonstrate data cleaning, feature engineering, SQL, and visualization skills using Python.*
