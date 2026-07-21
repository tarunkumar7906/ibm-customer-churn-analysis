# 📉 IBM Telecom Customer Churn Analysis — Python + SQL

**Tools used:** Python | Pandas | SQLite3 | Matplotlib | Seaborn  
**Status:** Completed

---

## 📌 Short Description

This project performs an end-to-end customer churn analysis on the IBM Telco dataset (7,043 customers | 33 attributes) using Python and in-memory SQL via SQLite3. I cleaned the raw Excel data, loaded it into a SQL database inside Python, and ran structured queries combined with Seaborn visualizations across 6 analytical areas — financial risk, customer behavior, service usage, churn reasons, geographic hotspots, and active customer risk segmentation. The goal was not just to find who churned — but to understand why, how much it cost, and what to do about it.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| 🐍 Python | Core language for the entire analysis |
| 🐼 Pandas | Data loading, cleaning, and transformation |
| 🗄️ SQLite3 | In-memory SQL — ran structured queries directly inside Python without an external database |
| 📊 Matplotlib | Base charting layer for all visualizations |
| 🎨 Seaborn | Bar charts, KDE plot, heatmap, scatter plot, and subplot grids |
| 📓 Jupyter Notebook | Writing and running the full analysis interactively |
| 📁 File Format | `.ipynb` notebook, `.xlsx` raw data, `.csv` cleaned export |

---

## 📁 Data Source

**Source:** IBM Telco Customer Churn Dataset (Kaggle)

**Size:** 7,043 customers | 33 attributes

Key columns used in the analysis:

| Column | Description |
|---|---|
| `CustomerID` | Unique customer identifier |
| `Churn Value` | 1 = Churned, 0 = Retained |
| `Churn Score` | Predicted churn risk score (0–100) |
| `Churn Reason` | Actual reason given by the customer |
| `CLTV` | Customer Lifetime Value |
| `Tenure Months` | How long the customer has been subscribed |
| `Monthly Charges` | Monthly bill amount ($) |
| `Total Charges` | Cumulative amount paid ($) |
| `Contract` | Month-to-Month, One Year, Two Year |
| `Payment Method` | Electronic Check, Bank Transfer, Credit Card, Mailed Check |
| `Internet Service` | DSL, Fiber Optic, None |
| `Senior Citizen`, `Partner`, `Dependents` | Demographic flags |
| `Online Security`, `Tech Support` | Add-on service flags |
| `City` | Customer location — used for geographic analysis |

**Cleaning steps performed:**
- Converted `Total Charges` from string to numeric using `pd.to_numeric(errors='coerce')`
- Found 11 null values — investigated and confirmed they were new subscribers with 0 tenure months, filled with 0
- Verified 0 duplicate rows
- Ran crosstab checks to confirm logical consistency across internet service and add-on columns
- Dropped irrelevant columns: `Country`, `State`, `Count`, `Lat Long`
- Exported cleaned data to `IBM_churn.csv` and loaded into SQLite3 for querying

---

## 🔍 Features / Highlights

### 🔴 Business Problem

A telecom company has a 26.54% churn rate — more than 1 in 4 customers is leaving. But the business does not know:

- Which contract types and payment methods are losing the most revenue?
- At what point in the customer lifecycle is churn highest?
- Which services are keeping customers — and which are not helping?
- Which cities are churn hotspots?
- Which currently active customers are still at risk — and how much monthly revenue do they represent?
- Why exactly do new customers leave in their first 6 months?

---

### 🎯 Goal of the Analysis

To combine Python EDA with in-memory SQL queries to go deeper than basic charts — segmenting customers by tenure, risk score, geography, payment method, and service usage — and build a clear, numbers-backed picture of churn risk with actionable recommendations.

---

### 📊 Walkthrough of Key Visuals

**Executive KPIs**

| Metric | Value |
|---|---|
| Total Customers | 7,043 |
| Churned Customers | 1,869 |
| Churn Rate | 26.54% |
| Retention Rate | 73.46% |
| Total Revenue | $16,056,168 |
| Revenue Lost to Churn | $2,862,926 |
| Avg Monthly Charge | $64.76 |
| Avg Total Charge | $2,279.73 |

---

**1. Revenue Lost by Contract Type (Bar Chart)**

