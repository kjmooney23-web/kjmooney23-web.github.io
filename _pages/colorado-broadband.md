---
layout: page
title: "Colorado's Broadband Opportunity"
subtitle: "Landscape, Competition & Expansion Strategy"
permalink: /colorado-broadband/
author_profile: true
toc: true
toc_label: "Project Sections"
toc_icon: "chart-bar"
---

## 1. Background & Overview

Colorado's broadband market is evolving fast. With fiber providers aggressively expanding into cable-dominated territories, the competitive dynamics are shifting in ways that create real risk for incumbents — and real opportunity for those paying attention.

This project was designed to get a clear picture of what's actually happening in the market. The goals were straightforward: understand the industry landscape, map the competitive dynamics between Cable and Fiber providers, assess where expansion opportunities exist across the state, and quantify the churn and penetration risks that come with increased competition.

Two datasets were used to accomplish this — FCC Broadband availability data covering over 2.7 million locations across Colorado, and the IBM Telco Customer Churn dataset to model retention risk behaviors. SQL was used to structure, clean, and analyze the data, and a four-dashboard Tableau story was built to communicate the findings visually.

The result is an end-to-end market analysis that goes beyond coverage maps — connecting infrastructure data to customer behavior to tell a more complete story.

---

## 2. Data Structure Overview

This project draws on two primary data sources: FCC Broadband Availability data (December 2025), which provides location-level infrastructure and provider information across Colorado, and the IBM Telco Customer Churn dataset (Kaggle, 2018), which supplies behavioral and demographic data used to model churn and retention risk. All data was structured and queried using SQL within a `fiber_analytics` MySQL database before being connected to Tableau for visualization.

The FCC data powers the first three dashboards; the IBM Telco dataset powers the fourth.

---

### Data Sources at a Glance

| Dashboard | Name | Source | CSV / Table(s) |
|---|---|---|---|
| 1 | Colorado's Broadband Landscape | FCC Broadband Data (Dec 2025) | `tech_overview` |
| 2 | Competitive Battleground | FCC Broadband Data (Dec 2025) | `fiber_and_cable_competitors` |
| 3 | Expansion Strategy in CO | FCC Broadband Data (Dec 2025) | `expansion_priority`, `cable_incumbents`, `county_competition` |
| 4 | Retention & Risk Strategy | IBM Telco Customer Churn (Kaggle, 2018) | `telco_churn` |

---

### Dashboard 1 — Colorado's Broadband Landscape
**Source tables:** `fcc_co_fiber`, `fcc_co_cable`

> **Key Stats:** 82 Fiber competitors vs. 10 Cable &nbsp;|&nbsp; Cable holds 59% market share across 2.78M locations &nbsp;|&nbsp; Fiber upload speeds are 13x faster than Cable

The two raw FCC tables were combined using a `UNION ALL` and grouped by technology type — filtering to technology codes `50` (Fiber FTTP) and `40` (Cable HFC) — to produce the `tech_overview` output. This yielded a clean side-by-side comparison of the two technology types across Colorado's 2.78M broadband locations.

| Field | Description |
|---|---|
| `technology_type` | Fiber (FTTP) or Cable (HFC) |
| `num_providers` | Count of distinct providers per technology |
| `locations_served` | Count of distinct locations served |
| `avg_download_mbps` | Average max advertised download speed |
| `avg_upload_mbps` | Average max advertised upload speed |

---

### Dashboard 2 — Competitive Battleground
**Source tables:** `fcc_co_fiber`, `fcc_co_cable`

> **Key Stats:** Xfinity serves 1.44M Cable locations; Quantum Fiber serves 548K Fiber locations &nbsp;|&nbsp; Most competitors offer upload speeds between 1,000–2,500 Mbps

The same two FCC tables were UNIONed again, this time grouped by `brand_name` rather than technology type, to produce the `fiber_and_cable_competitors` output. A Tableau calculated field was added to classify each provider into an upload speed performance tier:

IF [Avg Upload Mbps] >= 5000 THEN 'Best'
ELSEIF [Avg Upload Mbps] >= 2000 THEN 'Good'
ELSEIF [Avg Upload Mbps] >= 1000 THEN 'Average'
ELSEIF [Avg Upload Mbps] >= 500  THEN 'Bad'
ELSE 'Worst'
END


