# Cloud-Native Pizza Sales Analytics & Time-Series Business Intelligence Dashboard

## 📊 Executive Project Summary
This business intelligence solution provides an enterprise-grade analytics framework designed to monitor operational retail volume, evaluate peak time-series transaction trends, and break down product performance metrics. 

Architected using a cloud-hosted infrastructure, the project leverages **Supabase (PostgreSQL)** as the relational database engine and **Metabase** as the visualization layer. The dashboard transforms raw operational transaction records into highly scannable, actionable insights—enabling retail managers to optimize staffing schedules, manage supply chain inventories, and track localized revenue velocity.

![Dashboard Analytics Preview](dashboard_preview.png)

---

## 💡 Strategic Business Insights Discovered

### 🕒 Peak Operational Hours & Time-Series Shifts
* **The Lunchtime Spike (12:00 PM):** Transaction volumes reach their peak across almost all weekdays, with specific days exceeding 415 to 421 concurrent orders within that single hour block. This dictates a strict requirement for maximum kitchen staffing and prep readiness before noon.
* **The Evening Dinner Surge (01:00 PM & 06:00 PM):** A secondary major volume wave clusters heavily around early afternoon (1:00 PM) and late evening (6:00 PM), with Friday and Saturday nights maintaining high operational consistency (387–413 orders per hour).
* **Late-Night Velocity Drops (10:00 PM):** Operational demand significantly scales down across the board by late evening, dropping sharply to under 60-80 orders on specific weekdays (e.g., Mondays and Tuesdays), allowing managers to optimize labor costs by scheduling early kitchen close-outs.

### 🍕 Product Breakdown & Order Profiles
* **Consistent Category Market Share:** Total order volumes are distributed almost equally across product classes, with **Classic** leading at 28.9% (or 25.0% distinct orders), closely followed by **Veggie** (28.1%) and **Supreme** (28.1%), indicating uniform customer demand across the core menu.
* **Chicken Category Variance:** While Chicken holds a slightly lower overall category share (18.8%), its distinct order share climbs to 22.6%, revealing that when customers order chicken options, they represent highly targeted, specific purchasing profiles.

---

## 🛠️ Tech Stack & Infrastructure Architecture
* **Relational Database Engine:** PostgreSQL (Hosted via **Supabase Cloud BaaS**)
* **Business Intelligence & Visualization Layer:** Metabase BI Platform
* **Querying & Analytical Computations:** Advanced ANSI SQL (Time-series manipulations, multi-dimensional aggregations, dynamic date filters)

---

## 📈 Dashboard Architecture & Analytical Panes
The dashboard canvas is engineered with interactive controls to allow multi-variable data drilling:
* **Global Dynamic Slicers:** Integrated with cross-filtering components allowing stakeholders to drill down by **Category** (4 selections) and customized **Date Filters** (e.g., April 1, 2015 – May 27, 2015).
* **Time-Series Metric Engine (`date_wise_orders`):** A continuous line chart capturing total daily order fluctuations alongside target performance goals to map operational volatility.
* **High-Density Heatmap (`timezone_wise_pivottable`):** A matrix breakdown cross-tabulating transaction hours (10 AM to 10 PM) against operational days of the week (Monday through Sunday).
* **Category Breakdown Donut Charts:** Dual-metric visualization contrasting overall raw order distributions alongside unique distinct order counts.

---

## 🧠 Database Engineering & SQL Showcase
To feed the high-density Metabase pivot tables and dashboard visuals, the underlying data layer utilizes optimized SQL transformations.

### 1. High-Density Operational Heatmap Query
```sql
SELECT 
    EXTRACT(HOUR FROM order_time) AS operational_hour,
    TO_CHAR(order_date, 'DAY') AS day_of_week,
    COUNT(order_id) AS total_orders
FROM pizza_sales
WHERE order_date BETWEEN '2015-04-01' AND '2015-05-27'
GROUP BY 1, 2
ORDER BY 1 ASC;
