🟢 Beginner Level
00:00 Intro

-- What is SQL? It’s a language for querying relational databases.

07:38 Introduction to SQL

-- List all tables in the current database (PostgreSQL example)
SELECT table_name
FROM information_schema.tables
WHERE table_schema = 'public';

22:33 Setup Your Environment

# Install SQLite (example)
sudo apt-get install sqlite3
sqlite3 mydb.db

34:01 Query Data (SELECT)

SELECT first_name, last_name
FROM employees
WHERE department = 'Sales';

01:32:31 DDL Commands

CREATE TABLE customers (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100) UNIQUE
);
ALTER TABLE customers ADD COLUMN signup_date DATE;
DROP TABLE old_customers;

01:43:44 DML Commands

INSERT INTO customers (name, email, signup_date)
VALUES ('Alice', 'alice@example.com', '2025-08-06');
UPDATE customers
SET email = 'alice@newdomain.com'
WHERE id = 1;
DELETE FROM customers
WHERE signup_date < '2020-01-01';

🟡 Intermediate Level
02:08:03 Filtering Data

SELECT * FROM orders
WHERE amount BETWEEN 100 AND 500
  AND status IN ('shipped', 'delivered');

02:47:57 SQL Joins (Basics)

SELECT o.id, c.name
FROM orders o
INNER JOIN customers c
  ON o.customer_id = c.id;

03:27:29 SQL Joins (Advanced)

SELECT c.name, COUNT(o.id) AS order_count
FROM customers c
LEFT JOIN orders o
  ON o.customer_id = c.id AND o.status = 'completed'
RIGHT JOIN regions r
  ON c.region_id = r.id
FULL OUTER JOIN returns rt
  ON rt.order_id = o.id;

04:02:09 Set Operators

SELECT email FROM newsletter_subscribers
UNION
SELECT email FROM promo_subscribers;

04:47:41 SQL Functions

SELECT CURRENT_DATE, SESSION_USER, VERSION();

04:52:58 String Functions

SELECT UPPER(name), SUBSTR(email, 1, POSITION('@' IN email)-1)
FROM customers;

05:18:44 Numeric Functions

SELECT ROUND(price, 2), ABS(discount), FLOOR(quantity)
FROM products;

05:22:48 Date and Time Functions

SELECT NOW(), DATE_ADD(NOW(), INTERVAL 7 DAY), DATE_DIFF('day', order_date, NOW())
FROM orders;

06:59:06 NULL Functions

SELECT COALESCE(phone, 'N/A') AS phone_number
FROM customers;

08:07:50 Case Statement

SELECT order_id,
  CASE
    WHEN amount >= 1000 THEN 'High'
    WHEN amount >= 500 THEN 'Medium'
    ELSE 'Low'
  END AS priority
FROM orders;

08:43:36 Aggregate Functions

SELECT department, COUNT(*) AS cnt, AVG(salary), MAX(hire_date)
FROM employees
GROUP BY department;

08:50:11 Window Functions Basics

SELECT id, amount,
  ROW_NUMBER() OVER (ORDER BY amount DESC) AS rn
FROM orders;

09:47:00 Window Aggregate

SELECT id, amount,
  SUM(amount) OVER (PARTITION BY customer_id) AS total_per_customer
FROM orders;

10:53:09 Window Ranking

SELECT id, amount,
  RANK() OVER (PARTITION BY region ORDER BY amount DESC) AS region_rank
FROM sales;

11:56:05 Window Value

SELECT date, revenue,
  LAG(revenue) OVER (ORDER BY date) AS prev_day,
  LEAD(revenue) OVER (ORDER BY date) AS next_day
FROM daily_revenue;

🔴 Advanced Level
12:40:34 Advanced SQL Techniques

-- PIVOT example (SQL Server)
SELECT *
FROM sales
PIVOT (
  SUM(amount) FOR month IN ([Jan], [Feb], [Mar])
) AS pivot_table;

12:58:04 Subqueries

SELECT name
FROM customers
WHERE id IN (
  SELECT customer_id
  FROM orders
  WHERE amount > 1000
);

14:18:08 Common Table Expressions (CTE)

WITH recent_orders AS (
  SELECT * FROM orders
  WHERE order_date >= '2025-01-01'
)
SELECT customer_id, COUNT(*) 
FROM recent_orders
GROUP BY customer_id;

15:35:02 Views

CREATE VIEW high_value_customers AS
SELECT id, name
FROM customers
WHERE total_purchases > 5000;

16:36:40 CTAS and Temp Tables

-- CTAS
CREATE TABLE backup_orders AS
SELECT * FROM orders WHERE order_date < '2024-01-01';
-- Temp table (PostgreSQL)
CREATE TEMP TABLE tmp_top_customers AS
SELECT customer_id, SUM(amount) AS total
FROM orders
GROUP BY customer_id;

17:17:31 Compare Advanced Techniques

-- CTE vs subquery performance may vary; test both with EXPLAIN.

17:27:04 Stored Procedures

