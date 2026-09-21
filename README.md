# Yelp Business Intelligence: Performance & Market Opportunity Analysis

An end-to-end data analytics project using Python, SQL Server, and Power BI to analyze business performance, customer engagement, market structure, and city-category opportunities using Yelp business data.

---

## 📄 Executive Summary

A one-page, stakeholder-facing recommendation memo distilling this analysis into a specific business decision for a non-technical audience:

**[View Executive Memo →](executive-summary/market_expansion_memo.pdf)**

---

## Business Problem

Yelp contains information on businesses, ratings, and customer reviews across different cities and business categories. However, raw business data does not directly reveal where business activity is concentrated, which categories attract stronger customer engagement, or where potential market opportunities exist.

This project uses data analytics to transform the Yelp business dataset into actionable insights by examining market presence, customer engagement, business performance, and city-category opportunities.

## Project Objectives

- Analyze business distribution across cities and categories.
- Identify cities with strong average business ratings.
- Measure customer engagement using review activity.
- Identify high-performing businesses and businesses with high review volume but lower ratings.
- Identify highly rated businesses with relatively low review visibility.
- Identify city-category combinations with strong ratings and customer engagement as potential market opportunities.
- Develop an interactive Power BI dashboard to communicate the findings.

## Business Questions

**Market Analysis**
1. Which cities have the highest number of businesses?
2. Which cities have the highest average rating?
3. Which cities have the highest customer engagement based on review volume?
4. Which cities have high business concentration but relatively low customer engagement?

**Category Analysis**
5. Which business categories have the largest business presence?
6. Which categories have the highest average rating?
7. Which categories generate the highest review volume?
8. Which categories combine strong ratings with strong customer engagement?

**Business Performance**
9. Which businesses are the top performers?
10. Which highly reviewed businesses have below-average ratings?
11. Which businesses have high ratings but relatively low review volume?

**Market Opportunity**
12. Which city-category combinations show the strongest business opportunity?

## Dataset

The project uses the **Yelp Open Dataset**, focusing on the Business dataset.

The analysis uses business-level information including: business ID, name, address, city, state, postal code, latitude/longitude, star rating, review count, open/closed status, and business categories.

