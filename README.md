# FoodYum-Grocery-Store-Sales
Analyzed FoodYum grocery store sales data to clean, transform, and standardize product records, ensuring accurate pricing, stock, and sales trends across categories, helping the company make data-driven inventory and pricing decisions.
<br>
<br>
## PROJECT OVERVIEW
FoodYum is a U.S.-based grocery store chain selling produce, meat, dairy, baked goods, snacks, and household food staples. Rising food costs and a diverse customer base make it important to maintain accurate product records.
This project focused on data cleaning and transformation, preparing the dataset for further analysis. The products table contained details such as product type, brand, weight, price, average units sold, year added, and stock location. Cleaning this data ensures reliable insights, allows accurate pricing and demand analysis, and helps FoodYum manage inventory effectively.

<img width="750" height="375" alt="image" src="https://github.com/user-attachments/assets/b0924291-7431-4158-862f-14c567781429" />

<br>
<br>

## OBJECTIVE / KEY QUESTIONS

* How many products have missing year_added values, and how should they be corrected?
* How can inconsistencies in brand, stock location, and weight be standardized?
* What are the price ranges for each product category?
* Which high-demand products (average_units_sold > 10) exist in the Meat and Dairy categories?
<br>

## DATASET

* Source: FoodYum internal sales database
* Table: `products`
  
  | Column Name              | Description                                             |
  | ------------------------ | ------------------------------------------------------- |
  | **product\_id**          | Unique identifier of the product                        |
  | **product\_type**        | Product category: Produce, Meat, Dairy, Bakery, Snacks  |
  | **brand**                | Product brand name                                      |
  | **weight**               | Weight in grams (may contain some text inconsistencies) |
  | **price**                | Price in USD                                            |
  | **average\_units\_sold** | Average number of units sold per month                  |
  | **year\_added**          | Year the product was added to stock                     |
  | **stock\_location**      | Warehouse location: A, B, C, D                          |
<br>

## TOOLS AND SKILLS USED:

* SQL PostgreSQL
* Concepts: CTEs, `COALESCE`, `NULLIF`, `CASE` statements, `TRIM`, `UPPER`, `REPLACE`, `PERCENTILE_CONT`, `ROUND`, `GROUP BY`, `MIN`, `MAX`, `COUNT`, `DISTINCT`
* Techniques Used: Data cleaning, data transformation, handling NULLs, standardization of text fields, aggregation, descriptive statistics, exploration of column distributions.
<br>

## ANALYSIS & APPROACH:
### 1. Data Exploration: Understanding the Table

```sql
--- Overview of products table

SELECT *
FROM products
LIMIT 10;
```
We started by looking at the first 10 rows of the products table to get a general sense of the data. This helped us see the structure, types of values, and any obvious inconsistencies in the dataset before diving deeper.


### 2. Check product_id for NULLs

```sql
--- Exploring product_id column for NULLs, empty strings, missing, or inconsistent data

SELECT product_id
FROM products
WHERE product_id IS NULL;
```
We verified whether product_id had any missing values. Since this is the primary key, it must not contain NULLs. This check confirmed data integrity for unique identification.

### 3. Explore product_type

```sql
--- Exploring product_type column for NULLs, empty strings, missing, or inconsistent data

SELECT DISTINCT product_type
FROM products;
```
Checked all unique values of product_type to identify missing, empty, or inconsistent entries. This ensured that all product categories are correct and match the expected values: Produce, Meat, Dairy, Bakery, and Snacks.

### 4. Explore brand

```sql
--- Exploring brand column for NULLs, empty strings, missing, or inconsistent data

SELECT DISTINCT brand
FROM products;
```
We checked for unique brands to spot missing values (NULL), placeholder characters (-), capitalization, or spacing issues. This was important for later analysis to avoid counting the same brand multiple times incorrectly.

### 5. Explore the weight column

```sql
--- Exploring the weight column for NULLs, empty strings, missing, or inconsistent data

SELECT weight
FROM products
WHERE weight IS NULL;
```
Checked for missing or NULL weights. We also noticed some rows had text like 'grams' instead of numeric values. This would need transformation before analysis.

### 6. Explore the price column

```sql
--- Exploring price column for NULLs, empty strings, missing, or inconsistent data

SELECT price
FROM products
WHERE price IS NULL;
```
Validated whether any price values are missing. Since price is crucial for analysis, ensuring no NULLs exist was necessary.

### 7. Price statistics

```sql
--- Looking for Max, Min, and Avg prices in our dataset

SELECT 
	AVG(price),
	MAX(price),
	MIN(price)
FROM products;
```
Calculated average, maximum, and minimum prices to understand the pricing range for all products. This helps identify outliers or unusual pricing.

### 8. Explore average_units_sold column

```sql
--- Exploring the average_units_sold column for NULLs, empty strings, missing, or inconsistent data

SELECT average_units_sold
FROM products
WHERE average_units_sold IS NULL;
```
Checked for missing values in the average_units_sold column, which is important to evaluate sales trends.

### 9. Average_units_sold statistics

```sql
--- Looking for Max, Min, and Average average_units_sold Sold in our dataset

SELECT 
	MAX(average_units_sold),
	MIN(average_units_sold)
FROM products;
```
Determined the range of units sold to understand product popularity and spot potential outliers.

### 10. Explore year_added

```sql
--- Exploring the year_added column for NULLs, empty strings, missing, or inconsistent data

SELECT DISTINCT year_added
FROM products;
```
Reviewed all unique years products were added to identify missing values. Some rows were NULL due to a system bug in 2022.

### 11. Explore stock_location

```sql
--- Exploring the stock location column for NULLs, empty strings, missing, or inconsistent data

SELECT DISTINCT stock_location
FROM products;
```
Checked all unique warehouse locations. Ensured capitalization consistency (A, B, C, D) and flagged invalid or missing values.

### 12. Check table structure

```sql
--- Understanding the table structure and data types of all the columns

SELECT 
    column_name, 
    data_type
FROM information_schema.columns
WHERE table_name = 'products';
```
Checked the data types of all columns to confirm they match expectations and to identify any necessary type conversions during cleaning.

Exploring the sample rows and data types helped identify missing, inconsistent, or malformed values. Understanding the dataset structure ensured cleaning steps targeted the correct columns, enabling accurate analysis without errors. It also helps isolate all missing, inconsistent, or incorrectly formatted values. It was critical to understand the data quality before applying transformations, ensuring accurate insights later.

### 13. 
















