# Customer Shopping Behavior Analysis

**End-to-end data analytics project using Python, SQL Server and Power BI**

This project analyzes 3,900 retail customer transactions to understand what drives revenue, which customers are most valuable, and whether the company's subscription and discount programs actually work. It follows a real analyst workflow: load and explore the data in Python, clean it, answer business questions with SQL, and present the results in an interactive Power BI dashboard and a written business report.

---

## Table of Contents

1. [Business Problem](#business-problem)
2. [Tools & Skills](#tools--skills)
3. [Dataset](#dataset)
4. [Project Workflow](#project-workflow)
5. [Dashboard](#dashboard)
6. [Key Insights](#key-insights)
7. [Recommendations](#recommendations)
8. [Project Structure](#project-structure)
9. [How to Run](#how-to-run)
10. [Limitations](#limitations)

---

## Business Problem

The business wants to answer four questions:

- Where does revenue come from (products, categories, seasons, regions)?
- Which customer groups are the most valuable?
- Are the subscription and discount programs increasing customer spend?
- What actions should management prioritize to grow revenue?

---

## Tools & Skills

| Tool | Used for |
|---|---|
| **Python** (pandas, Jupyter) | Loading data, exploratory data analysis (EDA), data cleaning, feature engineering |
| **SQL Server** (T-SQL) | Answering 10 business questions with aggregations, CTEs, subqueries and window functions |
| **Power BI** (DAX) | 3-page interactive dashboard with KPI cards, slicers and measures |
| **Microsoft Word** | Business report with findings, statistical tests and recommendations |

**Skills demonstrated:** data cleaning, EDA, missing-value imputation, feature engineering, SQL (`GROUP BY`, `CASE`, CTEs, `ROW_NUMBER()`, `SUM() OVER()`), DAX measures, dashboard design, statistical testing, and business storytelling.

---

## Dataset

- **Source file:** `customer_shopping_behavior.csv`
- **Size:** 3,900 rows × 18 columns (one row per customer and their latest purchase)
- **Fields include:**
  - **Customer:** age, gender, location
  - **Purchase:** item, category, purchase amount (USD), size, color, season
  - **Behavior:** payment method, shipping type, purchase frequency, previous purchases
  - **Programs & feedback:** subscription status, discount applied, promo code used, review rating

---

## Project Workflow

### 1. Load & Explore the Data (Python)
- Loaded the CSV into a pandas DataFrame.
- Reviewed structure with `head()`, `info()` and filtered views (e.g., male customers sorted by purchase amount).
- Checked for missing values: **37 missing review ratings** were found.

### 2. Clean & Prepare the Data (Python)
| Step | What was done | Why |
|---|---|---|
| Handle missing values | Filled missing `Review Rating` with the **median rating of each product category** | Keeps ratings realistic for each category instead of using one overall value |
| Standardize column names | Converted to `snake_case` (e.g., `Purchase Amount (USD)` → `purchase_amount`) | Easier to query in SQL and use in Power BI |
| Create `age_group` | Split customers into 4 equal groups: Young Adult, Adult, Middle-aged, Senior (`pd.qcut`) | Enables age segmentation |
| Create `purchase_frequency_days` | Mapped text frequency (Weekly, Monthly, etc.) to number of days | Turns a text field into a usable number |
| Remove redundant column | Dropped `promo_code_used` after confirming it was **identical** to `discount_applied` | Avoids duplicate information |

**Output:** `customer_shopping_behavior_cleaned.csv` (3,900 rows × 19 columns, no missing values)

### 3. Analyze with SQL (SQL Server)
The cleaned data was loaded into SQL Server as the table `dbo.customer`, and 10 business questions were answered in `customer_analysis_queries.sql`:

| # | Business question |
|---|---|
| Q1 | Total revenue by gender |
| Q2 | Customers who used a discount but still spent above average |
| Q3 | Top 5 products by average review rating |
| Q4 | Average purchase amount: Standard vs. Express shipping |
| Q5 | Do subscribers spend more than non-subscribers? |
| Q6 | Top 5 products with the highest share of discounted purchases |
| Q7 | Customer segments: New, Returning, Loyal |
| Q8 | Top 3 most purchased products in each category |
| Q9 | Are repeat buyers more likely to subscribe? |
| Q10 | Revenue contribution by age group |

### 4. Build the Dashboard (Power BI)
An interactive 3-page dashboard, **Shopper Pulse** (`customer_analysis.pbix`), with six shared slicers (gender, age group, category, season, state, subscription):

| Page | What it shows |
|---|---|
| **Overview** | KPI cards, revenue by category, top 10 states, top 10 products |
| **Customers** | Revenue by gender and age group, customer segments, orders by size |
| **Behavior & Channels** | Payment methods, shipping types, subscription and discount comparison, purchase frequency |

### 5. Report Findings (Word)
A management-style report (`Customer_Shopping_Behavior_Report.docx`) summarizes the results, supported by statistical tests (t-tests, ANOVA, chi-square, correlation) and a prioritized action plan.

---

## Dashboard

> Add a screenshot of each dashboard page here.

![Dashboard Overview](images/dashboard_overview.png)

**Headline KPIs**

| Total Revenue | Transactions | Avg Order Value | Avg Rating |
|:---:|:---:|:---:|:---:|
| **$233,081** | **3,900** | **$59.76** | **3.75 / 5** |

---

## Key Insights

1. **Discounts are not increasing spend.** Discounted orders average **$59.28** vs. **$60.13** without a discount. The difference is not statistically significant (p = 0.27).
2. **Programs exclude women entirely.** All 1,053 subscribers and all discounted orders belong to male customers, even though women spend the same per order ($60.25 vs. $59.54).
3. **Purchase frequency drives value, not basket size.** Weekly, bi-weekly and fortnightly buyers are **42% of customers** but an estimated **83% of annual spend**.
4. **Revenue is concentrated in two categories.** Clothing (44.7%) and Accessories (31.8%) generate about **76% of revenue**.
5. **Fall is the strongest season.** Fall has the highest average order value ($61.56), the only seasonal difference that is statistically significant.
6. **Few new customers.** Only **2.1%** of customers are first-time buyers, so growth depends on the existing base.

---

## Recommendations

| Priority | Action | Expected impact |
|---|---|---|
| 1 | Replace blanket discounts with spend-threshold or bundle offers, tested against a control group | Protect margin on ~$99K of discounted revenue |
| 2 | Open subscriptions and offers to female customers | Reach 32% of customers currently excluded |
| 3 | Launch a frequency program (points per visit, reminders) | Grow the segment that drives most annual spend |
| 4 | Run a first-purchase campaign before the Fall peak | Improve the 2.1% new-customer rate |
| 5 | Review jeans and shirts (fit, quality, price) | Fix the lowest-rated, lowest-revenue items |

---

## Project Structure

```
customer-shopping-behavior-analysis/
│
├── data/
│   ├── customer_shopping_behavior.csv            # Raw dataset
│   └── customer_shopping_behavior_cleaned.csv    # Cleaned dataset (Python output)
│
├── notebooks/
│   └── customer_behavior.ipynb                   # Loading, EDA and cleaning
│
├── sql/
│   └── customer_analysis_queries.sql             # 10 business questions (T-SQL)
│
├── dashboard/
│   └── customer_analysis.pbix                    # Power BI dashboard
│
├── report/
│   └── Customer_Shopping_Behavior_Report.docx    # Business report
│
├── images/                                       # Dashboard screenshots
└── README.md
```

---

## How to Run

1. **Clone the repository**
   ```bash
   git clone https://github.com/<your-username>/customer-shopping-behavior-analysis.git
   ```
2. **Python:** install dependencies and open the notebook
   ```bash
   pip install pandas jupyter
   jupyter notebook notebooks/customer_behavior.ipynb
   ```
3. **SQL Server:** import `customer_shopping_behavior_cleaned.csv` as table `dbo.customer` (e.g., with the SSMS Import Flat File wizard), then run `customer_analysis_queries.sql`.
4. **Power BI:** open `dashboard/customer_analysis.pbix` in Power BI Desktop.

---

## Limitations

- No transaction dates, so trends over time cannot be measured.
- One purchase per customer, so annual spend is estimated from the stated purchase frequency.
- Discount amounts are not recorded, so the exact margin cost of discounts is unknown.
- The data appears to be a sample or synthetic dataset; results should be validated with real transaction data.

---

## Author

**Minh Duc Le**
Business Technology Management student

- LinkedIn: [your-linkedin-url](#)
- Email: [your-email](#)
- Portfolio: [your-portfolio-url](#)