**Data Source:** [Yelp Open Dataset on Kaggle](https://www.kaggle.com/datasets/yelp-dataset/yelp-dataset)

## Data Preparation

The raw business data (150,346 records) was processed using Python and Pandas before loading it into SQL Server.

Key preparation steps included:

- **Missing categories:** 103 records (0.07% of the dataset) had no assigned category and were removed, since category is required for all downstream category-level analysis. This left 150,243 usable records.
- **Whitespace standardization:** 49 city values and 150 business name values had leading/trailing whitespace, cleaned to prevent duplicate groupings in aggregation queries.
- **Business status field:** Created a readable `business_status` field ("Open"/"Closed") from the raw `is_open` binary indicator.
- **Category normalization:** The raw `categories` field (a single comma-separated string per business) was split and normalized into a proper category dimension table and a business-category relationship (junction) table, supporting clean relational joins in SQL Server and Power BI.
- **Data validation:** Confirmed zero duplicate business IDs, zero ratings outside the valid 1–5 range, zero negative review counts, and zero invalid `is_open` values before proceeding to analysis.
- **Cross-platform validation:** After loading into SQL Server, row counts were validated programmatically against the Python-side counts for every table (business, category, business-category) to confirm no data was lost or duplicated during the transfer.

## Data Integration & Connectivity

The project demonstrates data integration across multiple analytics platforms:

- **Kaggle API → Python:** Connected to the Kaggle API to programmatically acquire the Yelp dataset.
- **Python → SQL Server:** Connected Python to SQL Server using SQLAlchemy and PyODBC to load cleaned and transformed datasets.
- **SQL Server → Power BI:** Connected Power BI to SQL Server analysis views for interactive reporting and visualization.

## Final Analytical Dataset

| Dataset | Records |
|---|---|
| Business | 150,243 |
| Category | 1,311 |
| Business-Category Relationships | 668,549 |

## Tools & Technologies

| Area | Tools |
|---|---|
| Data Acquisition | Kaggle API |
| Data Preparation | Python, Pandas, NumPy |
| Database | Microsoft SQL Server |
| SQL Analysis | T-SQL, CTEs, Joins, Aggregations, Views |
| Data Visualization | Microsoft Power BI |
| Data Modelling | Power BI Data Model, DAX |
| Version Control | Git, GitHub |

## Project Workflow

**Data Acquisition → Data Preparation → SQL Database → Data Validation → Business Analysis → Power BI Dashboard → Business Insights**

## SQL Analysis

The cleaned datasets were loaded into Microsoft SQL Server for structured business analysis and validation, organized into four analytical areas:

**Market Analysis** — Business concentration across cities and states; average business ratings by city; customer engagement based on review volume; markets with high business concentration but relatively low customer engagement.

**Category Analysis** — Business presence by category; average rating by category; total review volume by category; categories combining strong ratings with strong customer engagement.

**Business Performance** — Identification of top-performing businesses using rating and review volume; highly reviewed businesses with below-average ratings; highly rated businesses with relatively low review volume.

**Market Opportunity** — Identification of city-category combinations with strong business presence, ratings, and customer engagement.

### Market Opportunity — Defined Criteria

A city-category combination is classified as a **market opportunity** if it meets all of the following thresholds:

- At least **10 businesses** in that city-category combination (ensures the signal isn't driven by one or two outlier businesses)
- Average rating of **4.0 or higher**
- Average of **50+ reviews per business**

This combination of volume, satisfaction, and engagement thresholds is designed to surface segments with *proven, validated* demand — not just a single standout business skewing the average.

### SQL Views

Three analytical views were created to support reporting and Power BI:

| SQL View | Purpose |
|---|---|
| `vw_Business_Analysis` | Business-level analysis and reporting |
| `vw_Category_Analysis` | Category-level performance and engagement analysis |
| `vw_Market_Opportunity` | City-category opportunity analysis |

## Power BI Dashboard

An interactive Power BI dashboard was developed to present the analytical results through an executive-level view of market presence, customer engagement, business performance, and market opportunities.

### Dashboard Components

- **KPI Cards** — Total Businesses, Average Rating, Total Reviews
- **Market Analysis** — Business Presence by City, Top 10 Cities by Average Rating
- **Category Analysis** — Top 10 Business Categories by Presence, Top 10 Categories by Customer Engagement
- **Business Performance** — Highly Reviewed Businesses with Below-Average Ratings
- **Market Opportunity** — Market Opportunity by City & Category

### Interactive Features

- City, State, and Business Status slicers
- Interactive filtering across dashboard visuals
- SQL Server-connected Power BI reporting

### Dashboard Preview

![Yelp Business Intelligence - Performance & Market Opportunity Analysis](Dashboard/dashboard_preview.png)

## Key Findings

### Market

- Philadelphia has the highest business presence in the dataset, with **14,566 businesses**.
- Safety Harbor, Florida records the highest average rating among cities with at least 100 businesses, at **4.13**.
- Philadelphia has the highest total review volume, with approximately **936K reviews**.
- New Orleans records approximately **100 reviews per business**, indicating strong customer engagement intensity.

### Categories

- **Restaurants** is the largest business category, with **52,268 businesses**.
- **Seafood** and **American (New)** show high customer engagement, with approximately **169** and **155 reviews per business**, respectively.
- Categories such as Walking Tours, Historical Tours, Distilleries, and Coffee Roasteries combine strong ratings with meaningful customer engagement.

### Business Performance

- The performance analysis identifies businesses with strong combinations of ratings and review volume.
- Several highly reviewed businesses have ratings below the overall dataset average of approximately **3.60**.
- Highly rated businesses with relatively low review volumes were identified as potential **hidden-visibility opportunities**.

### Market Opportunity

- **561 city-category combinations** met the defined opportunity criteria (see [Market Opportunity — Defined Criteria](#market-opportunity--defined-criteria) above).
- The strongest opportunities by rating and engagement include:

| City | Category | Businesses | Avg Rating | Total Reviews |
|---|---|---|---|---|
| New Orleans, LA | Historical Tours | 54 | 4.59 | 9,663 |
| New Orleans, LA | Walking Tours | 44 | 4.55 | 7,783 |
| Santa Barbara, CA | Tours | 86 | 4.65 | 5,949 |
| Santa Barbara, CA | Wine Tours | 55 | 4.70 | 2,962 |
| Nashville, TN | Historical Tours | 21 | 4.55 | 3,035 |

- New Orleans' Historical Tours segment alone generated 9,663 reviews — over 200x the dataset's per-business average — while maintaining a 4.59 rating, well above the overall market baseline of 3.60.
- Several strong-performing segments (e.g., New Orleans Food Tours: 13 businesses, 4.73 rating; New Orleans Rafting/Kayaking: 10 businesses, 4.70 rating) combine top-tier satisfaction with low competitive density — a pattern indicating headroom for new entrants rather than market saturation.

## Limitations

Yelp's dataset skews toward US metro areas with high app adoption and tourism activity; smaller cities, rural markets, and non-English-speaking regions are underrepresented. Ratings and review counts also reflect visibility and marketing reach, not purely service quality — a business with strong SEO or tourist foot traffic may outperform a comparable business in a lower-traffic location. Findings from this analysis should inform prioritization and further investigation, not replace on-the-ground due diligence.

## Repository Structure

```
Yelp-Business-Intelligence-Performance-Market-Opportunity-Analysis/
│
├── README.md
│
├── python/
│   └── Yelp_Business_Analytics.ipynb
│
├── sql/
│   └── Yelp_Business_Analytics.sql
│
├── Dashboard/
│   ├── Yelp_Business_Analytics.pbix
│   └── dashboard_preview.png
│
├── executive-summary/
│   └── market_expansion_memo.pdf
│
└── docs/
    └── data_dictionary.md
```

## Author

**Vyshnav P S** | Data Analyst

*This project is part of my data analytics portfolio.*
