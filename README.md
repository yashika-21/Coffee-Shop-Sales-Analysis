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
Explored all the columns of the dataset to ensure data accuracy and consistency. The following questions were asked and subsequent queries were formulated.
