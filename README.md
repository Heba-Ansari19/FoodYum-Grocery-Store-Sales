# FoodYum-Grocery-Store-Sales-
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
Checked all unique values of product_type to identify missing, empty, or inconsistent entries. This ensured that all product categories are correct and match the expected values: Produce, Meat, Dairy, Bakery, Snacks.

### 4. Explore brand

```sql
--- Exploring brand column for NULLs, empty strings, missing, or inconsistent data

SELECT DISTINCT brand
FROM products;
```
We checked for unique brands to spot missing values (NULL), placeholder characters (-), capitalization, or spacing issues. This was important for later analysis to avoid counting the same brand multiple times incorrectly.





























