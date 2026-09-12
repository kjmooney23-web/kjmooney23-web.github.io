---
layout: page
title: "Instacart Shopper Behavior Analysis"
subtitle: "When, What & Who"
permalink: /instacart-analytics/
author_profile: true
toc: true
toc_label: "Project Sections"
toc_icon: "chart-bar"
---

---

## 1. Background & Overview

---

Instacart's core business depends on matching supply to demand — having the right products available, in the right places, for the right customers, at the right time. But without a clear picture of when customers shop, what drives their reorder behavior, and who the most valuable segments are, operational and retention decisions default to intuition rather than evidence.

This project was built to answer three specific questions using the Kaggle Instacart Online Grocery Shopping Dataset: when do customers place orders throughout the day and week, which product departments drive the highest reorder rates, and how do customers segment by loyalty and purchase frequency?

The dataset covers approximately 3.4 million grocery orders from over 200,000 anonymized users across 50,000+ products. dbt was used to build a reproducible SQL transformation pipeline — staging raw CSV files into three analysis-ready mart models — before connecting to Tableau for visualization. A three-dashboard Tableau story was published to communicate the findings as a connected narrative.

The result is an end-to-end analytics workflow that moves from raw data through transformation to business insight — structured the same way a production analytics stack would be built.

---

## 2. Executive Summary

---

Instacart's order volume is concentrated in two clear windows: the weekend and the morning. Saturday and Sunday account for 35% of all weekly orders, and the 8am–10am window is the single highest-volume period of the day, peaking at 284,728 orders at 10am. Overnight hours — midnight through 6am — are nearly inactive. These patterns are consistent and actionable: fulfillment staffing and inventory readiness should be built around the Saturday–Sunday morning window as the primary operational priority.

On the product side, the data reveals a meaningful split between volume and loyalty. Produce drives 29% of all orders — nearly three times the next largest department — but it is Dairy & Eggs and Pets that hold the highest reorder rates at 51% and 49% respectively. Customers don't just buy dairy products; they come back for them reliably. This distinction between departments that attract orders and departments that anchor repeat behavior has real implications for inventory investment and marketing strategy.

The customer segmentation analysis surfaces the most commercially significant finding: Loyal customers — those with 16 or more lifetime orders — represent just 19% of the customer base but drive 41% of total order volume. They shop every six days on average and are the engine of Instacart's revenue. At the other end, At-Risk customers make up 30% of the base but contribute only 12% of orders, shopping every 24 days. The gap between these segments — in both frequency and value — defines where retention investment should be focused.

---

## 3. Insights Deep Dive

---

### Dashboard 1 — When Customers Shop

> **Key Finding:** Order volume is heavily concentrated on weekends and mornings — 35% of weekly orders fall on Saturday and Sunday, and the 8am–10am window is the single busiest period of the day.

A heatmap of order volume by day of week and hour of day shows a clear diagonal pattern: Saturday and Sunday between 9am and 4pm are the densest cells on the grid. The accompanying bar chart confirms that Saturday leads all days at 600,905 orders, followed by Sunday at 587,478 — together representing more than a third of weekly volume. The remaining five weekdays cluster between 426,000 and 467,000 orders, with Monday highest among them.

The hourly line chart tells an equally clear story. Orders are negligible from midnight through 6am, surge rapidly between 7am and 10am, plateau through the early afternoon, and decline steeply after 6pm. The 10am peak at 284,728 orders is the highest single hour across the entire dataset. A linear trend line confirms the morning-weighted demand curve, and the 8am–10am window consistently shows the highest concentration regardless of day of week.

---

### Dashboard 2 — What Customers Are Buying

> **Key Finding:** Produce dominates total order volume at 29% of all orders, but Dairy & Eggs and Pets drive the highest reorder rates — 51% and 49% respectively — indicating where true customer loyalty lives.

A bar chart of reorder rates by department, sorted descending with an average threshold reference line, shows that Dairy & Eggs leads all departments at 51%, followed by Pets (49%), Bakery (47%), Beverages (47%), and Breakfast (47%). These departments sit well above the average reorder threshold. At the bottom, Personal Care (23%), Pantry (24%), and International (25%) trail significantly — customers browse these categories but don't return for them reliably.