| Field | Description |
|---|---|
| `service` | Fiber or Cable label |
| `brand_name` | Provider brand name |
| `locations` | Count of distinct locations served per provider |
| `avg_download_mbps` | Average max advertised download speed |
| `avg_upload_mbps` | Average max advertised upload speed |
| `Upload Speed Category` | Calculated tier: Best / Good / Average / Bad / Worst |

---

### Dashboard 3 — Expansion Strategy in CO
**Source tables:** `fcc_co_fiber`, `fcc_co_cable`

> **Key Stats:** Adams and Douglas each have 67K+ cable-only locations &nbsp;|&nbsp; Upload speeds fall below 100 Mbps in Summit, Garfield, and Pitkin counties &nbsp;|&nbsp; The western slope is dominated by Cable

Three separate queries were built for this dashboard, each serving a distinct purpose in the expansion analysis:

- **`expansion_priority`** — Profiles eight target counties by volume of cable-only locations, cable-only percentage, and incumbent upload speed, establishing a priority ranking for fiber expansion opportunities.
- **`cable_incumbents`** — Pulls provider-level detail for those same eight counties from the raw FCC Cable table, enabling a view of who currently holds market control and at what service quality.
- **`county_competition`** — Covers all 64 Colorado counties, using LEFT JOINs to classify every location as fiber-only, cable-only, or served by both. Calculates `pct_cable_only` and `pct_has_fiber` at the county level to support the choropleth map.

| Field | Description |
|---|---|
| `county_name` / `county_fips` | County identifier |
| `cable_only_locations` | Locations with cable access but no fiber |
| `pct_cable_only` | % of county locations that are cable-only |
| `xfinity_avg_upload_mbps` | Incumbent upload speed benchmark |
| `cable_provider` | Brand name of cable provider serving the county |
| `has_fiber` / `has_cable` | Binary flags used to classify each location |
| `pct_has_fiber` | % of county locations with fiber access |

---

### Dashboard 4 — Retention & Risk Strategy
**Source table:** `telco_churn`

> **Key Stats:** Fiber churn rate is 42% — more than double Cable's 19% &nbsp;|&nbsp; 90% of churned Fiber customers are on month-to-month contracts &nbsp;|&nbsp; Automated payments correlate with significantly lower churn rates

The IBM Telco Customer Churn dataset was loaded as a single flat table. A single SQL query grouped customers by service type, contract length, payment method, and churn status, producing the aggregated metrics used across all visualizations in this dashboard.

| Field | Description |
|---|---|
| `InternetService` | Fiber or Cable |
| `contract` | Month-to-month, one-year, or two-year |
| `paymentmethod` | Bank transfer, credit card, electronic check, or mailed check |
| `churn` | Whether the customer churned (Yes / No) |
| `churned` | Count of churned customers |
| `churn_rate_pct` | Churn rate as a percentage |
| `avg_tenure_months` | Average customer tenure in months |
| `avg_monthly_charge` | Average monthly bill |
| `est_monthly_revenue_lost` | Estimated monthly revenue impact from churn |

---

## 3. Executive Summary

Colorado's broadband market is at an inflection point. Cable incumbents still hold the majority of
the market — 59% of 2.78M locations — but Fiber is rapidly closing the gap, backed by 82 competing
providers and speed capabilities that are categorically superior. The average Fiber upload speed in
Colorado is 3,588 Mbps, compared to just 283 Mbps for Cable — a 13x difference that is becoming
increasingly difficult for Cable providers to ignore as customer expectations rise.

The competitive landscape reveals a structural vulnerability for the market's biggest players.
Xfinity and Quantum Fiber dominate by volume, but both offer significantly slower upload speeds
than smaller, more agile competitors. This performance gap — where scale and speed are inversely
correlated — creates a clear opening for providers willing to compete on quality over footprint.

Geographically, eight counties have been identified as priority expansion targets for Fiber entry.
Adams and Douglas alone account for over 135,000 cable-only locations, while Summit, Garfield,
and Pitkin counties — home to some of Colorado's most prominent ski communities — are being
served by Cable upload speeds below 100 Mbps. The western slope of Colorado remains broadly
underserved, with cable-only coverage dominating across the region.

On the retention side, Fiber's 41.89% churn rate — more than double Cable's 18.96% — is the
project's most urgent finding. However, the data points to two highly actionable levers:
contract structure and payment behavior. Ninety percent of churned Fiber customers are on
month-to-month contracts, and customers paying by automated methods churn at significantly
lower rates than those using electronic or mailed checks. The churn problem is real, but it
is not intractable.


---

## 4. Insights Deep Dive

