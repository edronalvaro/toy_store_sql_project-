# Toy Store Sales & Inventory Analysis (SQL Project)

---

## Overview
This SQL project analyzes sales performance, profitability, and inventory efficiency for a multi-store toy retailer.  

The goal is to identify revenue drivers, optimize inventory allocation, and uncover operational inefficiencies that impact profitability.

---

## Business Objective
The business is profitable but faces key operational challenges:

- Uneven product profitability across categories  
- Inventory inefficiencies (stockouts and overstock)  
- Revenue concentration in a small number of products and stores  
- Seasonal demand fluctuations impacting planning  

The objective of this analysis is to provide data-driven insights to improve profitability and operational efficiency.

---

## Key Business Questions
- Which products generate the most revenue and profit?
- How does performance vary across product categories?
- Which stores contribute the most to overall revenue?
- Where are inventory inefficiencies occurring?
- How does revenue change over time (seasonality & trends)?

---

## Data Preparation & Cleaning
- Removed currency symbols from price and cost fields  
- Converted data types for accurate financial calculations  
- Checked for duplicates and invalid records  
- Handled missing values using COALESCE  
- Created a centralized analytics table (`master_sales`) for reporting  

---

## Key Analysis Performed

### 1. Product Performance Analysis
- Identified top and bottom performing products by revenue and profit  
- Calculated profit margins across products and categories  
- Ranked products within each category using window functions  

**Insight:**  
High revenue products are not always the most profitable. Some lower revenue products generate significantly higher margins.

---

### 2. Inventory Analysis
- Identified low-stock high-demand products  
- Measured sell-through rates across SKUs  
- Compared inventory levels against actual sales  

**Insight:**  
Stockouts in high-performing products are causing missed revenue opportunities, while low-demand products are overstocked.

---

### 3. Store Performance Analysis
- Ranked stores by revenue contribution  
- Analyzed profit margins by store location  
- Identified top-performing and underperforming stores  

**Insight:**  
Large urban stores drive most revenue, while smaller stores often show higher efficiency in profit margin.

---

### 4. Time-Series Analysis
- Monthly revenue trend analysis  
- MoM growth calculation using LAG window function  
- Seasonal pattern detection  

**Insight:**  
Sales show strong seasonality with peaks in Q4 and declines in January, followed by gradual recovery.

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
