# Chart Selection Justification
## Executive Dashboard — Retail Sales Analysis

---

## Overview

Every chart in the executive dashboard was chosen based on the specific business question it needed to answer. This document explains the reasoning behind each chart type, aligned with data visualization best practices.

---

## View 1: Sales Trend Over Time — Line Chart

**Business Question:** How are total sales and profit trending month-over-month and year-over-year?

**Chart Type:** Line Chart (dual-axis with Sales and Profit)

**Justification:**
- Line charts are the standard for showing **continuous change over time**. They make it easy to spot trends, seasonality, and inflection points.
- A dual-axis line chart allows Sales and Profit to be compared on separate scales without distortion.
- Bar charts could also work for monthly data, but a line chart reduces visual clutter when comparing multiple time periods (monthly across 2 years).
- The X-axis represents time (months), and the Y-axis represents currency values — a natural pairing for a time-series.

**Design Decisions:**
- Sales line in blue (primary metric), Profit line in green (secondary metric).
- Axis labels formatted in ₹ Lakhs for readability.
- Reference line added at zero profit to highlight any months with losses.

---

## View 2: Regional Performance — Filled Map + Bar Chart

**Business Question:** Which regions and states generate the most sales and profit?

**Chart Type:** Filled Map (choropleth) for state-level, horizontal Bar Chart for region-level summary

**Justification:**
- A **filled map** leverages geographic intuition — decision-makers immediately understand that darker = higher performance without reading labels. It encodes geographic patterns that a bar chart cannot.
- A **bar chart** alongside the map provides precise numerical comparison between the 4 regions (North, South, East, West), which is harder to read on a map.
- Pie charts were avoided because they are difficult to compare across more than 3–4 segments and don't show geographic context.
- Sorted bars (descending) highlight top-performing regions clearly.

**Design Decisions:**
- Map uses a sequential green colour palette (light = low, dark = high).
- Bar chart is sorted by Sales descending.
- Both views are linked via a region filter action.

---

## View 3: Category & Sub-Category Profitability — Horizontal Bar Chart / Highlight Table

**Business Question:** Which product categories and sub-categories drive profit — and which cause losses?

**Chart Type:** Horizontal Bar Chart (with diverging colours for profit/loss)

**Justification:**
- A **bar chart** is the clearest chart type for comparing discrete categories on a single metric (profit).
- **Horizontal layout** is preferred over vertical because sub-category names are long strings that would overlap on a vertical axis.
- **Diverging colour encoding** (red for negative, green for positive) immediately draws the eye to loss-making sub-categories without requiring the viewer to read exact values.
- A treemap was considered but rejected because it makes negative values (losses) harder to display and interpret cleanly.

**Design Decisions:**
- Bars sorted from most profitable to least profitable.
- Red bars for sub-categories with negative profit (Tables, Bookcases under heavy discounts).
- Data labels added to the end of each bar for precise values.

---

## View 4: Customer Segment Performance — Grouped Bar Chart

**Business Question:** How do Consumer, Corporate, and Home Office segments compare on Sales, Profit, and Return Rate?

**Chart Type:** Grouped Bar Chart (Sales & Profit side by side per segment)

**Justification:**
- A **grouped bar chart** allows direct comparison of multiple metrics (Sales, Profit, Return Rate) across the 3 segments simultaneously.
- A stacked bar chart would have been less readable because it would require comparing segment totals rather than individual segment values.
- A pie chart was rejected because it cannot show multiple metrics for the same category groups.
- The grouping makes it easy to spot which segment has the best profit efficiency vs. raw volume.

**Design Decisions:**
- Return Rate shown as a secondary metric on a line (dual-axis), overlaid on the grouped bars.
- Consistent colour coding: Consumer = blue, Corporate = orange, Home Office = grey.

---

## View 5: Shipping Mode & Delivery Delay — Bar Chart

**Business Question:** Which shipping modes cause the longest delays, and how does this impact customer ratings?

**Chart Type:** Horizontal Bar Chart (Average Delivery Days by Ship Mode) + Scatter Plot (Delivery Days vs Customer Rating)

