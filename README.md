# Part 4: Tableau Executive Dashboard & Data Storytelling

## Project Overview

This repository contains an executive Tableau dashboard built for a retail leadership team to monitor sales performance, profitability, customer segments, category performance, shipping performance, discount impact, and return patterns across India.

---

## Repository Structure

```
part4_tableau_dashboard/
├── data/
│   └── dashboard_sales_data.xlsx
├── tableau/
│   └── executive_dashboard.twbx
├── outputs/
│   ├── dashboard_story.md
│   ├── business_insights.md
│   └── chart_selection_justification.md
├── screenshots/
│   ├── full_dashboard.png
│   ├── sales_trend_view.png
│   ├── regional_performance_view.png
│   ├── category_profitability_view.png
│   └── filter_interaction_view.png
└── README.md
```

---

## Dataset Summary

| Attribute | Value |
|---|---|
| File | `dashboard_sales_data.xlsx` |
| Total Orders | 4,200 |
| Date Range | 2024–2025 |
| Total Sales | ₹21.7 Crore (approx.) |
| Total Profit | ₹3.33 Crore |
| Overall Profit Margin | 15.35% |
| Return Rate | ~4.5% (191 returned orders) |
| Avg. Delivery Days | 3.59 days |

---

## Field Descriptions & Data Types

| Field | Data Type | Description |
|---|---|---|
| order_id | String (Dimension) | Unique order identifier |
| order_date | Date | Date the order was placed |
| ship_date | Date | Date the order was shipped |
| customer_id | String (Dimension) | Unique customer identifier |
| customer_segment | String (Dimension) | Segment: Consumer, Corporate, Home Office |
| region | String (Dimension) | Geographic region: North, South, East, West |
| state | String (Geographic) | Indian state |
| city | String (Geographic) | City name |
| category | String (Dimension) | Product category: Technology, Furniture, Office Supplies |
| sub_category | String (Dimension) | Product sub-category |
| product_name | String (Dimension) | Product name |
| ship_mode | String (Dimension) | Shipping method used |
| sales | Number (Measure) | Revenue from the order |
| quantity | Integer (Measure) | Number of units ordered |
| discount | Decimal (Measure) | Discount applied (0 to 1 scale) |
| profit | Number (Measure) | Profit earned |
| return_flag | Integer (Boolean) | 1 = Returned, 0 = Not Returned |
| delivery_days | Integer (Measure) | Days from order to delivery |
| customer_rating | Decimal (Measure) | Customer satisfaction rating (1–5) |
| campaign_channel | String (Dimension) | Marketing channel: Organic, Email, Paid, Social, Referral |

---

## Assumptions

1. **order_date and ship_date** are stored as Excel serial numbers and converted to proper date format in Tableau using date parsing.
2. **delivery_days** is interpreted as the number of calendar days from order placement to delivery. Orders with 0 delivery days are classified as "Same Day" deliveries.
3. **return_flag = 1** indicates a returned order; return rate is calculated as returned orders ÷ total orders.
4. **discount** values range from 0 to 1 (e.g., 0.3 = 30% discount). For readability, they are formatted as percentages in the dashboard.
5. **profit** can be negative, indicating a loss on that order — typically occurring when high discounts are applied.
6. **campaign_channel** has some null values, assumed to represent orders with no tracked marketing source.
7. **customer_rating** is on a scale of 1 to 5; higher is better.
8. Geographic data refers to **India**: states and cities are Indian.

---

## Calculated Fields

The following calculated fields were created in Tableau:

| Calculated Field | Formula | Purpose |
|---|---|---|
| Profit Margin | `SUM([Profit]) / SUM([Sales])` | Measures profitability as a percentage of sales |
| Cost | `SUM([Sales]) - SUM([Profit])` | Estimates total cost of goods |
| Average Order Value | `SUM([Sales]) / COUNTD([Order ID])` | Average revenue per order |
| Return Rate | `SUM([Return Flag]) / COUNT([Order ID])` | Percentage of orders returned |
| Shipping Delay Bucket | `IF [Delivery Days] = 0 THEN "Same Day" ELSEIF [Delivery Days] <= 2 THEN "Fast (1–2 days)" ELSEIF [Delivery Days] <= 4 THEN "Standard (3–4 days)" ELSEIF [Delivery Days] <= 6 THEN "Slow (5–6 days)" ELSE "Very Slow (7+ days)" END` | Groups delivery speed into business-friendly categories |
| Discount Category | `IF [Discount] = 0 THEN "No Discount" ELSEIF [Discount] < 0.2 THEN "Low (< 20%)" ELSEIF [Discount] < 0.3 THEN "Medium (20–29%)" ELSE "High (30%+)" END` | Groups discount levels for analysis |

---

## How to Open the Dashboard

1. Open **Tableau Desktop** (version 2022.1 or later recommended).
2. Navigate to `tableau/executive_dashboard.twbx`.
3. Double-click to open — the packaged workbook includes the data source.
4. Use the interactive filters (Region, Category, Segment, Date, Ship Mode) to explore the data.

---

## Key Business Findings (Summary)

- **Technology** is the most profitable category (18.2% margin vs Furniture's 6.9%).
- **High discounts (≥ 30%)** reduce profit margins to just **6.5%** vs **19.8%** for low-discount orders.
- **Home Office** segment has the highest return rate at **5.7%**.
- **South region** leads in both sales and profit.
- **Copiers, Accessories, and Phones** are the top profitable sub-categories.
- **Standard Class shipping** accounts for the largest share of orders but has the highest average delivery time (4.7 days).

---

## Author

*[gurbanibal] — [bitsom_ba_25111041]*