CREATE PROCEDURE AddProduct(name VARCHAR(100), price DECIMAL)
LANGUAGE SQL
AS $$
  INSERT INTO products (name, price) VALUES (name, price);
$$;

18:12:58 Triggers

CREATE TRIGGER update_timestamp
BEFORE UPDATE ON customers
FOR EACH ROW
EXECUTE PROCEDURE set_updated_timestamp();

18:23:42 Indexes

CREATE INDEX idx_orders_date ON orders(order_date);

20:20:31 Execution Plan

EXPLAIN ANALYZE
SELECT * FROM orders WHERE customer_id = 123;

21:11:03 Partitions

CREATE TABLE logs (
  id SERIAL,
  log_date DATE,
  message TEXT
) PARTITION BY RANGE (log_date);
CREATE TABLE logs_2025_08 PARTITION OF logs
  FOR VALUES FROM ('2025-08-01') TO ('2025-09-01');

21:43:39 30x Performance Tips

-- Example: use covering index
CREATE INDEX idx_covering ON orders(customer_id, order_date);

22:24:25 AI and SQL

-- Example: call a user-defined AI function to summarize text
SELECT ai_summarize(notes) FROM meeting_minutes;

🧪 Projects
23:21:04 Project: SQL Data Warehouse

-- Create fact and dimension tables
CREATE TABLE dim_date (...);
CREATE TABLE fact_sales (...);

24:32:54 Project DWH | Bronze

-- Ingest raw source data
COPY bronze.sales_raw FROM 's3://bucket/sales.csv' CSV HEADER;

25:10:09 Project DWH | Silver

-- Clean and transform
INSERT INTO silver.sales_clean
SELECT CAST(id AS INT), TRIM(name), price
FROM bronze.sales_raw;

26:47:46 Project DWH | Gold

-- Aggregate for reporting
CREATE TABLE gold.daily_sales AS
SELECT date, SUM(amount) AS total
FROM silver.sales_clean
GROUP BY date;

27:41:51 Project: Exploratory Data Analysis (EDA)

SELECT 
  COUNT(*) AS total_rows,
  AVG(amount) AS avg_amount,
  MIN(amount) AS min_amount,
  MAX(amount) AS max_amount
FROM silver.sales_clean;

28:30:38 Project: Advanced Data Analytics

-- K-means clustering via SQL (PostgreSQL MADlib)
SELECT madlib.kmeans('sales_clean','clusters',3,'{}');

29:47:24 Thank You

-- End of tutorial
Here’s a GitHub README-ready version of the SQL tutorial sections with concise, organized code examples and headings:

⸻

📘 SQL Tutorial Code Examples

This repository contains concise SQL code examples mapped to a full video tutorial timeline.
Use this as a quick reference or learning companion.

⸻

🟢 Beginner Level

🔹 00:00 Intro

Overview of SQL: Structured Query Language used for managing relational databases.

🔹 07:38 Introduction to SQL

-- List all tables (PostgreSQL)
SELECT table_name
FROM information_schema.tables
WHERE table_schema = 'public';

🔹 22:33 Setup Your Environment

# Example: Install SQLite & open DB
sudo apt-get install sqlite3
sqlite3 mydb.db

🔹 34:01 Query Data (SELECT)

SELECT first_name, last_name
FROM employees
WHERE department = 'Sales';

🔹 01:32:31 DDL Commands

CREATE TABLE customers (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100) UNIQUE
);

ALTER TABLE customers ADD COLUMN signup_date DATE;

DROP TABLE old_customers;

🔹 01:43:44 DML Commands

INSERT INTO customers (name, email, signup_date)
VALUES ('Alice', 'alice@example.com', '2025-08-06');

UPDATE customers
SET email = 'alice@newdomain.com'
WHERE id = 1;

DELETE FROM customers
WHERE signup_date < '2020-01-01';


⸻

🟡 Intermediate Level

🔸 02:08:03 Filtering Data

SELECT * FROM orders
WHERE amount BETWEEN 100 AND 500
  AND status IN ('shipped', 'delivered');

🔸 02:47:57 SQL Joins (Basics)

SELECT o.id, c.name
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id;

🔸 03:27:29 SQL Joins (Advanced)

SELECT c.name, COUNT(o.id) AS order_count
FROM customers c
LEFT JOIN orders o ON o.customer_id = c.id
GROUP BY c.name;

🔸 04:02:09 Set Operators

SELECT email FROM newsletter_subscribers
UNION
SELECT email FROM promo_subscribers;

🔸 04:47:41 SQL Functions

SELECT CURRENT_DATE, SESSION_USER;

🔸 04:52:58 String Functions

SELECT UPPER(name), SUBSTR(email, 1, POSITION('@' IN email)-1)
FROM customers;

🔸 05:18:44 Numeric Functions

SELECT ROUND(price, 2), ABS(discount)
FROM products;

🔸 05:22:48 Date & Time Functions

SELECT NOW(), order_date, NOW() - order_date AS days_diff
FROM orders;

🔸 06:59:06 NULL Functions

