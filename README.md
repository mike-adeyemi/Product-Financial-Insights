# Product-Financial-Insights



# Table of contents
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
  - [Key Performance Indicators (KPIs)](#key-performance-indicators-(kpis))
  - [Sales Breakdown by Year & Quarter](#sales-breakdown-by-year-&-quarter)
  - [Sales by Country](#sales-by-country)
  - [Sales by Product & COGS Analysis](#sales-by-product-&-cogs-analysis)
- [Recommendation](#business-insights-&-recommendations)



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


# Testing
Row and Column Count Validation

|Property|	Description|
|-|-|
|Number of Rows|	702|
|Number of Columns	|13|


# Visualization

### 📈Power BI Visuals Used

- **KPI Cards:** Display total sales, profit, discount impact, and average sales.
- **Line Chart:** Monthly sales trends by product.
- **Bar Chart:** Sales by country.
- **Treemap:** Sales by product category.
- **Matrix Table:** Profit margins by product.


## 🖥 DAX Measures

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

## Sales Breakdown by Year & Quarter

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



## Sales by Product & COGS Analysis

- Top-Selling Products:

  - **Vermont ($53.6M)**
  - **Mandarin ($30.1M)**
  - **Royal Oak ($27.2M)**


- Cost of Goods Sold (COGS):
  - **Vermont:** $56M
  - **Mandarin:** $32M
  - **Burlington:** $32M

**Insight:** Some products have **high COGS**, affecting profit margins. Vermont has high sales but also high costs.



## 🔍Business Insights & Recommendations

- ✅ **India & UK have the highest sales** → Focus marketing campaigns in these regions.
- ✅ **Q4 shows peak sales** → Plan inventory and promotions ahead of the holiday season.
- ✅ **High COGS for Vermont** → Re-evaluate pricing and supplier costs to improve margins.
- ✅ **Discount Impact at $9M** → Assess if discounts are driving sales or reducing profitability.

