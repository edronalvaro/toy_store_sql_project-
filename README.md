# Toy Store Sales & Inventory Analysis (SQL Project)

A SQL data analysis project that explores sales performance, profitability, and inventory efficiency for a multi-store toy retailer. The goal is to uncover revenue drivers, optimize stock levels, and improve operational decision-making.

## Key Highlights

- Built a centralized analytics table using SQL (master_sales)
- Analyzed $ multi-million revenue dataset across stores and products
- Identified top-performing and underperforming products
- Measured inventory efficiency using sell-through rates
- Performed time-series analysis (monthly & seasonal trends)
- Used window functions (RANK, LAG) for advanced insights

- ## Business Impact

This analysis helps identify revenue opportunities, reduce stock inefficiencies, and improve profit margins through data-driven inventory and product decisions.

# Toy Store Sales & Inventory Analysis (SQL Project)

A SQL data analysis project that explores sales performance, profitability, and inventory efficiency for a multi-store toy retailer. The goal is to uncover revenue drivers, optimize stock levels, and improve operational decision-making.

## Key Highlights

- Built a centralized analytics table using SQL (master_sales)
- Analyzed multi-million dollar revenue dataset across stores and products
- Identified top-performing and underperforming products
- Measured inventory efficiency using sell-through rates
- Performed time-series analysis (monthly & seasonal trends)
- Used window functions (RANK, LAG) for advanced insights

## Business Impact

This analysis helps identify revenue opportunities, reduce stock inefficiencies, and improve profit margins through data-driven decisions.

## Top 5 Products by Profit Margin
```sql
SELECT 
    product_name, 
    ROUND(SUM(profit)/SUM(revenue) * 100, 2) AS profit_margin
FROM master_sales 
GROUP BY product_name
ORDER BY profit_margin DESC
LIMIT 5;
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
```