| Contract | Revenue Lost |
|---|---|
| Month-to-Month | ~12% of total revenue |
| One Year | ~4% of total revenue |
| Two Year | ~1% of total revenue |

Month-to-Month customers account for the majority of revenue lost. Two-Year contract holders barely churn at all.

---

**2. Revenue Lost by Payment Method (Bar Chart)**

| Payment Method | Revenue Lost |
|---|---|
| Electronic Check | ~9.76% of total revenue |
| Bank Transfer (Automatic) | ~3.65% of total revenue |
| Credit Card (Automatic) | ~3.40% of total revenue |
| Mailed Check | ~1.02% of total revenue |

Electronic Check customers are the highest churn risk by payment method. Automatic payment users retain far better.

---

**3. CLTV — Retained vs Churned (Bar Chart)**

Retained customers have a higher average CLTV than churned ones — meaning the company is losing lower-CLTV customers disproportionately, but still losing meaningful revenue overall.

---

**4. Churn Rate by Tenure Cohort (SQL + Bar Chart)**

| Tenure Group | Churn Rate |
|---|---|
| 0–6 Months | ~52% ← Most Critical |
| 6–12 Months | ~35% |
| 1–2 Years | ~28% |
| More than 2 Years | ~14% |

Over half of new customers churn within the first 6 months. Churn rate nearly halves every time tenure doubles.

---

**5. Churn Rate by Internet Service (SQL + Bar Chart)**

| Service Type | Churn Rate |
|---|---|
| Fiber Optic | 41.89% |
| DSL | 18.96% |
| No Internet | 7.40% |

Fiber Optic customers churn at more than double the rate of DSL customers — despite paying a premium price.

---

**6. Churn Rate by Customer Profile — 12 Subplot Grid (SQL Loop + Bar Charts)**

Ran a dynamic SQL query in a Python loop across 12 binary columns and plotted each as a labeled bar chart:

- Senior Citizens churn more than non-seniors
- Customers without a Partner or Dependents churn more
- Customers without Online Security churn significantly more
- Customers without Tech Support churn significantly more
- Paperless Billing customers churn more than non-paperless users
- Phone Service, Streaming TV, Streaming Movies show moderate churn variation

---

**7. Top 10 Churn Reasons (SQL + Horizontal Bar Chart)**

The actual reasons customers gave for leaving — ranked by count. Competitor-related reasons dominate.

---

**8. Monthly Charges Distribution — KDE Plot (Seaborn)**

Customers paying between $80–$100/month have the highest churn density. Retained customers are spread more evenly across lower charge bands. High bills are a direct churn driver.

---

**9. Correlation Heatmap (Seaborn)**

Key numerical correlations — Tenure Months, Monthly Charges, Total Charges, CLTV, Churn Score, Churn Value. Shows which features are most statistically linked to churn.

---

**10. Customer Risk Zone — Scatter Plot (Seaborn)**

Tenure vs Monthly Charges colored by Churn Label (Red = Churned, Blue = Retained). The high-risk cluster is clearly visible — short tenure + high monthly charges = the most vulnerable customers.

---

**11. Active Customer Risk Segmentation (SQL)**

| Risk Segment | Active Customers | Avg Monthly Charge | Revenue at Risk |
|---|---|---|---|
| High Risk (≥75) | 521 | $61.02 | $31,793.70/month |
| Medium Risk (50–74) | 2,120 | $61.70 | $1,30,794.20/month |
| Low Risk (<50) | 2,533 | $60.95 | $1,54,397.85/month |

521 currently active customers are at high churn risk — representing $31,793 in monthly revenue that could walk out the door.

---

**12. Top 10 Geographic Churn Hotspots (SQL + Horizontal Bar Chart)**

| City | Churn Rate | Churned Customers |
|---|---|---|
| San Diego | 33.33% | 50 |
| Glendale | 32.50% | 13 |
| Pasadena | 30.00% | 9 |
| San Francisco | 29.81% | 31 |
| Los Angeles | 29.51% | 90 |

Only cities with at least 30 customers were included for statistical reliability. Each bar shows churn %, count, and total revenue lost.

---

**13. Why Customers Leave in Their First 6 Months (SQL + Bar Chart)**

