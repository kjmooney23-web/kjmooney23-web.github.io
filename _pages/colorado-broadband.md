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

Colorado's broadband market is evolving fast. With fiber providers aggressively expanding into cable-dominated territories, the competitive dynamics are shifting in ways that create real risk for incumbents — and real opportunity for the fiber players.

This project was designed to get a clear picture of what's actually happening in the market. The goals were straightforward: understand the industry landscape, map the competitive dynamics between Cable and Fiber providers, assess where expansion opportunities exist across the state, and quantify the churn and penetration risks that come with increased competition.

Two datasets were used to accomplish this — FCC Broadband availability data covering over 2.7 million locations across Colorado, and the IBM Telco Customer Churn dataset to model retention risk behaviors. SQL was used to structure, clean, and analyze the data, and a four-dashboard Tableau story was built to communicate the findings visually.

The result is an end-to-end market analysis that goes beyond coverage maps — connecting infrastructure data to customer behavior to tell a more complete story.

---

## 2. Executive Summary

Colorado's broadband market is at an inflection point. Cable incumbents still hold the majority of
the market — 59% of 2.78M locations — but Fiber is rapidly closing the gap, backed by 82 competing
providers with speed capabilities that are categorically superior. The average Fiber upload speed in
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
lower rates than those using electronic or mailed checks. The churn problem is real, but there are ways to manage the risk.


---

## 3. Insights Deep Dive

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
## 4. Recommendations

---

### 1. Prioritize Fiber Expansion in Adams and Douglas Counties
With 67,800 and 67,444 cable-only locations respectively, Adams and Douglas represent the
single largest untapped opportunity for Fiber entry in Colorado. Both counties are in the
metro area with high population density, making infrastructure investment more efficient
per location served. These should be the first targets for any expansion roadmap.

---

### 2. Target Underserved Ski Communities as a Premium Market
Summit, Garfield, and Pitkin counties are being served by Cable upload speeds of 38, 49,
and 89 Mbps respectively — far below what remote work and high-demand streaming require.
These communities skew toward high-income households with both the willingness and ability
to pay a premium for superior service. A quality-differentiated Fiber offering in these
markets could command strong pricing power with relatively low competitive resistance.

---

### 3. Treat the Western Slope as a Strategic Long-Term Opportunity
The western slope of Colorado is broadly dominated by Cable with minimal Fiber penetration.
While infrastructure costs are higher in rural and mountainous terrain, the lack of
competition means any Fiber provider entering this region could establish durable market
leadership. A phased western slope strategy — starting with the most accessible
population centers — would be worth modeling as a longer-term investment.

---

### 4. Compete on Upload Speed, Not Just Download Speed
The data consistently shows that the biggest providers are the slowest on upload speeds.
As remote work, video conferencing, and cloud-based workflows become standard, upload speed
is increasingly the metric customers care about. A Fiber provider that leads with upload
speed as a core differentiator — rather than competing on price — can occupy a distinct
and defensible position in the market.

---

### 5. Incentivize Longer-Term Contracts to Reduce Fiber Churn
Ninety percent of churned Fiber customers are on month-to-month contracts. Offering
meaningful incentives to migrate customers to one- or two-year agreements — such as locked
pricing, installation credits, or bundled services — could significantly reduce the 41.89%
churn rate. Even shifting a portion of the month-to-month base to annual contracts would
have a material impact on revenue retention.

---

### 6. Promote Automated Payment Enrollment as a Retention Tool
Customers using automated payment methods (bank transfer or credit card) churn at
significantly lower rates than those paying by electronic or mailed check. A proactive
campaign to enroll customers in autopay at onboarding — paired with a small monthly
discount as an incentive — is a low-cost, high-return retention lever that can be
implemented immediately.

---

## 5. Caveats & Assumptions

---

### Data Source Limitations

**FCC Broadband Availability Data**
The FCC dataset reflects *advertised* maximum speeds, not actual measured speeds experienced
by customers. Providers are known to advertise theoretical peak performance, which may
differ significantly from real-world conditions. Additionally, FCC availability data reports
whether a provider *can* serve a location, not whether a customer is actively subscribed —
meaning coverage and market share are not the same thing. Actual penetration rates within
served areas are unknown.

**IBM Telco Customer Churn Dataset**
The churn dataset is sourced from a single telecommunications company's 2018 customer base
and was not collected in Colorado. It is used here as a behavioral proxy to understand
churn drivers across contract types and payment methods — not as a direct measurement of
Colorado broadband churn. Churn rates and behavioral patterns may differ across geographies,
time periods, and provider types.

---

### Analytical Assumptions

- Technology codes `50` (Fiber FTTP) and `40` (Cable HFC) were used exclusively to define
  Fiber and Cable, respectively. Other technology types (DSL, fixed wireless, satellite)
  were excluded from the analysis to maintain a clean Cable vs. Fiber comparison.
- The eight counties selected for the Expansion Strategy dashboard were chosen based on a
  combination of cable-only location volume and incumbent upload speed performance.
  Other counties may present viable opportunities not captured in this analysis.
- The `expansion_priority` table uses manually curated county-level values derived from
  the raw FCC data. These figures are point-in-time snapshots and may not reflect the
  most current deployment status.

---

### Areas for Future Improvement

- **Incorporate actual subscription data** alongside availability data to calculate true
  penetration rates by county and provider.
- **Add pricing data** to better model the relationship between monthly charges, contract
  type, and churn risk in a Colorado-specific context.
- **Expand geographic scope** of the churn analysis using a more recent and regionally
  representative dataset.
- **Layer in demographic and income data** by county to refine the expansion priority
  scoring model and better assess willingness-to-pay in target markets.

---

## Interactive Dashboard

[View Full Tableau Dashboard](https://public.tableau.com/app/profile/kristin.mooney/viz/TableauFiberAnalysis/ColoradosBroadbandOpportunity){: .btn .btn--primary .btn--large}

---

## 6. Data Structure Overview

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

## 7. SQL Queries

All SQL queries used in this project are available on a dedicated page, organized by dashboard.
Each query includes a brief description of its purpose.

[View SQL Queries →](/colorado-broadband-sql/){: .btn .btn--info .btn--large}
