# 📘 SQL Tutorial Code Examples

This repository provides concise SQL code snippets aligned with a full video tutorial.  
Use this for quick reference or structured learning.

---

## 🟢 Beginner Level

### 🔹 00:00 Intro  
*What is SQL?* Structured Query Language for managing relational data.

### 🔹 07:38 Introduction to SQL  
```sql
-- List all tables in the current database (PostgreSQL example)
SELECT table_name
FROM information_schema.tables
WHERE table_schema = 'public';
```

### 🔹 22:33 Setup Your Environment  
```bash
# Install SQLite and open DB
sudo apt-get install sqlite3
sqlite3 mydb.db
```

### 🔹 34:01 Query Data (SELECT)  
```sql
SELECT first_name, last_name
FROM employees
WHERE department = 'Sales';
```

### 🔹 01:32:31 DDL Commands  
```sql
CREATE TABLE customers (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100) UNIQUE
);

ALTER TABLE customers ADD COLUMN signup_date DATE;

DROP TABLE old_customers;
```

### 🔹 01:43:44 DML Commands  
```sql
INSERT INTO customers (name, email, signup_date)
VALUES ('Alice', 'alice@example.com', '2025-08-06');

UPDATE customers
SET email = 'alice@newdomain.com'
WHERE id = 1;

DELETE FROM customers
WHERE signup_date < '2020-01-01';
```

---

## 🟡 Intermediate Level

### 🔸 02:08:03 Filtering Data  
```sql
SELECT * FROM orders
WHERE amount BETWEEN 100 AND 500
  AND status IN ('shipped', 'delivered');
```

### 🔸 02:47:57 SQL Joins (Basics)  
```sql
SELECT o.id, c.name
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id;
```

### 🔸 03:27:29 SQL Joins (Advanced)  
```sql
SELECT c.name, COUNT(o.id) AS order_count
FROM customers c
LEFT JOIN orders o ON o.customer_id = c.id
GROUP BY c.name;
```

### 🔸 04:02:09 Set Operators  
```sql
SELECT email FROM newsletter_subscribers
UNION
SELECT email FROM promo_subscribers;
```

### 🔸 04:47:41 SQL Functions  
```sql
SELECT CURRENT_DATE, SESSION_USER;
```

### 🔸 04:52:58 String Functions  
```sql
SELECT UPPER(name), SUBSTRING(email FROM 1 FOR POSITION('@' IN email) - 1)
FROM customers;
```

### 🔸 05:18:44 Numeric Functions  
```sql
SELECT ROUND(price, 2), ABS(discount), FLOOR(quantity)
FROM products;
```

### 🔸 05:22:48 Date and Time Functions  
```sql
SELECT NOW(), order_date, NOW() - order_date AS days_elapsed
FROM orders;
```

### 🔸 06:59:06 NULL Functions  
```sql
SELECT COALESCE(phone, 'N/A') AS phone_number
FROM customers;
```

### 🔸 08:07:50 CASE Statement  
```sql
SELECT order_id,
  CASE
    WHEN amount >= 1000 THEN 'High'
    WHEN amount >= 500 THEN 'Medium'
    ELSE 'Low'
  END AS priority
FROM orders;
```

### 🔸 08:43:36 Aggregate Functions  
```sql
SELECT department, COUNT(*) AS count, AVG(salary), MAX(hire_date)
FROM employees
GROUP BY department;
```

### 🔸 08:50:11 Window Functions (Basics)  
```sql
SELECT id, amount,
  ROW_NUMBER() OVER (ORDER BY amount DESC) AS rn
FROM orders;
```

### 🔸 09:47:00 Window Aggregate  
```sql
SELECT id, amount,
  SUM(amount) OVER (PARTITION BY customer_id) AS total_per_customer
FROM orders;
```

### 🔸 10:53:09 Window Ranking  
```sql
SELECT id, amount,
  RANK() OVER (PARTITION BY region ORDER BY amount DESC) AS region_rank
FROM sales;
```

