# sql_retail_sales_project_1
**-- create TABLE**
```sql
DROP TABLE IF EXISTS retail_sales;
CREATE TABLE retail_sales
(
  transactions_id	INT PRIMARY KEY,
  sale_date DATE,	
  sale_time TIME,
  customer_id	INT,
  gender VARCHAR(15),
  age	INT,
  category VARCHAR(15),
  quantiy INT,
  price_per_unit FLOAT,
  cogs FLOAT,
  total_sale FLOAT
);
SELECT * FROM retail_sales
LIMIT 10;

SELECT COUNT(*) 
FROM retail_sales;
```

**-- Deleting empty colums**
```sql
DELETE FROM retail_sales
WHERE
transactions_id IS NULL
   OR
   sale_date IS NULL
   OR
   sale_time IS NULL
   OR
   customer_id IS NULL
   OR
   gender IS NULL
   OR
   age IS NULL
   OR
   category IS NULL
   OR
   quantiy IS NULL
   OR
   price_per_unit IS NULL
   OR
   cogs IS NULL
   OR
   total_sale IS NULL;
```
--   Data Exploration
**-- How many sales we have**
```sql
SELECT COUNT(*) as total_sale FROM retail_sales;
```
**-- How many unique customers you have**
```sql
SELECT COUNT(DISTINCT customer_id) as total_sale FROM retail_sales;
```
**--  How many unique category you have**
```sql
SELECT COUNT (DISTINCT category) as total_sale FROM retail_sales;
```
**-- categories**
```sql
SELECT DISTINCT category FROM retail_sales;
```
-- DATA ANALYSIS AND BUSINESS KEY PROBLEMS AND ANSWERS
1.**SQL query to retrieve all columns for sales made on "2022-11-05"**:
```sql
SELECT * 
FROM retail_sales
WHERE sale_date = '2022-11-05';
```
2.**SQL Query to retrieve all transactions where the category is clothing and the quantity sold is morethan 4 in the month of Nov-2022**
```sql
SELECT
 *
FROM retail_sales
WHERE category = 'Clothing'
  AND
  TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND
  quantiy >= 4
```
3.**SQL query to calculate the total sales from each category**
```sql
SELECT 
  category,
  SUM(total_sale) as net_sales,
  COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1
```
4.**SQL to find the average age of customers who purchase items from the beauty category**
```sql
SELECT
  ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'
```
5.**SQL query to find all transactions where total sale is greaterthan 1000**
```sql
SELECT
*
FROM retail_sales
WHERE total_sale > 1000
```
6.**SQL query to find the total number of transaction made by each gender in each category**
```sql
SELECT
  category,
  gender,
  COUNT(*) as total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY 1
```
7.**SQL query to calculate the best sale in each month. find the best selling month for each year**
```sql
SELECT
  year,
  month,
  avg_sale
FROM
(
SELECT
  EXTRACT(YEAR FROM sale_date) as year,
  EXTRACT(MONTH FROM sale_date) as month,
  AVG(total_sale) as avg_sale,
  RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1
```
8.**SQL query to find the top five customers based on the highest total sales**
```sql
SELECT
  customer_id,
  SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```
9.**SQL query to find the number of unique customers who purchased items from each category**
```sql
SELECT
  category,
  COUNT(DISTINCT customer_id) as coun_uni_customer
FROM retail_sales
GROUP BY category
```
10.**SQL query to create each shift and number of orders(Example Morning <=12, Afternoon Between 12 &17, Evening >17)**
```sql
WITH hourly_sales
AS(
SELECT*,
  CASE
  WHEN EXTRACT(HOUR FROM sale_time) <= 12 THEN 'Morning' 
  WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17  THEN 'Afternoo'
  ELSE 'Evening'
 END as shift 
FROM retail_sales
)
SELECT
  shift,
  COUNT(*) as total_orders
FROM hourly_sales
GROUP BY shift
```
**-- End Of Project**
-- Maiden project
