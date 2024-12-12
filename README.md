# Project Overview


# Project Structure

## 1. Database Setup
   * **Database Creation:** The project starts by creating a database named coffee_shop.
   * **Table Creation:** In the database a table named 'sales' is created to store the sales data. The table structure includes columns for Transaction Id, transaction date, transaction time, transaction qty, store id, store location, product id, unit price, product category, product type, and product detail.

``` sql
-- Creating the database.
CREATE DATABASE COFFEE_SHOP;
USE COFFEE_SHOP;

-- Creating the Table sales Schema/Structure.
CREATE TABLE sales (
	transaction_id INT PRIMARY KEY,
    	transaction_date DATE,
    	transaction_time TIME,
    	transaction_qty INT,
    	store_id INT,
    	store_location VARCHAR(20),	
    	product_id INT,
    	unit_price FLOAT,
    	product_category VARCHAR(25),	
	product_type VARCHAR(25),
    	product_detail VARCHAR(30)
);
```
## 2. Import the data into the table 'sales'.
The data was imported using command line.

## 3. Data Exploration and Pre-Processing.
Explored all the columns of the dataset to ensure data accuracy and consistency.
- ** Record Count** - Count the number of records.
- **Null value check** - Check for any null values in the dataset.
- **Check for duplicate records** - Check for any duplicate records in the dataset.

``` sql
-- Number of Records?
SELECT COUNT(*) FROM SALES;
```
![Screenshot 2024-12-12 120439](https://github.com/user-attachments/assets/64093d63-6bfb-4159-88d4-970469f95073)
```
-- Checking for null values.
SELECT * FROM SALES
WHERE 
transaction_id IS NULL
OR 
transaction_date IS NULL
OR 
transaction_qty IS NULL
OR 
store_id IS NULL
OR 
store_location IS NULL 
OR 
product_id IS NULL 
OR 
unit_price IS NULL
OR
product_category IS NULL 
OR 
product_type IS NULL
OR 
product_detail IS NULL;

-- Checking for duplicates.
SELECT TRANSACTION_ID, COUNT(*) FROM SALES GROUP BY TRANSACTION_ID HAVING COUNT(*) > 1;
```
- **Exploring the columns:**

1. transaction_id
``` sql
SELECT COUNT(DISTINCT transaction_id) FROM sales; 
```
2. transaction_date
``` sql
-- Which year data is from?
SELECT DISTINCT(YEAR(transaction_date)) 'YEAR' FROM SALES;  -- data is of Year 2023

-- How many months of data is it?
SELECT DISTINCT(MONTHNAME(transaction_date)) 'MONTHS' FROM SALES; -- 6 MONTHS JANUARY TO JUNE

-- ADDING MONTH NUMBER COLUMN
ALTER TABLE SALES ADD COLUMN month_no INT;
UPDATE SALES SET month_no = MONTH(transaction_date);

-- ADDING MONTH NAME COLUMN
ALTER TABLE SALES ADD COLUMN month_name VARCHAR(15);
UPDATE SALES SET month_name = MONTHNAME(transaction_date);

-- ADDING DAY NAME COLUMN
ALTER TABLE SALES ADD COLUMN day_name VARCHAR(10);
UPDATE SALES SET day_name = DAYNAME(transaction_date);
```

3. transaction_time
``` sql
-- Checking for the earliest hour at which the transactions took place.
SELECT MIN(HOUR(transaction_time)) 'HOURS' FROM SALES;

-- Checking for the last hour.
SELECT MAX(HOUR(transaction_time)) 'HOURS' FROM SALES;
```
4. transaction_qty
``` sql
SELECT DISTINCT(transaction_qty) FROM sales;
```
5. store_id
``` sql
SELECT DISTINCT(store_id) FROM SALES;    -- There are 3 stores 
``` 

