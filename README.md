# Toy Store Sales & Inventory Analysis (SQL Project)

---

## Executive Summary
This project analyzes sales performance, profitability, and inventory efficiency for a multi-store toy retailer using SQL.

The objective is to identify key revenue drivers, evaluate product and store performance, and uncover operational inefficiencies affecting profitability and inventory management.

The analysis reveals a profitable business with strong seasonal demand patterns, but also highlights inefficiencies in inventory allocation and uneven product contribution to revenue and profit.

---

## Business Problem
Although the business is profitable, several operational challenges exist:

- Uneven profitability across product categories and SKUs  
- Inventory misalignment leading to stockouts and overstock situations  
- Revenue concentration in a limited number of products and stores  
- Seasonal fluctuations impacting demand forecasting and planning  

This analysis aims to provide data-driven recommendations to improve profitability and operational efficiency.

---

## Key Business Questions
- Which products generate the highest revenue and profit?
- How does profitability vary across product categories?
- Which stores contribute the most to overall performance?
- Where do inventory inefficiencies exist?
- What seasonal patterns exist in sales performance?

---

## Data Preparation & Engineering

The analysis was built on a structured SQL transformation process:

- Removed currency symbols from product pricing fields in the products table  
- Converted price and cost fields into numeric data types for accurate calculations  
- Identified duplicate records in sales data using aggregation checks  
- Handled missing values within calculated fields using COALESCE  
- Built a centralized analytics table (`master_sales`) to consolidate sales, product, and store data for analysis  

This transformation layer enabled efficient querying and consistent metric calculations.

---

## Analytical Approach

### 1. Product Performance Analysis
- Measured total revenue and profit by product  
- Calculated profit margins across products and categories  
- Ranked products using window functions  

**Key Insight:**  
High revenue products are not always the most profitable, indicating a need to balance volume and margin-driven strategy.

---

### 2. Inventory Efficiency Analysis
- Identified low-stock high-demand products  
- Measured sell-through rates across SKUs  
- Compared inventory levels against actual sales demand  

**Key Insight:**  
Stockouts in high-demand products result in lost revenue opportunities, while some slow-moving products indicate excess inventory holding.

---

### 3. Store Performance Analysis
- Evaluated revenue contribution by store  
- Analyzed profit margins across locations  
- Ranked store performance over time  

**Key Insight:**  
Large urban stores drive the majority of revenue, while smaller stores often demonstrate higher efficiency in profit margins.

---

### 4. Time-Series Analysis
- Monthly revenue trend analysis  
- Month-over-month growth using LAG window function  
- Seasonal pattern identification  

**Key Insight:**  
Sales demonstrate strong seasonality, with peaks in Q4 and declines in January, followed by gradual recovery.

---

## Top 5 Products by Profit Margin
```sql
SELECT 
    product_name, 
    ROUND(SUM(profit)/SUM(revenue) * 100, 2) AS profit_margin
FROM master_sales 
GROUP BY product_name
ORDER BY profit_margin DESC
LIMIT 5;

This query identifies the most profitable products based on margin rather than revenue, helping prioritize high-value inventory decisions.
```

## Month-over-Month Revenue Growth
```sql
WITH monthly_revenue AS (
    SELECT 
        DATE_FORMAT(sale_date, '%Y-%m') AS month,
        SUM(revenue) AS total_revenue 
    FROM master_sales 
    GROUP BY 1
)
SELECT 
    month,
    total_revenue,
    LAG(total_revenue) OVER (ORDER BY month) AS prev_month,
    ROUND(
        (total_revenue - LAG(total_revenue) OVER (ORDER BY month)) 
        / LAG(total_revenue) OVER (ORDER BY month) * 100, 2
    ) AS mom_growth_pct
FROM monthly_revenue;

This query highlights month-over-month revenue trends and captures seasonality patterns in sales performance.
```
These queries demonstrate both profitability analysis and time-series analysis using window functions.
