# FoodYum-Grocery-Store-Sales

Analyzed **FoodYum grocery store sales data** to clean, transform, and standardize product records for accurate business insights. Addressed missing values, inconsistent `brand` names, text in `weight` fields, and `stock_location` variations. Created a fully reliable dataset to analyze pricing trends, category performance, and high-demand products. Enabled informed inventory planning, pricing strategies, and targeted promotions to optimize sales and customer satisfaction.
<br>
<br>
## PROJECT OVERVIEW

FoodYum is a U.S.-based grocery store chain selling produce, meat, dairy, baked goods, snacks, and household food staples. Rising food costs and a diverse customer base make it important to maintain accurate product records.
This project focused on **data cleaning and transformation, preparing the dataset for further analysis**. The products table contained details such as `product_type`, `brand`, `weight`, `price`, `average_units_sold`, `year_added`, and `stock_location`. Cleaning this data ensures reliable insights, allows accurate pricing and demand analysis, and helps FoodYum manage inventory effectively.

<img width="750" height="375" alt="image" src="https://github.com/user-attachments/assets/b0924291-7431-4158-862f-14c567781429" />

<br>
<br>

## OBJECTIVE / KEY QUESTIONS

* How many products have missing year_added values, and how should they be corrected?
* How can inconsistencies in brand, stock location, and weight be standardized?
* What are the price ranges for each product category?
* Which high-demand products (`average_units_sold` > 10) exist in the Meat and Dairy categories?
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

## TOOLS AND SKILLS USED

