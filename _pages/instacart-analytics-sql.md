---
layout: page
title: "Instacart Analytics — SQL Queries"
subtitle: "Staging, Intermediate, Marts & Analytical Queries"
permalink: /instacart-analytics-sql/
author_profile: true
toc: true
toc_label: "SQL Models"
toc_icon: "code"
---


---

[← Back to Project Overview](/instacart-analytics/){: .btn .btn--info}

---

All SQL models are organized following a staging → intermediate → dimensions → facts → marts architecture. Each layer builds on the previous to produce clean, analysis-ready tables for Tableau.

---

## 1. Staging Models

Staging models clean and standardize raw source tables. Each model maps directly to one source CSV file and handles nulls, adds readable labels, and applies consistent naming conventions.

---

### stg_orders

Cleans the raw `orders` table. Adds a readable day name, time-of-day bucket, and reorder cycle bucket. Coalesces null `days_since_prior_order` values (present on each user's first order) to `0`.

```sql
CREATE OR REPLACE VIEW stg_orders AS
SELECT
    order_id,
    user_id,
    eval_set,
    order_number,
    order_dow,
    CASE order_dow
        WHEN 0 THEN 'Sunday'    WHEN 1 THEN 'Monday'
        WHEN 2 THEN 'Tuesday'   WHEN 3 THEN 'Wednesday'
        WHEN 4 THEN 'Thursday'  WHEN 5 THEN 'Friday'
        WHEN 6 THEN 'Saturday'
    END AS order_day_name,
    order_hour_of_day,
    CASE
        WHEN order_hour_of_day BETWEEN 5  AND 8  THEN 'Early Morning (5-8am)'
        WHEN order_hour_of_day BETWEEN 9  AND 11 THEN 'Morning (9-11am)'
        WHEN order_hour_of_day BETWEEN 12 AND 14 THEN 'Lunch (12-2pm)'
        WHEN order_hour_of_day BETWEEN 15 AND 17 THEN 'Afternoon (3-5pm)'
        WHEN order_hour_of_day BETWEEN 18 AND 20 THEN 'Evening (6-8pm)'
        WHEN order_hour_of_day BETWEEN 21 AND 23 THEN 'Night (9-11pm)'
        ELSE 'Overnight (0-4am)'
    END AS time_of_day_bucket,
    COALESCE(days_since_prior_order, 0) AS days_since_prior_order,
    CASE
        WHEN days_since_prior_order IS NULL THEN 'First Order'
        WHEN days_since_prior_order <= 3    THEN '0-3 days'
        WHEN days_since_prior_order <= 7    THEN '4-7 days'
        WHEN days_since_prior_order <= 14   THEN '8-14 days'
        WHEN days_since_prior_order <= 21   THEN '15-21 days'
        ELSE '22-30 days'
    END AS reorder_cycle_bucket
FROM orders;
```

---

### stg_products

Cleans the raw `products` table and adds an `is_organic` flag based on the product name.

```sql
CREATE OR REPLACE VIEW stg_products AS
SELECT
    product_id,
    product_name,
    aisle_id,
    department_id,
    CASE
        WHEN LOWER(product_name) LIKE '%organic%' THEN TRUE
        ELSE FALSE
    END AS is_organic
FROM products;
```

---

### stg_order_products

Unions the `order_products__prior` and `order_products__train` tables into a single clean view with a `source_set` label.

```sql
CREATE OR REPLACE VIEW stg_order_products AS
SELECT
    order_id,
    product_id,
    add_to_cart_order,
    reordered,
    'prior' AS source_set
FROM order_products__prior
UNION ALL
SELECT
    order_id,
    product_id,
    add_to_cart_order,
    reordered,
    'train' AS source_set
FROM order_products__train;
```

---

## 2. Intermediate Models

Intermediate models join staging tables together into a single enriched dataset at the line-item grain, ready for downstream fact and dimension tables.

---

### int_order_items_enriched

Joins all five staging tables into a fully enriched line-item view — one row per product per order, with all order, product, aisle, and department context attached.

```sql
CREATE OR REPLACE VIEW int_order_items_enriched AS
SELECT
    op.order_id,
    op.product_id,
    op.add_to_cart_order,
    op.reordered,
    op.source_set,
    o.user_id,
    o.order_number,
    o.order_day_name,
    o.order_dow,
    o.order_hour_of_day,
    o.time_of_day_bucket,
    o.days_since_prior_order,
    o.reorder_cycle_bucket,
    p.product_name,
    p.is_organic,
    p.aisle_id,
    p.department_id,
    a.aisle,
    d.department
FROM stg_order_products op
JOIN stg_orders   o ON op.order_id    = o.order_id
JOIN stg_products p ON op.product_id  = p.product_id
JOIN aisles       a ON p.aisle_id     = a.aisle_id
JOIN departments  d ON p.department_id = d.department_id;
```

---

## 3. Dimension Tables

Dimension tables provide stable, descriptive reference data used across all fact and mart models.

---

### dim_products

Full product context: product name, organic flag, aisle, and department — all in one table.

```sql
CREATE OR REPLACE TABLE dim_products AS
SELECT
    p.product_id,
    p.product_name,
    p.is_organic,
    a.aisle_id,
    a.aisle      AS aisle_name,
    d.department_id,
    d.department AS department_name
FROM stg_products p
JOIN aisles      a ON p.aisle_id      = a.aisle_id
JOIN departments d ON p.department_id = d.department_id;
```

---

### dim_users

User-level behavioral profile: lifetime orders, preferred shopping day and hour, and shopper segment label.

```sql
CREATE OR REPLACE TABLE dim_users AS
SELECT
    user_id,
    COUNT(DISTINCT order_id)                         AS total_orders,
    AVG(days_since_prior_order)                      AS avg_days_between_orders,
    MODE() WITHIN GROUP (ORDER BY order_dow)         AS preferred_day_of_week,
    MODE() WITHIN GROUP (ORDER BY order_hour_of_day) AS preferred_hour,
    CASE
        WHEN COUNT(DISTINCT order_id) >= 20 THEN 'Power Shopper'
        WHEN COUNT(DISTINCT order_id) >= 10 THEN 'Regular Shopper'
        WHEN COUNT(DISTINCT order_id) >= 5  THEN 'Occasional Shopper'
        ELSE 'New / Infrequent Shopper'
    END AS shopper_segment
FROM stg_orders
GROUP BY user_id;
```

---

## 4. Fact Tables

Fact tables capture transactional data at the appropriate grain. Two fact tables are built: one at the order-item level and one at the order-header level.

---

### fct_order_items

One row per product per order. The lowest-grain table in the project — used for product-level analysis.

```sql
CREATE OR REPLACE TABLE fct_order_items AS
SELECT
    op.order_id,
    op.product_id,
    op.add_to_cart_order,
    op.reordered,
    op.source_set,
    o.user_id,
    o.order_number,
    o.order_dow,
    o.order_hour_of_day,
    o.days_since_prior_order
FROM stg_order_products op
JOIN stg_orders o ON op.order_id = o.order_id;
```

---

### fct_orders

One row per order. Aggregates basket size, reordered item count, and reorder percentage at the order level.

```sql
CREATE OR REPLACE TABLE fct_orders AS
SELECT
    o.order_id,
    o.user_id,
    o.order_number,
    o.order_dow,
    o.order_day_name,
    o.order_hour_of_day,
    o.time_of_day_bucket,
    o.days_since_prior_order,
    o.reorder_cycle_bucket,
    COUNT(op.product_id)                                    AS basket_size,
    SUM(op.reordered)                                       AS reordered_items,
    ROUND(
        100.0 * SUM(op.reordered) /
        NULLIF(COUNT(op.product_id), 0), 2
    )                                                       AS reorder_pct
FROM stg_orders o
JOIN stg_order_products op ON o.order_id = op.order_id
GROUP BY 1,2,3,4,5,6,7,8,9;
```

---

## 5. Mart Models

Mart models are the business-ready, aggregated tables that feed directly into Tableau. Three marts power the final dashboards.

---

### mart_product_performance

Product-level reorder loyalty, reach, and cart position. Feeds Dashboard 2 — What Customers Are Buying.

```sql
CREATE OR REPLACE TABLE mart_product_performance AS
SELECT
    p.product_id,
    p.product_name,
    p.department_name,
    p.aisle_name,
    p.is_organic,
    COUNT(DISTINCT fi.order_id)                              AS total_orders,
    COUNT(DISTINCT fi.user_id)                               AS unique_buyers,
    SUM(fi.reordered)                                        AS reorder_count,
    ROUND(100.0 * SUM(fi.reordered)
        / NULLIF(COUNT(fi.order_id), 0), 2)                 AS reorder_rate_pct,
    ROUND(AVG(fi.add_to_cart_order), 2)                     AS avg_cart_position,
    RANK() OVER (ORDER BY COUNT(DISTINCT fi.order_id) DESC)  AS popularity_rank,
    RANK() OVER (ORDER BY SUM(fi.reordered) DESC)           AS reorder_rank
FROM fct_order_items fi
JOIN dim_products p ON fi.product_id = p.product_id
GROUP BY 1,2,3,4,5;
```

---

### mart_timing_analysis

Hour × day aggregation for peak demand analysis. Feeds the heatmap, hourly line chart, and time-of-day breakdowns in Dashboard 1 — When Customers Shop.

```sql
CREATE OR REPLACE TABLE mart_timing_analysis AS
SELECT
    order_day_name,
    order_dow,
    order_hour_of_day,
    time_of_day_bucket,
    reorder_cycle_bucket,
    COUNT(order_id)                        AS order_count,
    COUNT(DISTINCT user_id)                AS unique_shoppers,
    ROUND(AVG(basket_size), 2)             AS avg_basket_size,
    ROUND(AVG(reorder_pct), 2)             AS avg_reorder_pct,
    SUM(basket_size)                       AS total_items_ordered
FROM fct_orders
GROUP BY 1,2,3,4,5
ORDER BY order_dow, order_hour_of_day;
```

---

### mart_user_rfm

User-level RFM (Recency × Frequency × Monetary) scoring using NTILE quintiles. One row per user with their score and named segment. Feeds Dashboard 3 — Shopper Loyalty.

```sql
CREATE OR REPLACE TABLE mart_user_rfm AS
WITH rfm_raw AS (
    SELECT
        user_id,
        MAX(order_number)        AS recency_score_raw,
        COUNT(DISTINCT order_id) AS frequency,
        AVG(basket_size)         AS monetary_proxy
    FROM fct_orders
    GROUP BY user_id
),
rfm_scored AS (
    SELECT
        user_id,
        frequency,
        ROUND(monetary_proxy, 2)                            AS avg_basket_size,
        NTILE(5) OVER (ORDER BY recency_score_raw DESC)     AS r_score,
        NTILE(5) OVER (ORDER BY frequency ASC)              AS f_score,
        NTILE(5) OVER (ORDER BY monetary_proxy ASC)         AS m_score
    FROM rfm_raw
)
SELECT
    user_id,
    frequency,
    avg_basket_size,
    r_score,
    f_score,
    m_score,
    r_score + f_score + m_score AS rfm_total,
    CASE
        WHEN r_score >= 4 AND f_score >= 4 THEN 'Champions'
        WHEN r_score >= 4 AND f_score >= 2 THEN 'Loyal Customers'
        WHEN r_score >= 3 AND f_score >= 1 THEN 'Potential Loyalists'
        WHEN r_score >= 4 AND f_score = 1  THEN 'Recent Customers'
        WHEN r_score <= 2 AND f_score >= 4 THEN 'At Risk'
        WHEN r_score <= 2 AND f_score >= 2 THEN 'Need Attention'
        WHEN r_score = 1  AND f_score = 1  THEN 'Lost'
        ELSE 'Hibernating'
    END AS rfm_segment
FROM rfm_scored;
```


---

[← Back to Project Overview](/instacart-analytics/){: .btn .btn--info .btn--large}
[View Full Tableau Story](https://public.tableau.com/app/profile/kristin.mooney/viz/InstacartAnalyticsProject/UnderstandingInstacartShopperBehavior){: .btn .btn--primary .btn--large}