**Justification:**
- A **bar chart** clearly ranks shipping modes by average delivery time — the primary operational metric.
- A **scatter plot** is used to explore the relationship between delivery delay and customer rating, since scatter plots are the ideal chart for showing correlations between two continuous variables.
- A scatter plot with one point per order would be too noisy, so delivery day buckets (Shipping Delay Buckets calculated field) are used as the X-axis with average rating on the Y-axis.

**Design Decisions:**
- Bar chart sorted by average delivery days (ascending), so the fastest mode is at the top.
- Colour used to encode shipping mode consistently across both charts for cross-referencing.

---

## View 6: Discount vs Profit — Scatter Plot

**Business Question:** Is there a relationship between the discount applied and the profit generated per order?

**Chart Type:** Scatter Plot

**Justification:**
- A **scatter plot** is the correct and only appropriate chart type for showing the **correlation or relationship between two continuous variables** (discount % and profit).
- It reveals patterns such as: as discount increases, profit decreases — and at what discount level profit turns negative.
- Bar charts, line charts, or pie charts cannot reveal this kind of variable-to-variable relationship.
- A trend line (linear regression) is added to make the direction of correlation immediately clear.

**Design Decisions:**
- X-axis: Discount percentage (0% to 35%).
- Y-axis: Profit per order.
- Colour encoding: Category (Technology, Furniture, Office Supplies) to identify which categories are most affected by discounting.
- Reference line at Profit = 0 to visually separate profitable from loss-making orders.

---

## View 7: Return Analysis — Bar Chart / Highlight Table

**Business Question:** Which categories, segments, or regions have the highest return rates?

**Chart Type:** Highlight Table (heat map) showing return rate by Segment × Category

**Justification:**
- A **highlight table** (cross-tab with colour encoding) is ideal for showing how a metric (return rate) varies across **two categorical dimensions** simultaneously (Segment and Category).
- It compresses a lot of information into a compact, scannable grid — more efficient than multiple separate bar charts.
- A bar chart per segment was considered but would require toggling between 3 charts to see the full picture.
- Colour encoding (white to dark red) makes high-return cells immediately stand out.

**Design Decisions:**
- Rows: Customer Segment | Columns: Category
- Cell colour: Return Rate % (white = low, dark red = high)
- Cell labels: show exact return rate % for precision

---

## KPI Cards (Summary Metrics)

Three KPI cards were included on the main dashboard for at-a-glance executive visibility:

| KPI | Value | Reason for Inclusion |
|---|---|---|
| Total Sales | ₹21.7 Crore | The headline business metric |
| Overall Profit Margin | 15.35% | Most important profitability signal |
| Return Rate | 4.55% | Risk indicator for operational quality |

**Design Justification:**
- KPI cards use large, bold numbers to enable quick scanning at the executive level.
- A single prominent number is more actionable for leadership than embedding these values inside charts where they are harder to find.
- Each KPI card responds to the dashboard filters, so when a Region or Category filter is applied, the KPIs update dynamically.

---

## General Design Principles Applied

| Principle | How Applied |
|---|---|
| Correct chart selection | Each chart matches the type of question (trend → line, comparison → bar, relationship → scatter, geographic → map) |
| Clear hierarchy | Dashboard title → KPI cards → charts arranged by importance (top-left = most critical) |
| Minimal clutter | No gridlines on scatter plots; minimal tick marks; whitespace between chart sections |
| Consistent colour | Technology = blue, Furniture = orange, Office Supplies = grey, consistently across all views |
| Proper labels | All axes labelled; chart titles use plain business language ("Monthly Sales Trend" not "Sheet 1") |
| Readable titles | Short, descriptive titles without jargon |
| Appropriate sorting | Bar charts always sorted by value (not alphabetically) for easier comparison |
| Useful filters | Region, Category, Segment, Ship Mode, Date range, Campaign Channel filters all connected to all charts |
| Avoidance of misleading scales | All Y-axes start at zero; no truncated axes; dual axes clearly labelled |
| Focus on business interpretation | Every chart has an annotation or caption explaining the key takeaway |
