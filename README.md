# Customer Shopping Behavior Analysis

An end-to-end data analytics project on 3,900 retail purchase records. The data is cleaned in Python, analysed with SQL in PostgreSQL, and presented in a Power BI dashboard, a written report and a slide deck.


## Overview

A retail business wants to understand how its customers shop. This project answers four questions:

- Which customer groups (gender, age, subscribers) bring in the most revenue?
- Which products and categories sell best, and how are they rated?
- Do discounts, subscriptions and express shipping lead to bigger purchases?
- What should the business change based on the data?

## Dataset

| | |
| --- | --- |
| File | `customer_shopping_behavior.csv` |
| Rows | 3,900 (one row per purchase) |
| Columns | 18 |
| Missing values | 37, all in `Review Rating` |

The columns cover three areas:

- Customer details: age, gender, location, subscription status
- Purchase details: item, category, purchase amount (USD), season, size, color
- Shopping behavior: discount applied, promo code used, previous purchases, purchase frequency, review rating, shipping type, payment method

## Tools

| Tool | Used for |
| --- | --- |
| Python (pandas) | Loading, EDA, cleaning, feature engineering |
| Jupyter Notebook | Running and documenting the Python work |
| PostgreSQL | Storing the cleaned table and running business queries |
| SQLAlchemy, psycopg2 | Loading the DataFrame from Python into PostgreSQL |
| Power BI Desktop | Interactive dashboard |
| Claude (AI assistant) | Drafting the project report and the slide deck |

## Steps

1. **Load and explore.** Read the CSV with pandas and checked structure and summary statistics with `df.info()` and `df.describe()`.
2. **Clean.** Filled the 37 missing review ratings with the median rating of each product category. Renamed columns to snake_case (for example `Purchase Amount (USD)` became `purchase_amount`).
3. **Engineer features.** Added `age_group` (four equal-sized age bins using `pd.qcut`) and `purchase_frequency_days` (purchase frequency converted to a number of days).
4. **Remove redundancy.** `promo_code_used` matched `discount_applied` on every row, so it was dropped.
5. **Load to PostgreSQL.** Wrote the cleaned DataFrame to a table called `customer`.
6. **Analyse with SQL.** Wrote 10 queries covering revenue by gender and age group, top-rated products, discount usage, shipping types, subscription impact, customer segments and best sellers per category.
7. **Build the dashboard.** Connected Power BI to the data and built a one-page dashboard.
8. **Report and present.** Wrote up the findings and recommendations in a report and turned them into a slide deck with the help of Claude.

## Dashboard

The Power BI dashboard (`Customer_behaviour.pbix`) has:

- Slicers for subscription status, gender, category and shipping type
- KPI cards: number of customers, average purchase amount, average review rating
- Charts for subscription share, revenue and sales by category, and revenue and sales by age group

## Results

- Total revenue is $233,081, with an average purchase of $59.76.
- Clothing brings in 45% of revenue and Outerwear only 8%. Average order value is about the same ($57 to $60) in every category.
- Subscribers do not spend more than non-subscribers ($59.49 vs $59.87 per purchase), and repeat buyers subscribe at the same 27% rate as everyone else.
- Hat, Sneakers and Coat are sold on discount about half the time, against 43% across the store.
- Young adults (18 to 31) bring in the most revenue of any age group.
- Express shipping orders average about $2 more than Standard.

Main recommendations: rework the subscription offer, test lower discounts on the most-discounted items, expand the Outerwear range and keep top-selling Clothing and Accessories items in stock.

Limitations: the data has no purchase dates, every subscriber and discount user in it is male, and every customer has at least one previous purchase. These look like artifacts of how the dataset was built, so the gender and segment results should be read with care.

## How to run

**Requirements:** Python 3.10+, PostgreSQL, Power BI Desktop (Windows).

1. Clone the repository and install the Python packages:

   ```bash
   pip install pandas sqlalchemy psycopg2-binary jupyter
   ```

2. Create a database in PostgreSQL:

   ```sql
   CREATE DATABASE customer_behavior;
   ```

3. Open the notebook and set your own PostgreSQL username and password in the connection cell. Don't commit real credentials to the repository.

   ```bash
   jupyter notebook demo.ipynb
   ```

   Run all cells. The last cell creates the `customer` table.

4. Run the queries in `customer_behaviour_analysis.sql` using pgAdmin or `psql`:

   ```bash
   psql -U postgres -d customer_behavior -f customer_behaviour_analysis.sql
   ```

5. Open `Customer_behaviour.pbix` in Power BI Desktop. If the data source needs updating, go to Transform data › Data source settings and point it to your PostgreSQL database.

## Project files

```
├── customer_shopping_behavior.csv     # raw dataset
├── demo.ipynb                         # Python: EDA, cleaning, load to PostgreSQL
├── customer_behaviour_analysis.sql    # 10 business queries
├── Customer_behaviour.pbix            # Power BI dashboard
├── report/                            # project report
├── presentation/                      # slide deck
└── images/
    └── dashboard.png                  # dashboard screenshot
```

## Author

Vibhav, MSc Computer Applications (Data Science)
