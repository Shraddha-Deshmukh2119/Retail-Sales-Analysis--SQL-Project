# 💼 Retail Sales Analysis - SQL Project

## 🔧 Project Overview

### Project Title: Retail Sales Analysis
- **Level**: Beginner
- **Database**: sales_analysis

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

---

## 🎯 Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

---

## 🔄 Project Structure

### 1. Database Setup
- **Database Creation**: The project starts by creating a database named `sales_analysis`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE sales_analysis;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

### 2. Data Exploration & Cleaning
- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

### 3. Data Analysis & Findings
The following SQL queries were developed to answer specific business questions:

#### 1. Sales on '2022-11-05'
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

#### 2. Transactions for 'Clothing' with quantity > 4 in November 2022
```sql
SELECT *
FROM retail_sales
WHERE 
    category = 'Clothing'
    DATE(sale_date, '%y-%m') = '2022-11'
    AND quantity >= 4;
```

#### 3. Total Sales by Category
```sql
SELECT 
    category,
    SUM(total_sale) AS net_sale,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```

#### 4. Average Age for 'Beauty' Category
```sql
SELECT
    ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

#### 5. Transactions with Total Sale > 1000
```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
```

#### 6. Total Transactions by Gender and Category
```sql
SELECT 
    category,
    gender,
    COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```

#### 7. Best Selling Month Each Year
```sql
SELECT 
    year,
    month,
    avg_sale
FROM 
(
    SELECT 
        YEAR( sale_date) AS year,
        MONTH( sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER(PARTITION BY YEAR( sale_date) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY year, month
) AS t1
WHERE rank = 1;
```

#### 8. Top 5 Customers by Total Sales
```sql
SELECT 
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

#### 9. Unique Customers by Category
```sql
SELECT 
    category,
    COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;
```

#### 10. Orders by Time Shifts
```sql
WITH hourly_sale AS
(
    SELECT *,
        CASE
            WHEN HOUR (sale_time) < 12 THEN 'Morning'
            WHEN HOUR (sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
```

---

## 📊 Findings

1. **Customer Demographics**: Sales are distributed across age groups, with popular categories like Clothing and Beauty.
2. **High-Value Transactions**: Multiple transactions exceed a total sale of 1000, indicating premium purchases.
3. **Sales Trends**: Monthly analysis highlights seasonal peaks.
4. **Customer Insights**: Identifies top customers and most popular product categories.

---

## 📈 Reports

1. **Sales Summary**: Total sales, customer demographics, and category performance.
2. **Trend Analysis**: Seasonal and monthly trends.
3. **Customer Insights**: Insights into top customers and unique customer counts per category.

---

## 🚀 How to Use

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/Shraddha-Deshmukh2119/Retail-Sales-Analysis--SQL-Project.git
   ```
2. **Set Up the Database**:
   - Run the SQL scripts provided to create and populate the database.
3. **Run the Queries**:
   - Use the SQL queries provided for analysis.
4. **Explore and Modify**:
   - Experiment with the dataset and customize queries for deeper insights.

---

> "Data is the foundation of decisions; SQL is the tool to unlock its value." ⚡

