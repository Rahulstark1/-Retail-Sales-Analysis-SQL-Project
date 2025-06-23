# -Retail-Sales-Analysis-SQL-Project

## Project Overview

**Project Title**: Retail Sales Analysis  

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `RETAIL_DB`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

`
CREATE DATABASE RETAIL_DB;
USE DATABASE RETAIL_DB;

CREATE SCHEMA RETAIL_SCHEMA;
USE SCHEMA RETAIL_SCHEMA;

CREATE TABLE RETAIL_TABLE(
transactions_id INT,
sale_date DATE,
sale_time TIME,
customer_id INT,
gender STRING,
age INT,
category STRING,
quantiy INT,
price_per_unit FLOAT,
cogs FLOAT,
total_sale FLOAT
);

--Assign primary key 

ALTER TABLE RETAIL_TABLE
ADD CONSTRAINT PK PRIMARY KEY (transactions_id);

SELECT * FROM RETAIL_TABLE
LIMIT 10;

---CHECK DATA QUALITY 
SELECT COUNT(* ) FROM RETAIL_TABLE;
SELECT COUNT(TRANSACTIONS_ID) FROM RETAIL_TABLE;

SELECT * FROM RETAIL_TABLE
WHERE PRICE_PER_UNIT IS NULL
or COGS is NULL
or TOTAL_SALE IS NULL
 ;


---DELETE NULL VALUE/  DATA CLEANING 
DELETE FROM RETAIL_TABLE
WHERE PRICE_PER_UNIT IS NULL
or COGS is NULL
or TOTAL_SALE IS NULL;

--Data Exploration
--1. how many sales we have 
SELECT 
SUM(TOTAL_SALE) 
FROM RETAIL_TABLE;

--2. HOW MANY UNIQUE CUSTOMER HAVE 
SELECT COUNT(DISTINCT CUSTOMER_ID) AS TOTAL_CUSTOMER
FROM RETAIL_TABLE ;

--3. hOW MANY CATEGORY HAVE
SELECT 
COUNT(DISTINCT CATEGORY) AS TOTAL_CATEGORY
FROM RETAIL_TABLE;

SELECT DISTINCT CATEGORY FROM RETAIL_TABLE;

--- Data Analysis & Findings
--1. Write a SQL query to retrieve all columns for sales made on '2022-11-05:

SELECT *
FROM RETAIL_TABLE
WHERE SALE_DATE = '2022-11-05';


--2.Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than equal to 4 in the month of Nov-2022:

SELECT *
FROM RETAIL_TABLE
WHERE CATEGORY = 'Clothing'
AND QUANTIY >= 4
AND to_char(SALE_DATE,'YYYY-MM') = '2022-11'
;

--3.Write a SQL query to calculate the total sales (total_sale) for each category.

SELECT CATEGORY AS ITEM_CATEGORY,
SUM(TOTAL_SALE) AS T_SALES
FROM RETAIL_TABLE
GROUP BY 1
ORDER BY 2 DESC;

--4. Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.:
SELECT
ROUND(AVG(AGE),2) AS AVG_AGE
FROM RETAIL_TABLE
WHERE CATEGORY = 'Beauty';

--5 Write a SQL query to find all transactions where the total_sale is greater than 1000.:
SELECT 
*
FROM RETAIL_TABLE
WHERE TOTAL_SALE > 1000
;

--6. Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.:

SELECT CATEGORY,
GENDER,
COUNT(TRANSACTIONS_ID)  AS TOTAL_TXN,
FROM RETAIL_TABLE
GROUP BY CATEGORY,GENDER
ORDER BY 1
;

--7. Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:
WITH CTE1 AS (SELECT
YEAR(SALE_DATE) AS YE_R,
TO_CHAR(SALE_DATE,'Mon') as MONT_NAME,
DATE_TRUNC(MONTH,SALE_DATE) AS MONTH_NAME,
AVG(TOTAL_SALE) AS AVG_S,
RANK() OVER (PARTITION BY YE_R ORDER BY AVG_S DESC) AS RN
FROM RETAIL_TABLE
GROUP BY MONT_NAME,MONTH_NAME,YE_R
ORDER BY YE_R,AVG_S)

SELECT *
FROM CTE1
WHERE RN =1
;

--8. Write a SQL query to find the top 5 customers based on the highest total sales:

SELECT 
CUSTOMER_ID,
SUM(TOTAL_SALE) AS TOTAL_SALES
FROM RETAIL_TABLE
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
;

--9. Write a SQL query to find the number of unique customers who purchased items from each category.:

SELECT
CATEGORY,
COUNT(DISTINCT(CUSTOMER_ID))  AS DIS_CUSTUMER
FROM RETAIL_TABLE
GROUP BY CATEGORY
;

--10. Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)

WITH CTE2 AS (SELECT
*,
HOUR(SALE_TIME) AS HRS,
FROM RETAIL_TABLE
)

SELECT
CASE
WHEN HRS <12 THEN 'Morning'
WHEN HRS BETWEEN 12 AND 17 THEN 'Aafternoon'
ELSE 'Evening'
end as SHIFT_TIMING,
COUNT(CUSTOMER_ID) AS TOTAL_ORDERS
FROM CTE2
GROUP BY SHIFT_TIMING
;
end queries

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## How to Use

1. 
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

