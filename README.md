# Customer Shopping Behavior Analysis

An end-to-end data analysis project on retail customer shopping behavior. Raw data is cleaned and enriched in **Python**, loaded into **PostgreSQL**, queried with **SQL** to answer business questions, and visualized in a **Power BI** dashboard.

## Project Workflow

```
customer_shopping.csv  →  Python (pandas)  →  PostgreSQL  →  SQL analysis  →  Power BI dashboard
     (raw data)          clean + feature eng.   (customer table)  (10 questions)     (visual report)
```

## Repository Contents

| File | Description |
|------|-------------|
| `customer_shopping.csv` | Raw dataset (3,900 rows, 18 columns) |
| `Customer_Shopping_python.ipynb` | Data loading, cleaning, feature engineering, and PostgreSQL load |
| `customer_shopping_sql.sql` | 10 business-question queries run on the `customer` table |
| `customer_shopping_bi.pbix` | Power BI dashboard (single page with slicers, KPI card, bar/column charts, and donut chart) |

## Dataset

Each row is one customer's purchase record.

| Column | Description |
|--------|-------------|
| Customer ID | Unique customer identifier |
| Age, Gender | Customer demographics |
| Item Purchased, Category | Product bought and its category |
| Purchase Amount (USD) | Amount spent |
| Location | US state |
| Size, Color, Season | Product attributes and purchase season |
| Review Rating | Rating given (37 missing values) |
| Subscription Status | Whether the customer is subscribed |
| Shipping Type | Standard, Express, Free Shipping, etc. |
| Discount Applied, Promo Code Used | Discount and promo usage flags |
| Previous Purchases | Number of earlier purchases |
| Payment Method | Payment type used |
| Frequency of Purchases | How often the customer buys |

## Data Preparation (Python)

Steps performed in `Customer_Shopping_python.ipynb`:

1. **Load and inspect** the CSV with `head()`, `info()`, `describe()`, and null checks.
2. **Handle missing values:** the 37 missing `Review Rating` values are filled with the median rating of each product category.
3. **Standardize column names:** lowercase with underscores, and `Purchase Amount (USD)` renamed to `purchase_amount`.
4. **Feature engineering:**
   - `age_group`: customers split into four quartile-based groups (Young Adult, Adult, Middle-aged, Senior) using `pd.qcut`.
   - `purchase_frequency_days`: purchase frequency text converted to a number of days (for example Weekly = 7, Monthly = 30, Quarterly = 90).
5. **Drop redundant column:** `promo_code_used` was removed after confirming it is identical to `discount_applied`.
6. **Load to PostgreSQL:** the cleaned DataFrame is written to a `customer` table in the `customer_shopping_database` database using SQLAlchemy and psycopg2.

## SQL Analysis

`customer_shopping_sql.sql` answers these business questions:

1. Total revenue from male vs. female customers
2. Customers who used a discount but still spent above the average purchase amount
3. Top 5 products by average review rating
4. Average purchase amount: Standard vs. Express shipping
5. Do subscribers spend more? (average spend and total revenue)
6. Top 5 products with the highest percentage of discounted purchases
7. Customer segments (New, Returning, Loyal) by previous purchases
8. Top 3 most purchased products within each category
9. Are repeat buyers (more than 5 previous purchases) likely to subscribe?
10. Revenue contribution of each age group

Techniques used: aggregation, `GROUP BY`/`HAVING`, subqueries, CTEs, `CASE WHEN`, window functions (`ROW_NUMBER() OVER (PARTITION BY ...)`), and type casting.

## Power BI Dashboard

`customer_shopping_bi.pbix` presents the findings visually, with slicers for interactive filtering, a KPI card, clustered bar and column charts, and a donut chart.

## Tech Stack

- **Python:** pandas, SQLAlchemy, psycopg2
- **Database:** PostgreSQL
- **BI:** Power BI Desktop
- **Tools:** Jupyter Notebook

## How to Run

1. **Install dependencies**
   ```bash
   pip install pandas sqlalchemy psycopg2-binary jupyter
   ```
2. **Create a PostgreSQL database** named `customer_shopping_database`.
3. **Run the notebook** `Customer_Shopping_python.ipynb`. Before running, update:
   - the CSV path in the first cell to where the file is on your machine
   - the PostgreSQL credentials (username, password, host, port) to your own
4. **Run the SQL queries** in `customer_shopping_sql.sql` against the `customer` table using pgAdmin, DBeaver, or `psql`.
5. **Open the dashboard** `customer_shopping_bi.pbix` in Power BI Desktop. If the data source path breaks, update it via *Transform data → Data source settings*.

