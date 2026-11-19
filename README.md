# Myntra E-commerce Sales & Product Analysis Dashboard (Power BI)

## Project Overview
This project analyses Myntra-style e-commerce order and product catalogue data using **Microsoft Power BI**. It is designed as a two-page interactive report: **Sales Overview** and **Product Analysis**. The report uses KPI cards, charts, a Top 10 products table, slicers, page navigation and a bookmark-controlled filter panel.

> The images in `screenshots/` are chart and dashboard preview graphics generated from the supplied CSV data. They are documentation previews, not screenshots exported from Power BI. Replace or supplement them with screenshots of your final `.pbix` report when it is complete.

## Project Objectives
- Measure revenue, order volume, quantity and product count.
- Compare revenue and orders across product categories, brands and city tiers.
- Review order-status distribution.
- Identify the Top 10 products by net revenue.
- Explore product size inventory from the catalogue.
- Build an interactive report with slicers and page navigation.

## Data Sources
| File | Purpose |
|---|---|
| `myntra_orders_master.csv` | Main order-level fact data used for sales analysis. |
| `myntra_festive_orders.csv` | Seasonal subset of order data. |
| `myntra_summer_orders.csv` | Seasonal subset of order data. |
| `myntra_winter_orders.csv` | Seasonal subset of order data. |
| `myntra_product_catalog.csv` | Product attributes and size inventory columns. |
| `myntra_web_catalog.csv` | Product web ratings and review counts; merge into product dimension by SKU. |

**Modeling note:** The three seasonal order files are subsets of the master order table in this project; do not append them to the master table unless you first verify they contain additional, non-duplicate orders.

## Data Model
Recommended model structure:
- `fact_orders` — renamed from `myntra_orders_master`; one row per order line/record.
- `Dim_product` — product catalogue, enriched with `Web_Rating` and `Review_Count` from the web catalogue using `SKU`.
- `Dim_Date` — calendar table related to `fact_orders[Order_Date]`.
- `Fact_Size_Inventory` — referenced from `Dim_product`; unpivot the size columns in this separate table so each SKU can have multiple size rows without duplicating the product dimension key.

Relationships:
- `Dim_product[SKU]` (one) → `fact_orders[SKU]` (many).
- `Dim_Date[Date]` (one) → `fact_orders[Order_Date]` (many).
- `Dim_product[SKU]` (one) → `Fact_Size_Inventory[SKU]` (many).

## Key Measures (DAX)
```dax
Total Revenue = SUM(fact_orders[Net_Revenue])

Total Orders = DISTINCTCOUNT(fact_orders[Order_ID])

Total Quantity = SUM(fact_orders[Quantity])

Average Order Value = DIVIDE([Total Revenue], [Total Orders], 0)

Delivered Orders =
CALCULATE([Total Orders], fact_orders[Order_Status] = "Delivered")

Cancelled Orders =
CALCULATE([Total Orders], fact_orders[Order_Status] = "Cancelled")

Total Products = DISTINCTCOUNT(Dim_product[SKU])
```

**Data limitation:** The supplied master order file does not contain a `Customer_ID` field, so a distinct-customer KPI cannot be calculated reliably from these files. Do not substitute Order_ID for Customer_ID.

## Dataset Snapshot
| Metric | Value |
|---|---:|
| Total net revenue | ₹1,188,952,967.33 |
| Distinct orders | 180,000 |
| Total quantity | 449,852 |
| Distinct catalogue products | 2,500 |
| Average revenue per order | ₹6,605.29 |
| Order date range | Jan 2023 – May 2025 |

*These are calculated directly from the supplied CSVs for documentation. Power BI totals may differ if filters, relationships, or row-level aggregation differ.*

## Dashboard Pages and Visuals
### 1. Sales Overview
- Total Revenue, Total Orders and Total Quantity KPI cards.
- Orders by City Tier.
- Orders by Category and Brand.
- Monthly order/revenue trends.
- Brand, Category and City Tier slicers.

![Sales Overview dashboard preview](screenshots/08_sales_overview_dashboard_preview.png)

### 2. Product Analysis
- Product Distribution by Category.
- Size-wise Inventory.
- Brand Performance by Revenue.
- City Tier Analysis by Revenue.
- Top 10 Products by Revenue.
- Order Status Distribution.
- Total Products KPI.
- Left-side page navigator and bookmark-based filter panel.

![Product Analysis dashboard preview](screenshots/09_product_analysis_dashboard_preview.png)

## Chart Previews
### Revenue by Category
![Revenue by category](screenshots/01_revenue_by_category.png)

### Top 10 Brands by Revenue
![Top brands](screenshots/02_top_brands_revenue.png)

### Monthly Revenue Trend
![Monthly trend](screenshots/03_monthly_revenue_trend.png)

### Revenue by City Tier
![City tier](screenshots/04_revenue_by_city_tier.png)

### Order Status Distribution
![Order status](screenshots/05_order_status_distribution.png)

### Top 10 Products by Revenue
![Top products](screenshots/06_top_10_products_table.png)

### Size-wise Inventory
![Size inventory](screenshots/07_size_inventory.png)

## Tools and Skills Demonstrated
- Microsoft Power BI Desktop
- Power Query data cleaning and transformation
- Data modeling, relationships and star-schema concepts
- DAX measures and calculated date table
- KPI cards, donut charts, bar charts, column charts and tables
- Slicers, page navigator, bookmarks and interactive filtering
- Data storytelling and dashboard design

## Suggested Repository Structure
```text
Myntra_PowerBI_Project/
├── README.md
├── screenshots/
│   ├── 01_revenue_by_category.png
│   ├── 02_top_brands_revenue.png
│   ├── 03_monthly_revenue_trend.png
│   ├── 04_revenue_by_city_tier.png
│   ├── 05_order_status_distribution.png
│   ├── 06_top_10_products_table.png
│   ├── 07_size_inventory.png
│   ├── 08_sales_overview_dashboard_preview.png
│   └── 09_product_analysis_dashboard_preview.png
└── data/  (optional: include source CSVs only if licensing and file size permit)
```

## How to Use
1. Open Power BI Desktop.
2. Import `myntra_orders_master.csv`, `myntra_product_catalog.csv` and `myntra_web_catalog.csv`.
3. Clean data types and missing values in Power Query.
4. Merge web rating and review fields into the product dimension using SKU.
5. Create the fact and dimension tables and relationships described above.
6. Create the DAX measures and visuals.
7. Save the report as `Myntra_Sales_Analysis.pbix`.

## Notes
- This is a learning and portfolio project, not an official Myntra report.
- The supplied data should be treated as a practice dataset; findings describe only these files.
- Keep the source CSVs separate from generated preview images.
- Add screenshots exported from your finished Power BI report before publishing the final portfolio version.

---
**Project:** Myntra E-commerce Sales & Product Analysis  
**Tool:** Microsoft Power BI  
**Author:** Chandan Nigam
