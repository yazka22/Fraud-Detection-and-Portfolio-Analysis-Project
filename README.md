# Fraud Detection & Portfolio Analysis

**Data Science selection task for Worldline · End-to-end fraud detection pipeline + portfolio analysis**

---

> **Context:** This project was completed as a technical selection task for a 
> **Data Science internship at Worldline**, a leading European payment services 
> provider.

---

## TL;DR

Built an **end-to-end fraud detection pipeline** in Python on **163,710 transactions** to identify fraudulent activity in a heavily imbalanced dataset (~8% fraud). Compared four ML classifiers — **Random Forest** achieved the best accuracy of **93.7%**, outperforming Logistic Regression (91.5%), Decision Tree (91.9%), and Gradient Boosting (92.7%). Complemented with **R-based portfolio analysis** of transaction volumes, gross/net profit (MSC_LTM, NET_MSC_LTM), and refunds, plus **interactive Tableau dashboards** for stakeholder reporting.

<img width="607" height="557" alt="Screenshot 2026-10-02 145301" src="https://github.com/user-attachments/assets/edc86a87-b2c6-445e-b08e-37f56c98bbd4" />

*ROC curves for all four models across three classes (Class 0: Accepted, Class 1: Blocked, Class 2: Fraud). Random Forest shows the highest AUC (0.92–0.93). Source: own processing.*

---

## Problem

Payment service providers like Worldline face two related analytical challenges:

1. **Fraud detection** — identifying fraudulent transactions in a dataset dominated by legitimate activity. Only **~8% of transactions are fraudulent**, making this a classic imbalanced classification problem where **missing fraud (false negative) is far more costly than a false alarm**.
2. **Portfolio analysis** — understanding transaction volumes, gross vs. net profit (MSC_LTM, NET_MSC_LTM), refund patterns, and card scheme distribution (MC, Visa, Other_CC) to inform merchant portfolio strategy.

This project addresses both: an ML-based fraud detection pipeline and a statistical portfolio analysis in R, visualized through Tableau dashboards.

---

## Data

### Fraud Detection Dataset

| Aspect | Detail |
|---|---|
| **Size** | 163,710 transactions |
| **Features** | `Time`, `Country` (A–E), `Value`, `Currency` (AD, ML, SH), `Status` |
| **Target** | `Status` — Accepted No Fraud (91.4%), Fraud (8.0%), Blocked (0.6%) |
| **Class imbalance** | Yes — fraud is a minority class (~8%) |
| **Time and Value correlation** | ~0.0014 (essentially uncorrelated) |

### Portfolio Dataset

| Aspect | Detail |
|---|---|
| **Source** | `Portfolio_for_test1.csv` (anonymized) |
| **Features** | Transaction counts and amounts (LTM), gross/net profit (`MSC`, `MSC_LTM`, `NET_MSC_LTM`), refunds, card scheme, Online/POS channel |
| **Observations** | ~1,900 client-merchant records |

---

## Methodology

### Part 1 — Fraud Detection (Python)

**1. Exploratory Data Analysis**
- Distribution of transaction values (right-skewed, peak around 20–25).
- Country-wise transaction volume (countries A–E; D has the highest fraud rate, A the lowest).
- Currency status breakdown (AD, ML, SH) with stacked bar charts.
- Fraudulent transactions over time — identified periodic spikes.
- Fraud rates by country and currency (line charts).

**2. Preprocessing**
- One-Hot Encoding for `Currency` and `Country`.
- Train/test split (80/20, random state = 42).

**3. Modeling**
Compared four classifiers on the encoded dataset:
- Logistic Regression
- Random Forest
- Decision Tree
- Gradient Boosting

**4. Evaluation**
- Accuracy score for each model.
- Multiclass ROC curves with AUC per class (Class 0: Accepted, Class 1: Blocked, Class 2: Fraud).

### Part 2 — Portfolio Analysis (R)

**1. Customer activity analysis**
- Mean and median transaction amounts LTM.
- Segmentation by channel (ONLINE vs. POS).

**2. Financial performance**
- Descriptive statistics for gross profit (`MSC_LTM`), net profit (`NET_MSC_LTM`), refunds.
- Histograms and boxplots.

**3. Card type analysis**
- Distribution of MC, Visa, Other_CC.
- Comparison of transaction amounts by scheme.

**4. Visualization** — `ggplot2` charts, histograms, bar charts, pie charts, boxplots.

### Part 3 — BI Dashboard (Tableau)

- **Net profit by card type** (MC, Visa, Other_CC) broken down by client.
- **Gross vs. net profit** comparison across schemes.
- **Treemap of transaction values** by Online/POS, Client, and Scheme — total `TRX_AMOUNT_LTM` = 101,789,713.
- **Bubble chart** of POS transaction volumes by scheme.

---

## Results

### Fraud Detection — Model Comparison

| Model | Accuracy |
|---|---|
| Logistic Regression | 0.9148 |
| Decision Tree | 0.9186 |
| Gradient Boosting | 0.9274 |
| **Random Forest** | **0.9366** ✅ |

**ROC-AUC by class (Random Forest):**
- Class 0 (Accepted): **0.92**
- Class 1 (Blocked): **0.81**
- Class 2 (Fraud): **0.93**

**Key findings:**
- **Country D** has the highest fraud rate (**14.9%**), while **Country A** has the lowest (**0.4%**) — a 34× difference.
- **Currency ML** has the highest fraud rate (**8.8%**); **Currency AD** the lowest (**7.6%**).
- Average fraudulent transaction value ($20.28) is slightly **lower** than non-fraudulent ($22.27) — so high-value transactions are not necessarily riskier.
- Transaction `Time` and `Value` are essentially uncorrelated (r = 0.0014).

### Portfolio Analysis

| Metric | Value |
|---|---|
| Mean transaction amount LTM | 19,798.87 |
| Median transaction amount LTM | 5,075.13 |
| Mean gross profit (MSC_LTM) | 79.71 |
| Median gross profit (MSC_LTM) | 20.76 |
| Total net MSC | 23,330.01 |
| Total refunds | -25,387.59 |
| Mean refund | -13.31 |

**Card scheme distribution:**
- MC: 34.5%
- Visa: 33.8%
- Other_CC: 31.7%

Nearly balanced across the three schemes — no single dominant card type.

---

## Tech Stack

- **Languages:** Python, R
- **Python:** `pandas`, `NumPy`, `scikit-learn`, `Matplotlib`, `Seaborn`
- **R:** `tidyverse`, `ggplot2`
- **BI:** Tableau Desktop
- **Environment:** Jupyter Notebook, RStudio

---

## Repository Contents