---

### Dashboard 1 — Colorado's Broadband Landscape

> **Key Finding:** Cable incumbents still own the majority of the market, but Fiber has a
significant speed advantage and far more competitors entering the space.

Cable currently holds 59% of Colorado's 2.78M broadband locations (1.64M) versus Fiber's 41%
(1.13M). Despite that market share gap, Fiber is competing on a fundamentally different
performance tier — with average upload speeds of 3,588 Mbps compared to Cable's 283 Mbps,
making Fiber upload speeds **13x faster** than Cable. The competitive field tells a similar
story: there are 82 distinct Fiber providers operating in Colorado versus only 10 Cable
providers, signaling that Fiber is already a crowded and aggressive growth market.

---

### Dashboard 2 — Competitive Battleground

> **Key Finding:** The biggest players are also the slowest — creating a performance gap
that smaller, faster providers can exploit.

Xfinity dominates Cable with 1.44M locations served, while Quantum Fiber leads the Fiber
segment with 548K locations. However, scale comes at a cost: the largest providers are
offering significantly slower upload speeds than their smaller competitors. Xfinity's average
upload speed sits around 531 Mbps, while smaller Fiber players like Force Broadband and BAM
Broadband deliver speeds of 6,000–10,000 Mbps. The majority of competitors cluster in the
Average upload speed tier (1,000–2,500 Mbps), leaving a clear performance gap at the top
of the market for differentiated providers to fill.

---

### Dashboard 3 — Expansion Strategy in CO

> **Key Finding:** Eight counties present immediate fiber expansion opportunities, with
Adams and Douglas alone representing over 135K underserved cable-only locations.

Adams and Douglas counties each have more than 67,000 cable-only locations — the largest
concentrations of locations currently served by Cable with no Fiber alternative. Beyond
volume, service quality adds urgency: in Summit, Garfield, and Pitkin counties — some of
Colorado's premier ski communities — cable upload speeds fall below 100 Mbps, with Summit
County's Xfinity service averaging just 38 Mbps. The western slope of Colorado is broadly
dominated by Cable, making it a structurally underserved region ripe for Fiber entry.

---

### Dashboard 4 — Retention & Risk Strategy

> **Key Finding:** Fiber has a churn problem — and it's largely driven by contract
flexibility and payment behavior, both of which are actionable.

Fiber customers churn at a rate of **41.89%**, more than double Cable's 18.96%. Fiber
customers are also paying significantly more — an average of $93.39 per month versus $58.38
for Cable — making churn an expensive problem. The data points to two clear levers:
**contract length** and **payment method**. Of Fiber customers who churn, 90% are on
month-to-month contracts, compared to just 2% on two-year contracts. On payment method,
customers using automated payments (bank transfer or credit card) churn at significantly
lower rates than those paying by electronic or mailed check — with electronic check Fiber
churn reaching as high as 53%.


---

## 5. Recommendations

*✏️ List your strategic recommendations based on the data.*

---

## 6. Caveats & Assumptions

*✏️ Note any data limitations, assumptions made, or areas for future improvement.*

---

## 7. SQL Queries

### tech_overview
```sql
#tech_overview
USE fiber_analytics;

SELECT
    CASE technology WHEN 50 THEN 'Fiber (FTTP)' WHEN 40 THEN 'Cable (HFC)' END AS technology_type,
    COUNT(DISTINCT provider_id)                                                  AS num_providers,
    COUNT(DISTINCT location_id)                                                  AS locations_served,
    ROUND(AVG(max_advertised_download_speed), 0)                                 AS avg_download_mbps,
    ROUND(AVG(max_advertised_upload_speed), 0)                                   AS avg_upload_mbps
FROM (
    SELECT technology, provider_id, location_id, max_advertised_download_speed, max_advertised_upload_speed FROM fcc_co_fiber
    UNION ALL
    SELECT technology, provider_id, location_id, max_advertised_download_speed, max_advertised_upload_speed FROM fcc_co_cable
) AS combined
GROUP BY technology;

### fiber_and_cable_competitors
```sql
USE fiber_analytics;

SELECT
'Fiber' as service,
    brand_name,
    COUNT(DISTINCT location_id)                  AS locations,
    ROUND(AVG(max_advertised_download_speed), 0) AS avg_download_mbps,
    ROUND(AVG(max_advertised_upload_speed), 0)   AS avg_upload_mbps