The total orders breakdown tells a different story. Produce accounts for 9.4 million orders — nearly double Dairy & Eggs at 5.4 million — making it the volume leader by a wide margin. A scatter plot of top individual products by total orders versus reorder rate identifies Organic Whole Milk, Banana, and Organic Strawberries as standouts: high in both dimensions, making them candidates for subscription or auto-replenishment features.

---

### Dashboard 3 — Shopper Loyalty Segments

> **Key Finding:** Loyal customers make up just 19% of the base but drive 41% of total orders — shopping every six days. At-Risk customers represent 30% of the base but contribute only 12% of orders.

A bar chart of customer distribution by segment shows that At-Risk customers are the largest group at 30%, followed by Regulars at 29%, New & Active at 23%, and Loyalists at 19%. The order contribution chart reveals the inversion: Loyalists punch far above their weight at 41% of total orders, while At-Risk customers contribute just 12% despite being the largest segment by headcount.

A histogram of days between orders shows that most customers shop every 7–17 days, with a secondary cluster at 30 days. The order behavior bar chart breaks this down by segment: Loyalists average 6 days between orders, Regulars 12 days, New & Active 17 days, and At-Risk 24 days. The staircase structure confirms that shopping frequency compounds with engagement — each step up the loyalty ladder represents a measurable acceleration in purchase cadence.

---

## 4. Recommendations

---

### 1. Concentrate Fulfillment Staffing on the Saturday–Sunday Morning Window
Saturday and Sunday between 8am and noon are the single highest-demand window in the dataset. Fulfillment staffing models should treat this window as the primary operational constraint. Friday evening inventory prep and surge staffing for Saturday morning would directly address the period of peak exposure. Overnight hours can be managed with a lean model.

---

### 2. Build a Reorder Reminder Feature for High-Churn Staple Departments
Dairy & Eggs, Pets, and Bakery customers reorder at rates of 47–51% — and they do so on a predictable cadence. A reorder reminder or auto-replenishment nudge triggered 5–7 days after a customer's last purchase in these departments would intercept the natural repurchase window and reduce the chance of a competitor capturing the next order.

---

### 3. Prioritize Produce and Dairy in Inventory Reliability Investment
Produce accounts for 29% of all orders — a stockout or fulfillment failure in this department affects nearly a third of every cart. Dairy & Eggs combines high volume (5.4M orders) with the highest reorder rate in the dataset. Inventory reliability in these two departments is not a supply chain detail; it is a core retention mechanism.

---

### 4. Build a Tiered Loyalty Program Targeting Occasional and Regular Segments
Moving a customer from At-Risk (24-day cadence) to Regular (12-day cadence) represents a near-doubling of purchase frequency. Moving a Regular to Loyal (6-day cadence) doubles it again. A tiered loyalty program — with milestones at the Regular and Loyalist thresholds — creates a structured path for accelerating cadence. Even modest movement across these segments would have a material impact on total order volume.

---

### 5. Use the At-Risk Segment as a Re-engagement Priority
At-Risk customers represent 30% of the user base but contribute only 12% of orders. At a 24-day average between orders, they are not fully churned — but they are close. A targeted re-engagement campaign timed around the 20–22 day mark — before the 30-day drop-off — would maximize the chance of recapturing these customers before they go dormant.

---

### 6. Leverage Top Products for Cross-Sell and Basket Expansion
Banana, Organic Whole Milk, and Organic Strawberries appear in the top tier of both total orders and reorder rate. These products are reliable anchors for repeat visits. Bundling or cross-selling adjacent high-reorder products alongside these anchors is a low-friction way to expand basket size without requiring new customer acquisition.

---

## 5. Caveats & Assumptions

---

### Data Source Limitations

**Kaggle Instacart Dataset (2017)**
The dataset covers prior order history only — the `eval_set = 'prior'` partition — which excludes train and test sets used in the original Kaggle competition. Order timestamps are not included; only the hour of day and day of week are available, meaning date-level trends cannot be analyzed. The dataset represents a snapshot of behavior from a single point in time and may not reflect current Instacart customer patterns.

**Customer Segmentation**
The segmentation model uses order count and average days between orders as its two dimensions. Revenue, basket size, and product mix are not included. The Tableau clustering algorithm (k=4) was validated against the dbt-computed `frequency_segment` labels, which aligned closely, but cluster boundaries are exploratory rather than statistically optimized.

---

### Analytical Assumptions