* [PostgreSQL](https://www.postgresql.org/download/)
* Concepts: CTEs, `COALESCE`, `NULLIF`, `CASE` statements, `TRIM`, `UPPER`, `REPLACE`, `PERCENTILE_CONT`, `ROUND`, `GROUP BY`, `MIN`, `MAX`, `COUNT`, `DISTINCT`
* Techniques Used: Data cleaning, data transformation, handling NULLs, standardization of text fields, aggregation, descriptive statistics, exploration of column distributions.
<br>

## ANALYSIS & APPROACH
### 1. Data Exploration: Understanding the Table

```sql
--- Overview of products table

SELECT *
FROM products
LIMIT 10;
```
We started by looking at the first 10 rows of the products table to get a **general sense of the data**. This helped us see the structure, types of values, and any obvious inconsistencies in the dataset before diving deeper.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/c042775f-33ba-4845-ba5a-8d69da4250d1" />


### 2. Check product_id for NULLs

```sql
--- Exploring product_id column for NULLs, empty strings, missing, or inconsistent data

SELECT product_id
FROM products
WHERE product_id IS NULL;
```
We verified whether product_id had any missing values. Since this is the **primary key, it must not contain NULLs**. This check confirmed data integrity for unique identification.


### 3. Explore product_type

```sql
--- Exploring product_type column for NULLs, empty strings, missing, or inconsistent data

SELECT DISTINCT product_type
FROM products;
```
Checked all unique values of product_type to **identify missing, empty, or inconsistent entries**. This ensured that all product categories are correct and match the expected values: Produce, Meat, Dairy, Bakery, and Snacks.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/023baa54-b285-4560-bce7-2a9eec3ef9bc" />


### 4. Explore brand

```sql
--- Exploring brand column for NULLs, empty strings, missing, or inconsistent data

SELECT DISTINCT brand
FROM products;
```
Checked for unique brands to spot **missing values (NULL), placeholder characters (-), capitalization, or spacing issues**. This was important for later analysis to avoid counting the same brand multiple times incorrectly.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/b568f05b-503c-40f7-877f-65aed66a0f87" />


### 5. Explore the weight column

```sql
--- Exploring the weight column for NULLs, empty strings, missing, or inconsistent data

SELECT weight
FROM products
WHERE weight IS NULL;
```
Checked for **missing or NULL weights**. We also noticed some rows had **text like 'grams' instead of numeric values**. This would need transformation before analysis.


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
Calculated average, maximum, and minimum prices to **understand the pricing range** for all products. This helps identify outliers or unusual pricing.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/27ba697a-215a-47f0-bdfa-7bfeeda49ef3" />


### 8. Explore average_units_sold column

```sql
--- Exploring the average_units_sold column for NULLs, empty strings, missing, or inconsistent data

SELECT average_units_sold
FROM products
WHERE average_units_sold IS NULL;
```
Checked for **missing values** in the average_units_sold column, which is important to evaluate sales trends.

### 9. Average_units_sold statistics

```sql
--- Looking for Max, Min, and Average average_units_sold Sold in our dataset

SELECT 
	MAX(average_units_sold),
	MIN(average_units_sold)
FROM products;
```
Determined the range of units sold to **understand product popularity and spot potential outliers**.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/9ab06cb9-c2ff-47da-8175-3b2d897bc32e" />


### 10. Explore year_added

```sql
--- Exploring the year_added column for NULLs, empty strings, missing, or inconsistent data

SELECT DISTINCT year_added
FROM products;
```
Reviewed all unique years products were added to **identify missing values**. Some rows were NULL due to a system bug in 2022.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/6b87abb8-4a76-405d-82ce-a4ef1022e629" />


### 11. Explore stock_location

```sql
--- Exploring the stock location column for NULLs, empty strings, missing, or inconsistent data

SELECT DISTINCT stock_location
FROM products;
```
Checked all **unique warehouse locations**. Ensured capitalization consistency **(A, B, C, D)** and flagged invalid or missing values.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/47581a2a-cb88-429f-bb7f-39010171a9e3" />


### 12. Check the table structure

```sql
--- Understanding the table structure and data types of all the columns

SELECT 
    column_name, 
    data_type
FROM information_schema.columns
WHERE table_name = 'products';
```
Checked the **data types of all columns** to confirm they match expectations and to identify any necessary type conversions during cleaning.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/0080edc2-bb75-4f10-9d2a-429be901d575" />



After thoroughly exploring each column of the products table, we observed the following:

* `product_id`: This column was perfect with **no missing values or data type errors**, providing a strong, unique identifier for each product, which ensures every record can be accurately tracked. **No cleaning was needed here.**
* `product_type`: This column also had **no missing values or data type errors**, confirming that all products are correctly categorized into Produce, Meat, Dairy, Bakery, or Snacks. This allowed us to confidently group and analyze products by category.
* `brand`: Several entries had **missing values represented as '-', inconsistent capitalization, and extra spaces**. To make this reliable, we **standardized** all brand names by **trimming spaces, converting to proper capitalization, and replacing missing or invalid entries with 'Unknown'**.
* `weight`: While there were **no missing values**, some entries included the **text 'grams', causing data type issues**. We cleaned this column by **removing text, converting all values to numeric format, and rounding consistently**, enabling accurate calculations for inventory and sales analysis.
* `price`: This column was **complete and correctly formatted**, so **no changes were required**, ensuring reliable pricing data for analysis.
* `average_units_sold`: The column had **no missing or incorrect values**, which allows precise identification of high-demand products.
* `year_added`: Some **missing entries alongside years from 2015–2022**. Replacing missing values with **2022** ensures consistency and allows accurate analysis of product age, sales trends, and pricing patterns.
* `stock_location`: Some records had **inconsistent capitalization** across the four warehouse locations (A, B, C, D). **Normalizing the text and replacing any invalid entries with 'Unknown'** ensures reliable location-based aggregation.

By addressing these issues—replacing missing values, standardizing text, and correcting formats—we created a fully clean and reliable dataset. This enables **accurate analysis of pricing trends, category performance, high-demand products, and inventory planning, providing a strong foundation for business decision-making.**


### 13. Check for Missing year_added Values

```sql
SELECT
	COUNT(*) AS missing_year
FROM products
WHERE year_added IS NULL;
```
I examined the `year_added column` to understand when each product was first stocked at FoodYum, because this information directly affects pricing trends, inventory planning, and sales analysis. In 2022, a system bug caused some products to have **missing `year_added` values***, which could have **led to inaccurate insights if not addressed**. Using this query, I counted all the missing entries, which allowed me to clearly see the scope of the issue. With this knowledge, I implemented a cleaning strategy by **replacing the missing values with 2022**, ensuring that future analyses of product age, category growth, and pricing trends would be accurate and reliable.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/521c1090-35e5-4ce7-9526-007c99bccda8" />


### 14. Cleaning and transforming data

```sql
--- Creating a CTE to replace NULLS in weight with the Median value
WITH weight_median AS(
	SELECT
		PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY REPLACE(weight, ' grams', '')::NUMERIC) AS median_weight
	FROM products
	WHERE weight IS NOT NULL
),
	
--- Creating a CTE to replace NULLS in price with the Median value
price_median AS (
	SELECT
		PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY price :: NUMERIC) AS median_price
	FROM products
	WHERE price IS NOT NULL
)	

----------------------------------------------------------------------------------------------------------------
	
SELECT

	--- NO CHANGES
	product_id :: INTEGER,

	--- NO CHANGES
	COALESCE(TRIM(product_type) :: TEXT, 'Unknown') AS product_type,

	--- Replace NULL and '-' in brands with 'Unknown'
	COALESCE(NULLIF(TRIM(brand), '-'), 'Unknown') AS brand,

	--- Replace NULLS with 'Median' from CTE, Cast type Numeric, round to 2 decimal places
	ROUND(
		COALESCE(
			REPLACE(weight, ' grams', '') :: NUMERIC, 
			(SELECT median_weight FROM weight_median)
		) :: NUMERIC, 2) AS weight,
	
	--- Replace NULLS with 'Median' from CTE, Cast type Numeric, round to 2 decimal places
	ROUND(COALESCE(price, (SELECT median_price FROM price_median)) :: NUMERIC, 2) AS price,

	--- Replace NULLS with '0' and cast type Integer
	COALESCE(average_units_sold :: INTEGER, 0) As average_units_sold,

	--- Replace NULLS with the recent year '2022' and cast type to Integer
	COALESCE(
    	CASE 
	        WHEN year_added :: TEXT ~ '^\d+$' THEN year_added::INTEGER
	        ELSE NULL
	    END, 
	2022) AS year_added,

	--- Normalized capitalization and replaced invalid or NULLS with 'Unknown'
	CASE 
		WHEN UPPER(stock_location) IN ('A', 'B', 'C', 'D') THEN UPPER(stock_location)
		ELSE 'Unknown'
	END AS stock_location
	
FROM products;
```
I cleaned the dataset thoroughly to make it ready for reliable analysis. First, I **replaced NULLs with appropriate values such as medians, defaults, or “Unknown”, which ensured that no record was left incomplete or unusable**. Next, I **standardized key columns like `weight`, `brand`, and `stock_location`** so that inconsistent entries (like different spellings, missing units, or lowercase/uppercase mismatches) would not break aggregations or groupings later on. To make sure each transformation was correct, I tested them individually through CTEs before combining everything into a single clean dataset. This **cleaning process was critical** because even small inconsistencies could have produced **misleading insights**—for example, `price` trends skewed by missing values or stock performance hidden due to inconsistent warehouse names. By carefully preparing the dataset, I created a strong foundation for the analysis, which allowed me to **confidently explore pricing patterns, track category-level performance, and identify high-demand products without worrying about data quality issues**.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/804d64e3-7b20-453a-827b-4ac5806cb0d3" />


### 15. Analyze price ranges per product type

```sql
SELECT
	product_type,
	MIN(price) AS min_price,
	MAX(price) AS max_price
FROM products
GROUP BY product_type;
```
I analyzed the minimum and maximum `prices` for each product category to understand the **full pricing range** within FoodYum’s inventory. By identifying the cheapest and most expensive items in categories like Produce, Meat, Dairy, Bakery, and Snacks, I could clearly see **which products appealed to budget-conscious customers and which were premium options**. This detailed understanding of price distribution allowed me to assess whether each category offered a balanced range of products, **helping the store make informed decisions about stocking, pricing strategies, and meeting the needs of a broad customer base.**

<img width="800" alt="image" src="https://github.com/user-attachments/assets/d87fbfbc-3292-4c8b-9058-6a47f02bf7e5" />


### 16. High-demand Meat and Dairy products

```sql
SELECT
	product_id,
	price,
	average_units_sold
FROM products
WHERE product_type IN ('Meat', 'Dairy') AND average_units_sold > 10;
```
I analyzed products with high average monthly sales, focusing on items in categories like Meat and Dairy, where the average units sold exceeded ten. By identifying these high-demand products, I could highlight which **items consistently sell faster and generate more revenue**. This information allowed FoodYum to **prioritize inventory planning, ensuring that popular products were always in stock, and to design targeted promotions to boost sales further.** Understanding which products move quickly also helped **optimize stock turnover, reduce waste, and improve overall profitability.**

<img width="800" alt="image" src="https://github.com/user-attachments/assets/848e9844-5b14-426f-a5e8-7e31fc6964dc" />


<br>
<br>

## INSIGHTS AND FINDINGS

* **170 products** had **missing `year_added`**; filling with **2022** ensured consistent timelines for pricing and sales analysis.
* **`brand` names had inconsistencies**; normalization enabled accurate aggregation and reporting.
* The `weight` column required **cleaning due to text entries**; standardization allowed numeric comparisons and calculations.
* **`price` ranges varied across categories**: Meat ($11.48–$16.98) and Dairy ($8.33–$13.97) were premium, Produce ($3.46–$8.78) and Snacks ($5.20–$10.72) covered budget options.
* **698 Meat and Dairy products had `average_units_sold` > 10**, highlighting high-demand items for inventory focus.
<br>

## RECOMMENDATIONS

* Prioritize **stocking and promoting high-demand Meat and Dairy products** to optimize revenue and turnover.
* Maintain **`price` diversity in all categories** to cater to a broad customer base.
* Use the **cleaned dataset** for trend analysis, predictive modeling, and better inventory decisions.
* Regularly **monitor and standardize `brand` and `stock_location`** data to prevent future inconsistencies.
* Combine insights from `price`, `weight`, and sales to **guide procurement, promotions, and warehouse planning effectively.**
<br>

## FINAL NOTES

This project demonstrated how SQL can **transform raw, inconsistent product data into actionable business insights**. By systematically cleaning and standardizing the dataset, FoodYum can make informed decisions on **pricing, inventory, and stock allocation**. These insights enable the company to **meet customer demand effectively, optimize revenue, and ensure profitability**. Future extensions could include predictive sales modeling or seasonal demand analysis to further refine inventory and promotional strategies.
<br>
<br>
## REFERENCE

* [DataCamp](https://app.datacamp.com/)