| Churn Reason | Count | % of Early Churn |
|---|---|---|
| Attitude of support person | 80 | 10.20% |
| Don't know | 72 | 9.18% |
| Competitor offered higher download speeds | 72 | 9.18% |
| Competitor had better devices | 64 | ~8% |
| Competitor made better offer | 63 | ~8% |

Poor customer support attitude is the single biggest reason new customers leave in their first 6 months.

---

### 💡 Business Insights

- **26.54% churn rate is a serious problem** — industry benchmark for telecom is 15–20%. This company is above it.

- **$2.86M in revenue already lost** — not a future risk, a current reality. Month-to-Month contracts and Electronic Check users account for the largest share.

- **The first 6 months are the most dangerous** — 52% churn rate in the 0–6 month cohort. New customers are leaving before they even settle in.

- **Poor customer support is the #1 early churn driver** — "Attitude of support person" is the top reason customers leave in their first 6 months. This is entirely fixable.

- **Fiber Optic has a quality or value problem** — customers pay more but churn at 41.89%. Nearly 6x the rate of customers with no internet service.

- **Online Security and Tech Support are retention tools** — customers without them churn significantly more. These add-ons are not just revenue — they are loyalty anchors.

- **521 high-risk active customers represent $31,793/month** — these are still retained but at serious risk. Proactive outreach before they leave is far cheaper than acquiring new ones.

- **Automatic payment = better retention** — Bank Transfer and Credit Card (auto) customers churn far less than Electronic Check users. The payment method itself signals commitment.

- **Los Angeles has the highest absolute churn volume** — 90 churned customers. San Diego has the highest churn rate at 33.33%.

---

### 📌 Recommendations

1. **Fix the first 90 days** — implement structured onboarding with 30/60/90-day check-in touchpoints. Train support staff specifically for new customer interactions. "Attitude of support person" being the top churn reason is a solvable problem.

2. **Improve Fiber Optic quality and value** — 41.89% churn rate means customers are not getting what they pay for. Audit speeds, reliability, and support quality for this segment specifically. Consider bundling Tech Support and Online Security into Fiber plans.

3. **Incentivize annual contracts and auto-pay** — offer a $5–$10 monthly discount for switching from Month-to-Month to a 1 or 2-year plan. Offer a similar discount for switching from Electronic Check to automatic payment.

4. **Bundle Online Security and Tech Support into base plans** — customers without them churn significantly more. Removing the add-on decision reduces friction and improves retention.

5. **Act on the 521 high-risk active customers immediately** — they represent $31,793/month in revenue at risk. A targeted outreach campaign for this segment has a clear and measurable ROI.

6. **Focus regional retention efforts on San Diego and Los Angeles** — highest churn rate and highest churn volume respectively. Region-specific interventions — better local support, competitive pricing, or network upgrades — are warranted.

## 📂 Project Structure

```
ibm-customer-churn-analysis/
├── IBM_Customer_churn_analysis.ipynb   ← Full analysis notebook
├── Telco_customer_churn.xlsx           ← Raw dataset
├── IBM_churn.csv                       ← Cleaned dataset (exported mid-notebook)
└── README.md
```

---

## 💡 What I Learned

- Using **SQLite3 inside Python** to run SQL on a Pandas DataFrame — no external database needed
- Writing **dynamic SQL queries inside a Python loop** to analyze 12 binary columns without repeating code
- Using **`CASE WHEN`** in SQL to create tenure cohorts and risk segments directly in the query
- Using **`HAVING`** to filter cities with at least 30 customers for statistical reliability
- Investigating *why* nulls exist instead of just dropping them — found they were new subscribers with 0 tenure
- Using **`pd.crosstab`** to verify logical consistency across related columns before analysis
- Building a **KDE plot** in Seaborn to compare charge distributions between churned and retained customers
- Combining SQL output DataFrames directly with Seaborn in one clean workflow — query → DataFrame → visualize

---

## 🙋 Connect with Me

This is part of my ongoing data analytics learning journey. If you have feedback on the SQL queries, the analysis structure, or the visualizations — I would genuinely like to hear it.

- 🔗 LinkedIn: [linkedin.com/in/tarun-kumar-5b9280396](https://linkedin.com/in/tarun-kumar-5b9280396)
- 💻 GitHub: [github.com/tarunkumar7906](https://github.com/tarunkumar7906)

---

*Still learning. Open to feedback.*