- The `eval_set = 'prior'` filter was applied in staging to restrict analysis to prior order history only.
- `days_since_prior_order` null values were coalesced to `0` in the staging model and excluded from average days between orders calculations.
- Department-level reorder rates are calculated as `total_reorders / total_orders` at the department grain.
- Tableau's built-in clustering was set to k=4 based on visual inspection of the scatter plot distribution.

---

### Areas for Future Improvement

- **Incorporate revenue and basket size data** to build a true Customer Lifetime Value model.
- **Add timestamp data** to enable day-level trend analysis and cohort-based retention curves.
- **Expand the segmentation model** using a full RFM framework once revenue data is available.
- **Build a churn prediction model** using logistic regression on order recency, frequency, and department mix.

---

## Interactive Dashboard

[View Full Tableau Story](https://public.tableau.com/app/profile/kristin.mooney/viz/InstacartAnalyticsProject/UnderstandingInstacartShopperBehavior){: .btn .btn--primary .btn--large}

---

## 6. Data Structure Overview

---

This project draws on one primary data source: the Kaggle Instacart Online Grocery Shopping Dataset (2017), covering approximately 3.4 million grocery orders from 200,000+ anonymized users. All transformations were built using dbt following a staging → marts architecture before connecting to Tableau for visualization.

---

### Data Sources at a Glance

| Dashboard | Name | Source | dbt Model(s) |
|---|---|---|---|
| 1 | When Customers Shop | Kaggle Instacart Dataset (2017) | `mart_timing_analysis` |
| 2 | What Customers Are Buying | Kaggle Instacart Dataset (2017) | `mart_product_performance` |
| 3 | Shopper Loyalty Segments | Kaggle Instacart Dataset (2017) | `mart_user_rfm` |

---

### Dashboard 1 — When Customers Shop
**dbt model:** `mart_timing_analysis`

> **Key Stats:** Saturday leads at 600,905 orders &nbsp;|&nbsp; 35% of weekly orders on Saturday–Sunday &nbsp;|&nbsp; 10am peak at 284,728 orders &nbsp;|&nbsp; 56% of orders placed between 9am–4pm

| Field | Description |
|---|---|
| `order_hour` | Hour of day (0–23) |
| `order_dow` | Day of week (0=Sunday, 6=Saturday) |
| `total_orders` | Count of distinct orders per hour/day combination |
| `unique_shoppers` | Count of distinct users per hour/day combination |
| `pct_of_total` | Each cell's share of total weekly order volume |

---

### Dashboard 2 — What Customers Are Buying
**dbt model:** `mart_product_performance`

> **Key Stats:** Produce accounts for 29% of all orders (9.4M) &nbsp;|&nbsp; Dairy & Eggs reorder rate: 51% &nbsp;|&nbsp; Pets reorder rate: 49% &nbsp;|&nbsp; Personal Care lowest at 23%

| Field | Description |
|---|---|
| `department` | Product department name |
| `total_orders` | Total order line items in the department |
| `total_reorders` | Count of line items flagged as reordered |
| `unique_products` | Count of distinct products in the department |
| `reorder_rate` | `total_reorders / total_orders` |
| `Reorder Probability` | Calculated tier: High / Medium / Low |

---

### Dashboard 3 — Shopper Loyalty Segments
**dbt model:** `mart_user_rfm`

> **Key Stats:** Loyalists = 19% of customers, 41% of orders &nbsp;|&nbsp; At-Risk = 30% of customers, 12% of orders &nbsp;|&nbsp; Loyalists shop every ~6 days &nbsp;|&nbsp; At-Risk shop every ~24 days

| Field | Description |
|---|---|
| `user_id` | Anonymized customer identifier |
| `total_orders` | Lifetime order count per customer |
| `avg_days_between_orders` | Average gap in days between consecutive orders |
| `frequency_segment` | dbt label: Loyal (16+), Regular (6–15), Occasional (2–5), New (1) |
| `Clusters` | Tableau clustering output (k=4) |
| `Customer Segment` | Calculated field mapping cluster to named segment |

---

## 7. dbt Models

---

All SQL transformations are built in dbt following a staging → marts architecture. Staging models clean and standardize raw source tables; mart models aggregate into analysis-ready outputs.

[View dbt Models →](/instacart-analytics-dbt/){: .btn .btn--info .btn--large}
