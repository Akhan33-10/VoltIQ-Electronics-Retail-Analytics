# ⚡ VoltIQ — Global Electronics Retail Analytics

## 📌 Project Overview

VoltIQ is an end-to-end business intelligence project analyzing a global electronics retailer operating across **8 countries**, **3 continents**, and **67 stores** worldwide. The goal was to uncover revenue drivers, profitability patterns, and customer behaviour using SQL-verified insights visualized in a 2-page interactive Power BI dashboard.

---

## 🗂️ Dataset

| Table | Rows | Description |
|---|---|---|
| Sales | 62,884 | Transaction records (2016–2021) |
| Customers | 15,266 | Demographics & location |
| Products | 2,517 | Category, brand, cost & price |
| Stores | 67 | Store location & size |
| Exchange Rates | 11,215 | Daily multi-currency rates |
| Data Dictionary | — | Column definitions |

**Source:** Maven Analytics — Global Electronics Retailer Dataset

---

## 🛠️ Tools & Technologies

- **Python** — Data cleaning (encoding fix, currency symbol removal)
- **PostgreSQL** — Data loading, exploration & SQL analysis
- **Power BI** — Dashboard design, DAX measures, bookmarks
- **DAX** — Custom measures including currency-normalized revenue, median order value, avg delivery days
- **Power Query** — Composite key creation for exchange rate relationship

---

## 🔧 Data Preparation

### Python Cleaning
- Fixed UTF-8 encoding error in Customers file (special characters in names)
- Removed `$` signs and `,` thousand separators from Products price columns
- Exported clean CSVs for PostgreSQL import

### PostgreSQL Loading
- Created 5 tables with appropriate data types
- Fixed `state_code` column size after import error
- Loaded all 62,884 sales rows successfully

### Power BI Data Model
- Established relationships across all 5 tables
- Solved composite key challenge for Exchange Rates:
  - Created `currency_date_key` column in both Sales and Exchange Rates tables
  - Connected via `currency_code + order_date` to ensure correct daily exchange rate per transaction

---

## 📊 Dashboard

### Page 1 — Business Performance

**KPIs:** Total Revenue · Total Cost · Total Profit · Profit Margin % · Total Orders · Total Units Sold

**Visuals:**
- Revenue & Profit by Country
- Revenue & Profit by Category
- Top 5 Brands by Revenue
- Revenue & Profit Trend (2016–2020)
- Store Size vs Revenue (Scatter)
- Interactive Insight Panel (Bookmark)

### Page 2 — Customer Insights & Behaviour

**KPIs:** Total Customers · Avg Order Value · Median Order Value · Total Countries · Avg Delivery Days

**Visuals:**
- Median Order Value by Country
- Revenue by Continent
- Revenue by Gender (Donut)
- Total Customers by Year
- Total Customers by Age Group

---

## 🔍 Key Findings & Recommendations

### Finding 1 — US Market Concentration Risk
**Finding:** United States drives 54% of total revenue ($30M out of $55M) despite operating in 8 countries across 3 continents.
**Action:** Diversify marketing spend toward high-potential underserved markets, particularly in Europe where 5 countries combined generate only $15.5M.
**Impact:** Reducing US dependency from 54% to 40% by growing European markets could stabilize revenue against US economic downturns.

---

### Finding 2 — Australia is the Hidden Gem
**Finding:** Australia has the lowest order volume (1,182 orders) but the highest median order value ($1,672) — customers spend 46% more per order than the global median ($1,122).
**Action:** Increase marketing investment and store presence in Australia to capture more high-value customers.
**Impact:** Even a 50% increase in Australian order volume could add ~$1.9M revenue without acquiring lower-value customers.

---

### Finding 3 — Category Concentration
**Finding:** Computers ($19M) and Home Appliances ($11M) together account for 54% of revenue despite 9 categories being available.
**Action:** Investigate underperforming categories (Games & Toys, Music & Audio Books) for either growth opportunity or portfolio rationalization.
**Impact:** A 20% revenue lift in the bottom 4 categories would add ~$2M annually.

---

### Finding 4 — 60+ is the Core Customer Segment
**Finding:** Customers aged 60+ represent the largest segment (5,632 customers) and drive $26M in revenue — 47% of total.
**Action:** Design loyalty programs and product recommendations targeting the 60+ demographic while developing strategies to attract under-30 customers (only 942 currently).
**Impact:** Growing the under-30 segment to match 30-44 levels would add ~1,700 new customers.

---

### Finding 5 — COVID-19 Impact & Recovery
**Finding:** Revenue peaked at $18.3M in 2019 then dropped 44% to $10.3M in 2020 due to COVID-19. 2021 data is partial (Jan–Feb only) and excludes full recovery analysis.
**Action:** Conduct post-2021 analysis when full data is available to measure recovery rate and identify which markets rebounded fastest.
**Impact:** Understanding recovery patterns helps prioritize where to reinvest first.

---

## 💡 SQL Queries Highlights

```sql
-- Currency-normalized revenue by country
SELECT 
    c.continent, c.country,
    ROUND(SUM(s.quantity * p.unit_price_usd * er.exchange), 2) AS revenue,
    COUNT(DISTINCT s.order_number) AS total_orders,
    PERCENTILE_CONT(0.5) WITHIN GROUP 
        (ORDER BY s.quantity * p.unit_price_usd * er.exchange) AS median_order_value
FROM customers c
JOIN sales s ON c.customerkey = s.customerkey
JOIN products p ON s.productkey = p.productkey
JOIN exchange_rates er 
    ON s.currency_code = er.currency 
    AND s.order_date = er.date
GROUP BY c.continent, c.country
ORDER BY revenue DESC;
```

---

## 📁 Repository Structure

```
VoltIQ/
│
├── data/
│   └── (source CSV files)
│
├── sql/
│   └── analysis_queries.sql
│
├── dashboard/
│   └── VoltIQ.pbix
│
├── screenshots/
│   ├── page1_business.png
│   └── page2_customers.png
│
└── README.md
```

---

## 👤 Author

**Anas Khan**
[GitHub](https://github.com/Akhan33-10) · [LinkedIn](#)

---

*This project is part of my data analytics portfolio demonstrating end-to-end skills across data cleaning, SQL analysis, data modeling, and business intelligence.*