### 🔸 11:56:05 Window Value  
```sql
SELECT date, revenue,
  LAG(revenue) OVER (ORDER BY date) AS prev_day,
  LEAD(revenue) OVER (ORDER BY date) AS next_day
FROM daily_revenue;
```

---

## 🔴 Advanced Level

### 🔻 12:40:34 Advanced SQL (Pivot)  
```sql
-- SQL Server example
SELECT * FROM (
  SELECT month, product, amount
  FROM sales
) AS source
PIVOT (
  SUM(amount) FOR month IN ([Jan], [Feb], [Mar])
) AS pvt;
```

### 🔻 12:58:04 Subqueries  
```sql
SELECT name
FROM customers
WHERE id IN (
  SELECT customer_id FROM orders WHERE amount > 1000
);
```

### 🔻 14:18:08 Common Table Expressions (CTEs)  
```sql
WITH recent_orders AS (
  SELECT * FROM orders WHERE order_date >= '2025-01-01'
)
SELECT customer_id, COUNT(*)
FROM recent_orders
GROUP BY customer_id;
```

### 🔻 15:35:02 Views  
```sql
CREATE VIEW high_value_customers AS
SELECT id, name
FROM customers
WHERE total_purchases > 5000;
```

### 🔻 16:36:40 CTAS & Temp Tables  
```sql
CREATE TABLE backup_orders AS
SELECT * FROM orders WHERE order_date < '2024-01-01';

CREATE TEMP TABLE tmp_top_customers AS
SELECT customer_id, SUM(amount) AS total
FROM orders
GROUP BY customer_id;
```

### 🔻 17:27:04 Stored Procedures  
```sql
CREATE PROCEDURE AddProduct(name VARCHAR(100), price DECIMAL)
LANGUAGE SQL
AS $$
  INSERT INTO products (name, price) VALUES (name, price);
$$;
```

### 🔻 18:12:58 Triggers  
```sql
CREATE TRIGGER update_timestamp
BEFORE UPDATE ON customers
FOR EACH ROW
EXECUTE FUNCTION set_updated_timestamp();
```

### 🔻 18:23:42 Indexes  
```sql
CREATE INDEX idx_orders_date ON orders(order_date);
```

### 🔻 20:20:31 Execution Plan  
```sql
EXPLAIN ANALYZE
SELECT * FROM orders WHERE customer_id = 123;
```

### 🔻 21:11:03 Partitions  
```sql
CREATE TABLE logs (
  id SERIAL,
  log_date DATE,
  message TEXT
) PARTITION BY RANGE (log_date);

CREATE TABLE logs_2025_08 PARTITION OF logs
  FOR VALUES FROM ('2025-08-01') TO ('2025-09-01');
```

### 🔻 21:43:39 Performance Tips  
```sql
CREATE INDEX idx_covering ON orders(customer_id, order_date);
```

### 🔻 22:24:25 AI + SQL Example  
```sql
SELECT ai_summarize(notes) FROM meeting_minutes;
```

---

## 🧪 Projects

### 🧱 23:21:04 Data Warehouse Setup  
```sql
CREATE TABLE dim_date (...);
CREATE TABLE fact_sales (...);
```

### 🧱 24:32:54 DWH Bronze (Raw Layer)  
```sql
COPY bronze.sales_raw FROM 's3://bucket/sales.csv' CSV HEADER;
```

### 🧱 25:10:09 DWH Silver (Clean Layer)  
```sql
INSERT INTO silver.sales_clean
SELECT CAST(id AS INT), TRIM(name), price
FROM bronze.sales_raw;
```

### 🧱 26:47:46 DWH Gold (Aggregated Layer)  
```sql
CREATE TABLE gold.daily_sales AS
SELECT date, SUM(amount) AS total
FROM silver.sales_clean
GROUP BY date;
```

### 📊 27:41:51 Exploratory Data Analysis (EDA)  
```sql
SELECT COUNT(*) AS total_rows,
       AVG(amount) AS avg_amount,
       MIN(amount),
       MAX(amount)
FROM silver.sales_clean;
```

### 📈 28:30:38 Advanced Analytics  
```sql
SELECT madlib.kmeans('sales_clean', 'clusters', 3, '{}');
```
