# Product-Financial-Insights


### Table of contents
- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Stages](#stages)
- [Design](#design)
  - [Mockup](#mockup)
  - [Tools](#tools)
- [Development](#development)
  - [Pseudocode](#pseudocode)
  - [Data Exploration](#data-exploration-notes)
  - [Data Cleaning](#data-cleaning)
- [Testing](#testing)
- [Visualization](#visualization)
  - [Visuals](#power-bi-visuals-used)
  - [DAX Measures](#dax-measures)
- [Analysis](#analysis)
  - [Key Performance Indicators (KPIs)](#key-performance-indicators-kpis)
  - [Sales Breakdown by Year and Quarter](#sales-breakdown-by-year-and-quarter)
  - [Sales by Country](#sales-by-country)
  - [Sales by Product and COGS Analysis](#sales-by-product-and-cogs-analysis)
- [Recommendation](#business-insights-and-recommendations)



### Project Goals
- Analyze sales data to uncover key business insights
- Develop a Power Bi dashboard with interative visualization.
- Optimize reporting using DAX Measures for KPI's
- Provide documentation for easy replication and understanding on Github

### Project Execution Steps
1. Data Understanding & Cleaning: Clean and prepare the sales dataset.
2. Load Data into Power BI: Import the cleaned dataset into Power BI.
3. Create Power BI Dashboard: Develop a dashboard with KPIs, DAX measures, and interactive visualizations.
4. Business Insights & Findings: Analyze the data to identify top-selling products, best-performing countries, seasonal trends, and profitability analysis.



# 📜Project Overview

This project provides a comprehensive sales performance analysis using Power BI. The interactive dashboard helps business stakeholders understand total sales, profit, discount impact, and sales distribution by country, product, and time period.

📊 Key Features
- ✔ Sales, Profit, and Performance Analysis
- ✔ Dynamic Power BI Dashboard
- ✔ Custom DAX Measures for Advanced Analytics
- ✔ Interactive Visualizations


## Data Source
The data is sourced from an Excel extract, sales data to uncover key business insights collected for analysis.


# Stages
- Design
- Developement
- Testing
- Analysis


# Design

### Mockup

- The visualizations will include:
1. Slicers for date, month and country insights
2. Treemaps for high-level comparisons
3. Scorecards for quick KPI insights
4. Bar Charts to compare performance metrics
5. Line charts category trends
6. Funnel for quater comparison
7. Donut Chart for monthly insights
![Detailed-view_mockup](https://github.com/user-attachments/assets/5feed18a-bef3-44b8-84c4-67c50f3f2117)
![dashboard_mockup](https://github.com/user-attachments/assets/1523695e-e032-40af-979d-f07860b33dee)


# Tools

|Tool|	Purpose|
|-|-|
|Excel|	Exploring the data|
|Power BI	|Visualizing the data via interactive dashboards|
|GitHub|	Hosting the project documentation and version control|
|Mokkup AI	|Designing the wireframe/mockup of the dashboard|

# Development

### Pseudocode
- What's the general approach in creating this solution from start to finish?
1. Load the data into Excel for an initial scan.
2. Perform necessary transformations (removing nulls, renaming columns, formatting data types).
3. Load the cleaned dataset into Power BI for visualization.
4. Apply DAX measures to compute engagement rates and other metrics.
5. Publish insights and document findings on GitHub.


### Data exploration notes
This is the stage where you have a scan of what's in the data, errors, inconcsistencies, bugs, weird and corrupted characters etc

- What are your initial observations with this dataset? What's caught your attention so far?
1. There are at least 13 columns that contain the data we need for this analysis, which signals we have everything we need from the file without needing to contact the client for any more data.
2. After the proper check, the raw total rows in the datasets are 712(rows), we remove the empty rows of the datasets, and we have 706 rows left in the datsets (that is 6 blank rows are removed.)

3. In Column A (Country) All empty spacing removed, and Rename some of the Country abbreviation etc UK to United Kingdom. ( That is 141 rows UK to United Kingdom were replaced).
4. In column B (Product), some of the text are in Proper and Uppercase. Proper case fixed and Product trim to remove spacing in the column B
5. 4 Duplicates found, 706 Rows in datasets, were reduced to 702 rows.



## Data cleaning
The cleaned dataset should meet these conditions:

- All columns are retained.
- No missing values in key performance fields.
- Appropriate data types assigned to each column.


## Transform the data
```sql
/*
- CLEANING STEPS

1. REMOVE DUPLICATES IN THE TABLE
2. LOCATE AND DELETE NULLS IN THE TABLE 
3. DROP THE UNNECCESSARY COLUMN 
4. REPLACE THE ABBREVIATIONS
5. TRIM ALL THE COLUMNS IN THE TABLE 
6. NORMALIZE DATA
7. TO CHECK THE DATA TYPE OF DATE FORMAT
8. CONVERT INTEGER TO DATE IN A QUERY (WITHOUT CHANGING COLUMN TYPE)
9. FORMAT THE DATE COLUMN (Add a new date column, )
10.REPLACE ALL NULL VALUE IN A DISCOUNT WITH O
*/

-- 1. Remove duplicate rows in the table

	WITH CTE AS (
		SELECT *,
	ROW_NUMBER()
	OVER (PARTITION BY [Country]
	,[Product]
		,[Units_Sold]
	,[Manufacturing_Price]
		,[Sale_Price]
	,[Gross_Sales]
		,[Discounts]
	,[Sales]
		,[COGS]
	,[Profit]
		,[Date]
	,[Month_Name]
		,[Year]
	,[column14] ORDER BY (SELECT NULL)) AS rn
		FROM sales_data	)
	DELETE FROM CTE WHERE rn > 1;

	SELECT
	*
	FROM sales_data;

--2.  LOCATE NULLS FROM THE TABLE

	SELECT * 
		FROM sales_data
	WHERE Country IS NULL 
		AND Product IS NULL 
	AND Units_Sold IS NULL 
		AND Manufacturing_Price IS NULL 
	AND Sale_Price IS NULL 
		AND Gross_Sales IS NULL 
	AND Discounts IS NULL 
		AND Sales IS NULL 
	AND COGS IS NULL 
		AND Profit IS NULL 
	AND Date IS NULL 
		AND Month_Name IS NULL 
	AND Year IS NULL 
		AND column14 IS NULL;



--	DELETE NULLS FROM THE TABLE

	DELETE FROM sales_data
		WHERE Country IS NULL 
	AND Product IS NULL 
		AND Units_Sold IS NULL 
	AND Manufacturing_Price IS NULL 
		AND Sale_Price IS NULL 
	AND Gross_Sales IS NULL 
		AND Discounts IS NULL
	AND Sales IS NULL 
		AND COGS IS NULL 
	AND Profit IS NULL 
		AND Date IS NULL 
	AND Month_Name IS NULL 
		AND Year IS NULL 
	AND column14 IS NULL;

-- 3.	DROP THE UNNECCESSARY COLUMN 

	ALTER TABLE sales_data  
		DROP 
	COLUMN column14;

-- 4.	SELECT ALL THE ABBREVIATIONS

	SELECT * FROM sales_data 
		WHERE CONCAT_WS(' ', Country, Product, Units_Sold, Manufacturing_Price, Sale_Price, Gross_Sales, 
         Discounts, Sales, COGS, Profit, Date, Month_Name, 
		Year) LIKE '%UK%';

--	REPLACE ALL THE ABBREVIATIONS

	UPDATE sales_data
		SET Country = 
	REPLACE(Country, 'UK', 'United Kingdom'),
		Product = REPLACE(Product, 'UK', 'United Kingdom'),
	Units_Sold = REPLACE(Units_Sold, 'UK', 'United Kingdom'),
		Manufacturing_Price = REPLACE(Manufacturing_Price, 'UK', 'United Kingdom'),
	Sale_Price = REPLACE(Sale_Price, 'UK', 'United Kingdom'),
		Gross_Sales = REPLACE(Gross_Sales, 'UK', 'United Kingdom'),
	Discounts = REPLACE(Discounts, 'UK', 'United Kingdom'),
	    Sales = REPLACE(Sales, 'UK', 'United Kingdom'),
	COGS = REPLACE(COGS, 'UK', 'United Kingdom'),
		Profit = REPLACE(Profit, 'UK', 'United Kingdom'),
	Date = REPLACE(Date, 'UK', 'United Kingdom'),
		Year = REPLACE(Year, 'UK', 'United Kingdom');


-- 5.	TRIM ALL THE COLUMNS IN THE TABLE 

	DECLARE @sql NVARCHAR(MAX) = '';

	SELECT @sql = @sql + 
		'UPDATE sales_data SET ' + 
	STRING_AGG(CONCAT(name, ' = 
		LTRIM(RTRIM(', name, '))'), ', ') + ';'
	FROM sys.columns
		WHERE object_id = OBJECT_ID('sales_data') 
	AND system_type_id IN (167, 231, 175); -- Only text-based columns (VARCHAR, NVARCHAR, CHAR)

	EXEC sp_executesql @sql;


-- 6.	NORMALIZE DATA

	UPDATE sales_data
		SET Product = UPPER(LEFT(Product, 1)) + 
	LOWER(SUBSTRING(Product, 2, LEN(Product)));



--	TO CHECK THE DATA TYPE OF DATE FORMAT

	SELECT COLUMN_NAME, DATA_TYPE 
	FROM INFORMATION_SCHEMA.COLUMNS 
	WHERE TABLE_NAME = 'sales_data' AND 
		COLUMN_NAME = 'Date';


--	CONVERT INTEGER TO DATE IN A QUERY (WITHOUT CHANGING COLUMN TYPE)


	SELECT Date, DATEADD(DAY, Date - 2, '1900-01-01') 
	AS Converted_Date
	FROM sales_data;


--	Add A NEW DATE COLUMN 

	ALTER TABLE sales_data 
	ADD Date_Converted DATE;
		

--	CONVERT AND POPULATE THE NEW COLUMN
		
	UPDATE sales_data
	SET Date_Converted = 
	DATEADD(DAY, Date - 2, '1900-01-01');


--	REMOVE THE OLD COLUMN AND RENAME THE NEW ONE 

	ALTER TABLE sales_data 
	DROP COLUMN Date;
	EXEC sp_rename 'sales_data.Date_Converted', 'Date', 'COLUMN';


-- 10.	REPLACE ALL NULL VALUE IN A DISCOUNT WITH O

	UPDATE sales_data
	SET Discounts = 0
	WHERE Discounts IS NULL;


	SELECT * FROM sales_data


--	TO REARRANGE sale_data tables (Date before Year)

	SELECT Country, Product, Units_Sold, Manufacturing_Price, Sale_Price, Gross_Sales, Discounts, Sales, COGS, Profit, Month_Name, Date, Year -- Arrange as needed
	INTO sales_data_new
	FROM sales_data;


--	Drop Old Sales_data table
	DROP TABLE sales_data;

--	Rename the New Table

	EXEC sp_rename 'sales_data_new', 'sales_data';


	Select * from sales_data;

```




# Testing

## Row Count Validation

### SQL Query

```sql
/*
1. Count the total number of records (or rows) are in the sales table
*/

-- 1.	Count the total number of records (or rows) are in the sales table
	SELECT 
    COUNT(*)
	AS 
	no_of_rows
	FROM 
	sales_data;

```
### Output
![1  Row_count (testing)](https://github.com/user-attachments/assets/439acd47-5465-4986-8960-221b2a659bef)


## Column Count Validation

### SQL Query
```sql
/*
2. Count the total number of columns (or fields) are in the sales table
*/
-- Column count

			SELECT 
	   COUNT(*)
		as column_count
		FROM 
		INFORMATION_SCHEMA.COLUMNS
		WHERE 
		TABLE_NAME = 'sales_data';
```

### Output

![2  Column_count (testing)](https://github.com/user-attachments/assets/e3f35f64-2bea-4155-9452-fe85839aa2db)


## Data type check

### SQL query
```sql
/*
3.	Data types check
*/
-- 3. Data types check

	SELECT 
	COLUMN_NAME,
	DATA_TYPE 
	FROM 
  INFORMATION_SCHEMA.COLUMNS
	WHERE 
   TABLE_NAME = 'sales_data';

```
### Output
![3  Check_data_type](https://github.com/user-attachments/assets/f5608668-76f2-43cd-a441-bb5dd51baf35)


## Duplicate count check

### SQL query
```sql
/*
4. Duplicate counts
*/
--4.	Duplicate records count 		
	SELECT *, COUNT(*) AS duplicate_count
	FROM sales_data
	GROUP BY Country, Product, Units_Sold, Manufacturing_Price, Sale_Price, Gross_Sales, Discounts, Sales, COGS, 
	Profit, Month_Name, Date, Year
	HAVING COUNT(*) > 1;

	```

### Output

![4  Duplicate_records_count](https://github.com/user-attachments/assets/d88894c0-c5ea-4c88-9171-353437e58f48)


|Property|	Description|
|-|-|
|Number of Rows|	700|
|Number of Columns	|13|


# Visualization

### 📈Power BI Visuals Used

- **KPI Cards:** Display total sales, profit, discount impact, and average sales.
- **Line Chart:** Monthly sales trends by product.
- **Bar Chart:** Sales by country.
- **Treemap:** Sales by product category.
- **Matrix Table:** Profit margins by product.

![Dashboard](https://github.com/user-attachments/assets/422f7a17-dde4-4856-a6ec-01d583b795bb)

### Detailed-view
![Detailed-view](https://github.com/user-attachments/assets/85227a54-3d63-42d5-baa8-9d9dc0e92001)

## 🖥DAX Measures

📍 Total Sales
```sql
Total Sales = SUM('Data'[Sales])
```

📍 Total Profit
```sql
Total Profit = SUM('Data'[Profit])
```

📍 Profit Margin %
```sql
Profit Margin % = DIVIDE(SUM('Data'[Profit]), SUM('Data'[Sales]), 0)
```
📍 Discount Impact
```sql
Discount Impact = SUM('Data'[Total Discounts])
```
📍 Monthly Sales
```sql
Monthly Sales = SUMX(VALUES('Data'[Month]), SUM('Data'[Sales]))
```

📍Total Gross Sales 
```sql
SUM(Data[Gross Sales])
```

📍Profit to COGS Ratio
```sql
DIVIDE(SUM(Data[Profit]), SUM(Data[COGS]), 0)
```

📍Running Total Sales
```sql
CALCULATE(
    SUM(Data[ Sales]),
    FILTER(
        ALL(Data[Date]),
        Data[Date] <= MAX(Data[Date])
    )
)
```

📍Max Sales Previous Year
```sql
 CALCULATE(
    MAX(Data[ Sales]),
    YEAR(Data[Date]) = YEAR(TODAY()) - 1
)
```


📍Max Sales Current Year = 
```sql
CALCULATE(
    MAX(Data[ Sales]),
    YEAR(Data[Date]) = YEAR(TODAY())
)
```

📍Gross Sales per Unit = 
```sql
DIVIDE(SUM(Data[Gross Sales]), SUM(Data[Units Sold]), 0)
```


📍Avg Sales per Month
```sql
AVERAGEX(
    VALUES(Data[Month Name]),
    SUM(Data[ Sales])
)
```

📍Avg Sales 
```sql
AVERAGE(Data[ Sales])
```


📊 Dashboard Analysis

# Analysis

## Key Performance Indicators (KPIs)
|Metric|Value|Description|
|-|-|-|
|Total Sales|$189.3M|Overall revenue generated|
|Total Profit| $87.5M|Net Profit after deducting costs| 
|Discount Impact|$9M|Amount of discount applied|
|Average Sales|$270K|Average revenue per sale|

## Sales Breakdown by Year and Quarter

- **Sales Trends:** The dashboard compares sales between **2018** and **2019,** showing a growth pattern.

- **Quarterly Performance:**

  - **Q1:** $28.1M
  - **Q2:** $42.4M
  - **Q3:** $52.4M
  - **Q4:** $56.7M (highest sales)

**Insight: Q4 shows the highest sales, possibly due to seasonal factors like holiday shopping.**


## Sales by Country
|County|Sales($M)|
|-|-|
|Indian|40.81M|
|United Kingdom| 39.74M|
|United States|37.18M|
|Brazil| 36.65M|

**Insight:** India has the **highest sales,** indicating strong market presence.



## Sales by Product and COGS Analysis

- Top-Selling Products:

  - **Vermont ($53.6M)**
  - **Mandarin ($30.1M)**
  - **Royal Oak ($27.2M)**


- Cost of Goods Sold (COGS):
  - **Vermont:** $56M
  - **Mandarin:** $32M
  - **Burlington:** $32M

**Insight:** Some products have **high COGS**, affecting profit margins. Vermont has high sales but also high costs.



## 🔍Business Insights and Recommendations

- ✅ **India & UK have the highest sales** → Focus marketing campaigns in these regions.
- ✅ **Q4 shows peak sales** → Plan inventory and promotions ahead of the holiday season.
- ✅ **High COGS for Vermont** → Re-evaluate pricing and supplier costs to improve margins.
- ✅ **Discount Impact at $9M** → Assess if discounts are driving sales or reducing profitability.

