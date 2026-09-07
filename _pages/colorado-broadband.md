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

This project draws on two primary data sources: FCC Broadband Availability data (December 2025), which provides location-level infrastructure and provider information across Colorado, and the IBM Telco Customer Churn dataset (Kaggle, 2018), which supplies behavioral and demographic data used to model churn and retention risk. All data was structured and queried using SQL within a fiber_analytics MySQL database before being connected to Tableau for visualization.

The FCC data powers the first three dashboards; the IBM Telco dataset powers the fourth.

| Dashboard | Name | Source | CSV / Table(s) |
| --- | --- | --- | --- |
| 1 | Colorado's Broadband Landscape | FCC Broadband Data (Dec 2025) | ``tech_overview`` |
| 2 | Competitive Battleground | FCC Broadband Data (Dec 2025) | ``fiber_and_cable_competitors`` |
| 3 | Expansion Strategy in CO | FCC Broadband Data (Dec 2025) | ``expansion_priority``, ``cable_incumbents``, ``county_competition`` |
| 4 | Retention & Risk Strategy | IBM Telco Customer Churn (Kaggle, 2018) | ``telco_churn`` |

Dashboard 1 — Colorado's Broadband Landscape
Source tables: fcc_co_fiber, fcc_co_cable

The two raw FCC tables were combined using a UNION ALL and grouped by technology type — filtering to technology codes 50 (Fiber FTTP) and 40 (Cable HFC) — to produce the tech_overview output. This yielded a clean side-by-side comparison of the two technology types across Colorado's 2.78M broadband locations.

Key fields:
| Field | Description |
| --- | --- |
| ``technology_type`` | Fiber (FTTP) or Cable (HFC) |
| ``num_providers`` | Count of distinct providers per technology |
| ``locations_served`` | Count of distinct locations served |
| ``avg_download_mbps`` | Average max advertised download speed |
| ``avg_upload_mbps`` | Average max advertised upload speed |

Dashboard 2 — Competitive Battleground
Source tables: fcc_co_fiber, fcc_co_cable

The same two FCC tables were UNIONed again, this time grouped by brand_name rather than technology type, to produce the fiber_and_cable_competitors output. This enabled a provider-level competitive analysis across both Fiber and Cable. A Tableau calculated field was added to classify each provider into an upload speed performance tier:

IF [Avg Upload Mbps] >= 5000 THEN 'Best'
ELSEIF [Avg Upload Mbps] >= 2000 THEN 'Good'
ELSEIF [Avg Upload Mbps] >= 1000 THEN 'Average'
ELSEIF [Avg Upload Mbps] >= 500  THEN 'Bad'
ELSE 'Worst'
END

Key fields:
| Field | Description |
| --- | --- |
| ``service`` | Fiber or Cable label |
| ``brand_name`` | Provider brand name |
| ``locations`` | Count of distinct locations served per provider |
| ``avg_download_mbps`` | Average max advertised download speed |
| ``avg_upload_mbps`` | Average max advertised upload speed |
| ``Upload ``Speed ``Category`` | Calculated Tableau tier: Best / Good / Average / Bad / Worst |

Dashboard 3 — Expansion Strategy in CO
Source tables: fcc_co_fiber, fcc_co_cable

Three separate queries were built for this dashboard, each serving a distinct purpose in the expansion analysis:

expansion_priority — Profiles eight target counties by volume of cable-only locations, cable-only percentage, and incumbent upload speed, establishing a priority ranking for fiber expansion opportunities.

cable_incumbents — Pulls provider-level detail for those same eight counties from the raw FCC Cable table, enabling a view of who currently holds market control and at what service quality.

county_competition — Covers all 64 Colorado counties, using LEFT JOINs to classify every location as fiber-only, cable-only, or served by both. Calculates pct_cable_only and pct_has_fiber at the county level to support the choropleth map.

Key fields:

| Field | Description |
| --- | --- |
| ``county_name`` / ``county_fips`` | County identifier |
| ``cable_only_locations`` | Locations with cable access but no fiber |
| ``pct_cable_only`` | % of county locations that are cable-only |
| ``xfinity_avg_upload_mbps`` | Incumbent upload speed benchmark |
| ``cable_provider`` | Brand name of cable provider serving the county |
| ``has_fiber`` / ``has_cable`` | Binary flags used to classify each location |
| ``pct_has_fiber`` | % of county locations with fiber access |

Dashboard 4 — Retention & Risk Strategy
Source table: telco_churn

The IBM Telco Customer Churn dataset was loaded as a single flat table. A single SQL query grouped customers by service type, contract length, payment method, and churn status, producing the aggregated metrics used across all visualizations in this dashboard.

Key fields:
| Field | Description |
| --- | --- |
| ``InternetService`` | Fiber or Cable |
| ``contract`` | Month-to-month, one-year, or two-year |
| ``paymentmethod`` | Bank transfer, credit card, electronic check, or mailed check |
| ``churn`` | Whether the customer churned (Yes / No) |
| ``churned`` | Count of churned customers |
| ``churn_rate_pct`` | Churn rate as a percentage |
| ``avg_tenure_months`` | Average customer tenure in months |
| ``avg_monthly_charge`` | Average monthly bill |
| ``est_monthly_revenue_lost`` | Estimated monthly revenue impact from churn |


*(Add your ERD image below by uploading it to your repo and referencing it like this:)*

![ERD Diagram](/assets/images/erd.png)

---

## 3. Executive Summary

*✏️ Summarize your key findings and the 4 dashboards you built.*

---

## 4. Insights Deep Dive

*✏️ Walk through your most important analytical findings here.*

---

## 5. Recommendations

*✏️ List your strategic recommendations based on the data.*

---

## 6. Caveats & Assumptions

*✏️ Note any data limitations, assumptions made, or areas for future improvement.*

---

## 📊 Interactive Dashboard

[View Full Tableau Dashboard](https://public.tableau.com/app/profile/kristin.mooney/viz/TableauFiberAnalysis/ColoradosBroadbandOpportunity){: .btn .btn--primary .btn--large}

[View SQL Queries on GitHub](https://github.com/kjmooney23-web){: .btn .btn--info .btn--large}