SELECT COALESCE(phone, 'N/A') AS phone_number
FROM customers;

🔸 08:07:50 CASE Statement

SELECT order_id,
  CASE
    WHEN amount >= 1000 THEN 'High'
    WHEN amount >= 500 THEN 'Medium'
    ELSE 'Low'
  END AS priority
FROM orders;

🔸 08:43:36 Aggregate Functions

SELECT department, COUNT(*) AS cnt, AVG(salary)
FROM employees
GROUP BY department;

🔸 08:50:11 Window Functions (Basics)

SELECT id, amount,
  ROW_NUMBER() OVER (ORDER BY amount DESC) AS rn
FROM orders;

🔸 09:47:00 Window Aggregate

SELECT id, customer_id, amount,
  SUM(amount) OVER (PARTITION BY customer_id) AS total_per_customer
FROM orders;

🔸 10:53:09 Window Ranking

SELECT id, amount,
  RANK() OVER (PARTITION BY region ORDER BY amount DESC) AS region_rank
FROM sales;

🔸 11:56:05 Window Value

SELECT date, revenue,
  LAG(revenue) OVER (ORDER BY date) AS prev_day,
  LEAD(revenue) OVER (ORDER BY date) AS next_day
FROM daily_revenue;


⸻

🔴 Advanced Level

🔻 12:40:34 Advanced Techniques (Pivot)

-- SQL Server
SELECT * FROM (
  SELECT month, product, amount
  FROM sales
) AS source
PIVOT (
  SUM(amount) FOR month IN ([Jan], [Feb], [Mar])
) AS pvt;

🔻 12:58:04 Subqueries

SELECT name
FROM customers
WHERE id IN (
  SELECT customer_id FROM orders WHERE amount > 1000
);

🔻 14:18:08 Common Table Expressions (CTEs)

WITH recent_orders AS (
  SELECT * FROM orders WHERE order_date >= '2025-01-01'
)
SELECT customer_id, COUNT(*)
FROM recent_orders
GROUP BY customer_id;

🔻 15:35:02 Views

CREATE VIEW high_value_customers AS
SELECT id, name
FROM customers
WHERE total_purchases > 5000;

🔻 16:36:40 CTAS & Temp Tables

-- CTAS
CREATE TABLE backup_orders AS
SELECT * FROM orders WHERE order_date < '2024-01-01';

-- Temporary Table (PostgreSQL)
CREATE TEMP TABLE tmp_top_customers AS
SELECT customer_id, SUM(amount) AS total
FROM orders
GROUP BY customer_id;

🔻 17:27:04 Stored Procedures

CREATE PROCEDURE AddProduct(name VARCHAR(100), price DECIMAL)
LANGUAGE SQL
AS $$
  INSERT INTO products (name, price) VALUES (name, price);
$$;

🔻 18:12:58 Triggers

CREATE TRIGGER update_timestamp
BEFORE UPDATE ON customers
FOR EACH ROW
EXECUTE PROCEDURE set_updated_timestamp();

🔻 18:23:42 Indexes

CREATE INDEX idx_orders_date ON orders(order_date);

🔻 20:20:31 Execution Plan

EXPLAIN ANALYZE
SELECT * FROM orders WHERE customer_id = 123;

🔻 21:11:03 Partitions

CREATE TABLE logs (
  id SERIAL,
  log_date DATE,
  message TEXT
) PARTITION BY RANGE (log_date);

CREATE TABLE logs_2025_08 PARTITION OF logs
  FOR VALUES FROM ('2025-08-01') TO ('2025-09-01');

🔻 21:43:39 Performance Tips

-- Use covering indexes for performance
CREATE INDEX idx_covering ON orders(customer_id, order_date);

🔻 22:24:25 AI + SQL Example

-- Hypothetical AI SQL function
SELECT ai_summarize(notes) FROM meeting_minutes;


⸻

🧪 Projects

🧱 23:21:04 SQL Data Warehouse

-- Dimensional model
CREATE TABLE dim_date (...);
CREATE TABLE fact_sales (...);

🧱 24:32:54 DWH Bronze (Raw Layer)

COPY bronze.sales_raw FROM 's3://bucket/sales.csv' CSV HEADER;

🧱 25:10:09 DWH Silver (Clean Layer)

INSERT INTO silver.sales_clean
SELECT CAST(id AS INT), TRIM(name), price
FROM bronze.sales_raw;

🧱 26:47:46 DWH Gold (Aggregated Layer)

CREATE TABLE gold.daily_sales AS
SELECT date, SUM(amount) AS total
FROM silver.sales_clean
GROUP BY date;

📊 27:41:51 Exploratory Data Analysis (EDA)

SELECT COUNT(*) AS total_rows,
       AVG(amount) AS avg_amount,
       MIN(amount),
       MAX(amount)
FROM silver.sales_clean;

📈 28:30:38 Advanced Analytics

-- K-means clustering via MADlib (PostgreSQL)
SELECT madlib.kmeans('sales_clean', 'clusters', 3, '{}');