FROM fcc_co_fiber
GROUP BY brand_name
union all
SELECT
	'Cable' as service,
    brand_name,
    COUNT(DISTINCT location_id)                                  AS locations,
    ROUND(AVG(max_advertised_download_speed), 0)                 AS avg_download_mbps,
    ROUND(AVG(max_advertised_upload_speed), 0)                   AS avg_upload_mbps
FROM fcc_co_cable
GROUP BY brand_name
;

### expansion_priority
```sql
USE fiber_analytics;

SELECT
    county_name,
    county_fips,
    cable_only_locations,
    pct_cable_only,
    xfinity_avg_upload_mbps
FROM (
    SELECT 'Adams'      AS county_name, '001' AS county_fips, 67800 AS cable_only_locations, 42.4 AS pct_cable_only, 531.0 AS xfinity_avg_upload_mbps
    UNION ALL SELECT 'Douglas',         '035',                67444,                          57.3,                  150.0
    UNION ALL SELECT 'Broomfield',      '014',                14199,                          63.3,                  191.0
    UNION ALL SELECT 'Eagle',           '037',                15297,                          85.4,                  400.0
    UNION ALL SELECT 'Summit',          '117',                14047,                          89.4,                   38.0
    UNION ALL SELECT 'Garfield',        '045',                13230,                          78.1,                   49.0
    UNION ALL SELECT 'Pitkin',          '097',                 7268,                          97.1,                   89.0
    UNION ALL SELECT 'Fremont',         '043',                 9500,                          65.0,                 1000.0
) AS county_data
ORDER BY county_name asc;

### cable_incumbents
```sql
USE fiber_analytics;

SELECT
    CASE SUBSTRING(block_geoid, 3, 3)
        WHEN '001' THEN 'Adams'     WHEN '014' THEN 'Broomfield'
        WHEN '035' THEN 'Douglas'   WHEN '037' THEN 'Eagle'
        WHEN '043' THEN 'Fremont'   WHEN '045' THEN 'Garfield'
        WHEN '097' THEN 'Pitkin'    WHEN '117' THEN 'Summit'
    END                                                          AS county_name,
    SUBSTRING(block_geoid, 3, 3)                                 AS county_fips,
    brand_name                                                   AS cable_provider,
    COUNT(DISTINCT location_id)                                  AS locations_served,
    ROUND(AVG(max_advertised_download_speed), 0)                 AS avg_download_mbps,
    ROUND(AVG(max_advertised_upload_speed), 0)                   AS avg_upload_mbps
FROM fcc_co_cable
WHERE SUBSTRING(block_geoid, 3, 3) IN ('001','014','035','037','043','045','097','117')
GROUP BY county_fips, brand_name
ORDER BY county_fips, locations_served DESC;

