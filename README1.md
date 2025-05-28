
# Retail Sales Analysis SQL Project
### Zakirul Khan
## Project Overview

**Project Title**: Retail Sales Analysis  
**Database**: `sql_project_p2`

This project demonstrates foundational SQL skills for data analysis by building and analyzing a retail sales database. The workflow covers database setup, data cleaning, exploratory data analysis (EDA), and business-driven SQL queries to extract insights from retail sales data.

---

## Objectives

1. **Database Setup**: Create and populate a `retail_sales` table with relevant sales data.
2. **Data Cleaning**: Identify and remove records with missing or null values to ensure data quality.
3. **Exploratory Data Analysis**: Perform queries to understand sales volume, unique customers, and product categories.
4. **Business Analysis**: Use SQL queries to answer specific business questions about sales patterns, customer behavior, and product performance.

---

## Project Structure

### Database and Table Creation

- Database: `sql_project_p2`  
- Table: `retail_sales` stores transactional sales data including transaction ID, sale date/time, customer details, product category, quantities, pricing, costs, and total sales.

```sql
CREATE DATABASE sql_project_p2;

DROP TABLE IF EXISTS retail_sales;
CREATE TABLE retail_sales
(
    transaction_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

---

### Data Cleaning and Validation

Identify and remove any records with missing or null values.

```sql
SELECT * FROM retail_sales
WHERE transaction_id IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR gender IS NULL
   OR category IS NULL
   OR quantity IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;

DELETE FROM retail_sales
WHERE transaction_id IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR gender IS NULL
   OR category IS NULL
   OR quantity IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
```

---

### Exploratory Data Analysis (EDA)

Basic queries to explore sales data.

```sql
-- Total number of sales
SELECT COUNT(*) AS total_sale FROM retail_sales;

-- Unique customers count
SELECT COUNT(DISTINCT customer_id) AS unique_customers FROM retail_sales;

-- Count of distinct categories
SELECT COUNT(DISTINCT category) FROM retail_sales;

-- List distinct categories
SELECT DISTINCT category FROM retail_sales;
```

---

## Business Problem Queries and Analysis

```sql
-- Q1: Sales on '2022-11-05'
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';

-- Q2: Clothing transactions with quantity > 4 in Nov 2022
SELECT *
FROM retail_sales
WHERE category = 'Clothing' 
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity > 4;

-- Q3: Total sales per category
SELECT category, SUM(total_sale) AS net_sales
FROM retail_sales
GROUP BY category;

-- Q4: Average age of customers buying 'Beauty' products
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';

-- Q5: Transactions with total_sale > 1000
SELECT * FROM retail_sales
WHERE total_sale > 1000;

-- Q6: Total transactions by gender and category
SELECT category, gender, COUNT(*) AS total_transactions
FROM retail_sales
GROUP BY category, gender
ORDER BY category;

-- Q7: Best selling month per year based on average sales
SELECT year, month, avg_sale FROM
(
  SELECT EXTRACT(YEAR FROM sale_date) AS year, 
         EXTRACT(MONTH FROM sale_date) AS month,
         AVG(total_sale) AS avg_sale,
         RANK() OVER (PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
  FROM retail_sales
  GROUP BY 1, 2
) AS ranked_sales
WHERE rank = 1;

-- Q8: Top 5 customers by total sales
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;

-- Q9: Unique customers per category
SELECT category, COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;

-- Q10: Sales shifts and number of orders
WITH hourly_sales AS (
  SELECT *,
    CASE
      WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
      WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
      ELSE 'Evening'
    END AS shift
  FROM retail_sales
)
SELECT shift, COUNT(transaction_id) AS total_orders
FROM hourly_sales
GROUP BY shift;
```

---

## Key Findings

- Sales cover diverse product categories and customer segments.
- Several high-value transactions exceeding $1000.
- Monthly and shift-based trends highlight peak sales periods.
- Top customers identified by total purchase value.
- Insights support data-driven business strategies and marketing.

---

## Usage Instructions

1. **Clone this repository** to your local machine.
2. **Run the SQL scripts** provided to set up your database and populate data.
3. **Execute the analysis queries** to explore sales patterns and customer behavior.
4. **Modify queries** as needed to explore additional questions or datasets.

---

## Author

This project showcases my SQL skills applied to retail sales data analysis. Feedback and collaboration are welcome!

---
