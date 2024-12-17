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
- **Record Count** - Count the number of records.
- **Null value check** - Check for any null values in the dataset.
- **Check for duplicate records** - Check for any duplicate records in the dataset.

``` sql
-- Number of Records?
SELECT COUNT(*) FROM SALES;
```
![Screenshot 2024-12-12 120439](https://github.com/user-attachments/assets/64093d63-6bfb-4159-88d4-970469f95073)
```sql
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
```
![Screenshot 2024-12-12 121003](https://github.com/user-attachments/assets/925eab92-cb08-4924-8da3-d6f2746e228f)

```sql
-- Checking for duplicates.
SELECT TRANSACTION_ID, COUNT(*) FROM SALES GROUP BY TRANSACTION_ID HAVING COUNT(*) > 1;
```
![Screenshot 2024-12-12 121449](https://github.com/user-attachments/assets/25350245-c2b3-4964-a043-3bb510c13683)

- **Exploring the columns:**

**1. transaction_id**
``` sql
SELECT COUNT(DISTINCT transaction_id) FROM sales; 
```
The transaction_id column is the Primary Key of the table 'sales'. The column contains of unique and not null values.

**2. transaction_date**

- The data is of which Year?
```sql
SELECT DISTINCT(YEAR(transaction_date)) 'YEAR' FROM SALES;  
```
![Screenshot 2024-12-12 121944](https://github.com/user-attachments/assets/fa1ba1cd-2685-4691-b297-c4906e7aa988)

- How many months of data is it?
```sql
SELECT COUNT(DISTINCT monthname(TRANSACTION_DATE)) 'NUMBER OF MONTHS' FROM SALES;
```
![Screenshot 2024-12-12 122444](https://github.com/user-attachments/assets/2d20060d-c639-4909-b735-c3ef165c9cfe)

- Which months is the data from?
```sql
SELECT DISTINCT(MONTHNAME(transaction_date)) 'MONTHS' FROM SALES;
```
![Screenshot 2024-12-12 122522](https://github.com/user-attachments/assets/32a2ea3c-91b1-4ce8-bf9a-7c936d80e207)

-- ADDING MONTH NUMBER COLUMN
ALTER TABLE SALES ADD COLUMN month_no INT;
UPDATE SALES SET month_no = MONTH(transaction_date);

-- ADDING MONTH NAME COLUMN
ALTER TABLE SALES ADD COLUMN month_name VARCHAR(15);
UPDATE SALES SET month_name = MONTHNAME(transaction_date);

-- ADDING DAY NAME COLUMN
ALTER TABLE SALES ADD COLUMN day_name VARCHAR(10);
UPDATE SALES SET day_name = DAYNAME(transaction_date);

**3. transaction_time**
- Checking for the earliest hour at which the transactions took place.
```sql
SELECT MIN(HOUR(transaction_time)) 'HOURS' FROM SALES;
```
![Screenshot 2024-12-12 123530](https://github.com/user-attachments/assets/06ceff74-45d5-42c1-a405-7eec376be685)

- Checking for the last hour.
```sql
SELECT MAX(HOUR(transaction_time)) 'HOURS' FROM SALES;
```
![Screenshot 2024-12-12 123555](https://github.com/user-attachments/assets/b49dfaca-d07c-4cb8-8934-72bfc59d521d)

**4. transaction_qty**
``` sql
SELECT DISTINCT(transaction_qty) FROM sales;
```
**5. store_id**
``` sql
SELECT DISTINCT(store_id) FROM SALES;     
``` 
- There are 3 stores of the Coffee Shop. This column contains the ID of each store.

**6. store_location**
```sql
SELECT DISTINCT(store_location) FROM SALES;  -- There are 3 different locations

SELECT DISTINCT(store_id),store_location FROM SALES;  -- STORE + LOCATION
```
![Screenshot 2024-12-12 123750](https://github.com/user-attachments/assets/14aac94f-660e-4269-9518-330313c17226)

**7. product_id**
```sql
SELECT DISTINCT(product_id),PRODUCT_DETAIL FROM SALES ORDER BY PRODUCT_ID; -- ID'S OF THE PRODUCTS THEIR NAMES
```
**8. unit_price **
```sql
SELECT AVG(UNIT_PRICE) FROM SALES;
```
**9. product_category**
```sql
SELECT DISTINCT(product_category) FROM SALES;  
```
There are 9 product categories. 

**10. product_type**
```sql
SELECT DISTINCT(product_type),PRODUCT_DETAIL FROM SALES;
```

**11. product_detail**

## Data Analysis and Business Questions
**Q1. What were the total sales and total orders for the 6 months?**
```sql
SELECT ROUND(SUM(transaction_qty * unit_price)) TOTAL_REVENUE,COUNT(*) TOTAL_ORDERS FROM SALES;
```
![Screenshot 2024-12-12 125458](https://github.com/user-attachments/assets/935731c5-0fb2-47c6-8b16-47d168a27c13)

**Q2. Store wise contribution to sales?**
```sql
SELECT STORE_ID,STORE_LOCATION,ROUND(SUM(transaction_qty * unit_price),0) TOTAL_REVENUE
FROM SALES
GROUP BY STORE_ID,STORE_LOCATION ORDER BY TOTAL_REVENUE DESC;  
```
![Screenshot 2024-12-12 125429](https://github.com/user-attachments/assets/b575cfca-dcb2-4c6c-9122-f1ddfc405e9c)

**Q3. What were the sales per month?**
```sql
SELECT MONTH_NAME,ROUND(SUM(transaction_qty * unit_price),0) MONTHLY_SALES
FROM SALES GROUP BY MONTH_NAME ORDER BY MONTHLY_SALES DESC;
```
![Screenshot 2024-12-12 125352](https://github.com/user-attachments/assets/78b60fb0-1fde-44aa-8568-01e9988beec1)

**Q4. What were the monthly sales of each store?**
```sql
SELECT 
	MONTH_NAME,
	month(TRANSACTION_DATE) AS MONTH_NUMBER,
	STORE_LOCATION,
	ROUND(SUM(transaction_qty * unit_price)) 'TOTAL SALES',
	RANK() OVER(PARTITION BY month(TRANSACTION_DATE) ORDER BY ROUND(SUM(transaction_qty * unit_price)) DESC) RNK
FROM SALES 
GROUP BY MONTH_NAME,month(TRANSACTION_DATE),STORE_LOCATION;
```
![Screenshot 2024-12-16 202555](https://github.com/user-attachments/assets/e8097a34-9dee-4cae-8cb3-4ecba84d05c8)
Hell's Kitchen generated the most revenue each month followed by Astoria and then Lower Manhattan.

**Q5. Revenue generated by each category?**
```sql
SELECT
	PRODUCT_CATEGORY,
	ROUND(SUM(TRANSACTION_QTY * UNIT_PRICE)) 'SALES'
FROM SALES GROUP BY PRODUCT_CATEGORY
ORDER BY ROUND(SUM(TRANSACTION_QTY * UNIT_PRICE)) DESC;
```
![Screenshot 2024-12-12 125316](https://github.com/user-attachments/assets/1f8407b6-d55e-4c17-876b-6a79a3ec6664)


**Q6. What were overall top 5 Best Selling Products on the basis of sales volume?**
```sql
SELECT PRODUCT_CATEGORY,PRODUCT_TYPE,PRODUCT_NAME,SUM(TRANSACTION_QTY) AS 'TOTAL QTY SOLD'
FROM SALES GROUP BY PRODUCT_CATEGORY,PRODUCT_TYPE,PRODUCT_NAME 
ORDER BY SUM(TRANSACTION_QTY) DESC LIMIT 5;
```

**Q7.What were overall top 5 Best Selling Products on the basis of Revenue?**
```sql
SELECT PRODUCT_CATEGORY,PRODUCT_TYPE,PRODUCT_NAME,ROUND(SUM(TRANSACTION_QTY*UNIT_PRICE)) AS 'TOTAL SALES' 
FROM SALES GROUP BY PRODUCT_CATEGORY,PRODUCT_TYPE,PRODUCT_NAME 
ORDER BY ROUND(SUM(TRANSACTION_QTY*UNIT_PRICE)) DESC LIMIT 5;
```
Q8. Which days of the week were busy? (Week days or Weekends?)
```sql
SELECT 
    day_name,
    hours,
    COUNT(TRANSACTION_ID) AS 'NO. OF TRANSACTIONS',
    RANK() OVER(PARTITION BY DAY_NAME ORDER BY COUNT(TRANSACTION_ID) DESC) RNK
FROM SALES 
GROUP BY DAY_NAME,hours
HAVING DAY_NAME IN ('Saturday','Sunday');
```