### county_competition
```sql
USE fiber_analytics;

SELECT
    CASE SUBSTRING(block_geoid, 3, 3)
        WHEN '001' THEN 'Adams'          WHEN '003' THEN 'Alamosa'
        WHEN '005' THEN 'Arapahoe'       WHEN '007' THEN 'Archuleta'
        WHEN '009' THEN 'Baca'           WHEN '011' THEN 'Bent'
        WHEN '013' THEN 'Boulder'        WHEN '014' THEN 'Broomfield'
        WHEN '015' THEN 'Chaffee'        WHEN '017' THEN 'Cheyenne'
        WHEN '019' THEN 'Clear Creek'    WHEN '021' THEN 'Conejos'
        WHEN '023' THEN 'Costilla'       WHEN '025' THEN 'Crowley'
        WHEN '027' THEN 'Custer'         WHEN '029' THEN 'Delta'
        WHEN '031' THEN 'Denver'         WHEN '033' THEN 'Dolores'
        WHEN '035' THEN 'Douglas'        WHEN '037' THEN 'Eagle'
        WHEN '039' THEN 'Elbert'         WHEN '041' THEN 'El Paso'
        WHEN '043' THEN 'Fremont'        WHEN '045' THEN 'Garfield'
        WHEN '047' THEN 'Gilpin'         WHEN '049' THEN 'Grand'
        WHEN '051' THEN 'Gunnison'       WHEN '053' THEN 'Hinsdale'
        WHEN '055' THEN 'Huerfano'       WHEN '057' THEN 'Jackson'
        WHEN '059' THEN 'Jefferson'      WHEN '061' THEN 'Kiowa'
        WHEN '063' THEN 'Kit Carson'     WHEN '065' THEN 'Lake'
        WHEN '067' THEN 'La Plata'       WHEN '069' THEN 'Larimer'
        WHEN '071' THEN 'Las Animas'     WHEN '073' THEN 'Lincoln'
        WHEN '075' THEN 'Logan'          WHEN '077' THEN 'Mesa'
        WHEN '079' THEN 'Mineral'        WHEN '081' THEN 'Moffat'
        WHEN '083' THEN 'Montezuma'      WHEN '085' THEN 'Montrose'
        WHEN '087' THEN 'Morgan'         WHEN '089' THEN 'Otero'
        WHEN '091' THEN 'Ouray'          WHEN '093' THEN 'Park'
        WHEN '095' THEN 'Phillips'       WHEN '097' THEN 'Pitkin'
        WHEN '099' THEN 'Prowers'        WHEN '101' THEN 'Pueblo'
        WHEN '103' THEN 'Rio Blanco'     WHEN '105' THEN 'Rio Grande'
        WHEN '107' THEN 'Routt'          WHEN '109' THEN 'Saguache'
        WHEN '111' THEN 'San Juan'       WHEN '113' THEN 'San Miguel'
        WHEN '115' THEN 'Sedgwick'       WHEN '117' THEN 'Summit'
        WHEN '119' THEN 'Teller'         WHEN '121' THEN 'Washington'
        WHEN '123' THEN 'Weld'           WHEN '125' THEN 'Yuma'
        ELSE 'Other'
    END                                                                              AS county_name,
    SUBSTRING(block_geoid, 3, 3)                                                     AS county_fips,
    'Colorado'                                                                       AS state,
    COUNT(*)                                                                         AS total_locations,
    SUM(CASE WHEN has_fiber = 1 AND has_cable = 1 THEN 1 ELSE 0 END)               AS fiber_and_cable,
    SUM(CASE WHEN has_fiber = 1 AND has_cable = 0 THEN 1 ELSE 0 END)               AS fiber_only,
    SUM(CASE WHEN has_fiber = 0 AND has_cable = 1 THEN 1 ELSE 0 END)               AS cable_only,
    ROUND(SUM(CASE WHEN has_fiber = 0 AND has_cable = 1 THEN 1 ELSE 0 END)
          * 100.0 / COUNT(*), 1)                                                     AS pct_cable_only,
    ROUND(SUM(CASE WHEN has_fiber = 1 THEN 1 ELSE 0 END)
          * 100.0 / COUNT(*), 1)                                                     AS pct_has_fiber
FROM (
    SELECT
        a.location_id,
        a.block_geoid,
        CASE WHEN f.location_id IS NOT NULL THEN 1 ELSE 0 END AS has_fiber,
        CASE WHEN c.location_id IS NOT NULL THEN 1 ELSE 0 END AS has_cable
    FROM (
        SELECT location_id, block_geoid FROM fcc_co_fiber
        UNION
        SELECT location_id, block_geoid FROM fcc_co_cable
    ) AS a
    LEFT JOIN (SELECT DISTINCT location_id FROM fcc_co_fiber) AS f ON a.location_id = f.location_id
    LEFT JOIN (SELECT DISTINCT location_id FROM fcc_co_cable) AS c ON a.location_id = c.location_id
) AS competition
GROUP BY county_fips
ORDER BY pct_cable_only DESC;

### telco_churn
```sql
USE fiber_analytics;

SELECT
    InternetService,
    tenure,
    gender,
    partner,
    contract,
    paymentmethod,
    churn,
    COUNT(*)                                                        AS total_customers,
    SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END)                 AS churned,
    ROUND(SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END)
          * 100.0 / COUNT(*), 1)                                    AS churn_rate_pct,
    ROUND(AVG(tenure), 1)                                           AS avg_tenure_months,
    ROUND(AVG(MonthlyCharges), 2)                                   AS avg_monthly_charge,
    ROUND(SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END)
          * AVG(MonthlyCharges), 0)                                 AS est_monthly_revenue_lost
FROM telco_churn
GROUP BY InternetService,
    tenure,
    gender,
    partner,
    contract,
    paymentmethod,
    churn
ORDER BY churn_rate_pct DESC;

## 📊 Interactive Dashboard

[View Full Tableau Dashboard](https://public.tableau.com/app/profile/kristin.mooney/viz/TableauFiberAnalysis/ColoradosBroadbandOpportunity){: .btn .btn--primary .btn--large}

[View SQL Queries on GitHub](https://github.com/kjmooney23-web){: .btn .btn--info .btn--large}
