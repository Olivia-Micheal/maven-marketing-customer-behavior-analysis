# Maven Marketing: Customer Behaviour and Revenue Analysis
### A Power BI analysis of 2,237 customers covering demographics, campaign engagement, product performance, channel effectiveness and web purchasing behaviour

---

I assumed customers with children would be Maven Marketing's most active online shoppers.

They are not.

72% of Maven Marketing's customers have children at home. They show stronger campaign engagement. They represent the majority of the customer base. On paper they look like the perfect target audience.

But when I looked at actual web purchases, customers without children averaged 4.4 purchases compared to 4.0 for customers with children.

That single finding changed how I approached everything else in this analysis. Not just building a dashboard. But letting data challenge what you think you already know.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Business Problem and Questions](#business-problem-and-questions)
3. [Tools and Skills](#tools-and-skills)
4. [Dataset Description](#dataset-description)
5. [Data Cleaning and Transformation](#data-cleaning-and-transformation)
6. [Data Model](#data-model)
7. [Dashboard Pages](#dashboard-pages)
8. [Key Insights and Findings](#key-insights-and-findings)
9. [What the Data Got Interesting](#what-the-data-got-interesting)
10. [Summary and Conclusion](#summary-and-conclusion)
11. [Recommendations](#recommendations)
12. [Live Dashboard](#live-dashboard)
13. [Author and Contact](#author-and-contact)

---

## Project Overview

This project was completed as part of the Digitaley Drive Data Analytics Bootcamp using Microsoft Power BI.

Maven Marketing provided a dataset of 2,237 customers covering demographics, campaign responses, product spending, channel activity and web purchasing behaviour. The goal was to analyze customer behaviour patterns and deliver actionable insights to support smarter marketing, product and channel decisions.

The final output is a 5-page interactive Power BI dashboard built on a normalized star schema data model with custom DAX measures and dynamic page navigation.


<img width="1520" height="685" alt="Digital overview" src="https://github.com/user-attachments/assets/61498092-de35-4e64-8db3-79b0c63ee514" />

*Figure 1: Executive Overview Dashboard*


---

## Business Problem and Questions

Maven Marketing needed to move beyond surface-level reporting and understand the real drivers behind customer engagement and revenue. Five business questions guided the entire analysis.

1. What does the average Maven Marketing customer look like?
2. Which marketing campaign was the most successful?
3. Which products are performing best?
4. Which channels are underperforming?
5. What factors are significantly related to the number of web purchases?

Every dashboard page, every visual and every DAX measure was built to answer one of these questions. Not to look impressive. To be useful.

---

## Tools and Skills

| Tool | Purpose |
|---|---|
| Power BI Desktop | Dashboard design and visualization |
| Power Query | Data cleaning and transformation |
| DAX | Calculated measures and KPIs |
| Data Modeling | Star schema design and table relationships |

**Skills demonstrated:**
Data Cleaning, Outlier Detection, Median Imputation, Data Normalization, Star Schema Modeling, DAX Measure Writing, Customer Segmentation, Marketing Analytics, Dashboard Design, Data Storytelling

---

## Dataset Description

- **Source:** Maven Marketing customer dataset
- **Records:** 2,237 customers
- **Fields:** 28 columns
- **Format:** Single flat CSV file

The dataset covered customer demographics including age, income, education, marital status, household composition and country. It also contained six marketing campaign response columns, six product spending columns, four channel purchase columns and web visit behaviour data.


<img width="1917" height="762" alt="maven raw data" src="https://github.com/user-attachments/assets/55fbc9a8-900f-4b74-b219-015141045f75" />

*Figure 2: Raw Dataset (Flat Table before Transformation)*


This is how the data originally came in. One flat table with campaign responses, product spending and channel purchases all stored as separate columns with no structure for cross-dimensional analysis.

---

## Data Cleaning and Transformation

The dataset arrived as one flat table with several quality issues that needed to be resolved before any analysis could begin.

**Issues identified and resolved:**

| Issue | Detail | Resolution |
|---|---|---|
| Missing Income values | 24 null entries across 2,237 rows | Replaced with median income of $51,000 |
| Income outlier | One customer recorded with income of $666,666, more than 4x the next highest value of $162,397 | Row removed |
| Age outlier | One customer born in 1893, producing an impossible age of 131 years | Row removed |
| Invalid Marital Status | Entries labelled Absurd and YOLO present in the marital status column | Replaced with Other |
| Incorrect data types | Multiple columns loaded with wrong data types | Corrected across all affected columns in Power Query |

After cleaning, 2,237 rows were reduced to **2,235 clean rows** ready for modeling.

**Transformation approach:**

The original flat table contained campaign responses, product spending and channel purchases all stored in separate columns making cross-dimensional analysis impossible. Each group was unpivoted into a dedicated fact table.

- Six campaign columns unpivoted into DimCampaign_Table (Campaign Name, Acceptance: 0 or 1)
- Six product columns unpivoted into DimProduct_Table (Product Name, Revenue)
- Four channel columns unpivoted into DimChannel_Table (Channel Name, Purchases)

**Calculated columns added:**

- Age = 2014 minus Year of Birth based on the most recent customer join date in the dataset
- Age Group = Young Adults (18 to 30), Adults (31 to 45), Seniors (46 to 60), Elderly (60+)
- Income Category = Low (below $30,000), Medium ($30,000 to $70,000), High (above $70,000)
- Kid Category = With Kids (Kidhome + Teenhome greater than 0), No Kids


<img width="1896" height="1001" alt="power query maven" src="https://github.com/user-attachments/assets/400247ca-2dfe-494e-9d9f-7559fb36cb63" />

*Figure 3: Power Query Editor showing Applied Steps*


---

## Data Model

The flat table was restructured into a star schema with four tables.

| Table | Type | Purpose |
|---|---|---|
| Fact marketing_data | Dimension | Customer demographics, profile and calculated columns |
| DimCampaign_Table | Fact | Campaign name and acceptance per customer per campaign |
| DimProduct_Table | Fact | Product name and revenue per customer per product |
| DimChannel_Table | Fact | Channel name and purchase count per customer per channel |

All three fact tables connect to the central customer table via One to Many relationships using customer ID as the key. This structure allows customer demographic filters to interact with campaign, product and channel data across the entire dashboard.

**Key DAX measures created:**

Total Revenue, Total Purchases, Total Campaign Acceptances, Campaign Response Rate %, Average Age, Average Income, % Customers with Kids, % Married Customers, Average Web Purchases per Customer, % Online Buyers, Average Income of Web Buyers, Avg Spend per Customer by Product, Customers with Web Purchases.


<img width="1877" height="993" alt="Power bi model view" src="https://github.com/user-attachments/assets/cb17e01f-3878-419e-a0bc-efa342e133bc" />

*Figure 4: Power BI Model View showing Star Schema*


---

## Dashboard Pages

The dashboard tells a single connected story across 5 pages with dynamic navigation buttons, dropdown slicers and a consistent dark teal theme throughout.

**Page 1: Executive Overview**

High level snapshot of all key metrics in one view. Total Revenue of $1M, Total Purchases of 33K, Campaign Response Rate of 45%, Average Customer Age of 45, Average Income of $52K and Total Customers of 2,237. Supported by Revenue by Product and Channel Performance overview charts.


<img width="1520" height="685" alt="Digital overview" src="https://github.com/user-attachments/assets/e32c9432-d75d-4f04-a397-d77ecaf94176" />

*Figure 5: Executive Overview Page*


**Page 2: Customer Profile and Demographics**

Answers Q1. Breaks down the 2,237 customer base by education level, income category, age group, marital status, kids category and country of origin. Includes an interactive globe map showing customer geographic distribution and a ranked country bar chart.


<img width="1521" height="682" alt="Digital Customer" src="https://github.com/user-attachments/assets/74d9ce61-7802-4f88-b820-7226b072960d" />

*Figure 6: Customer Profile Page*


**Page 3: Campaign Response and Engagement**

Answers Q2. Shows campaign acceptance comparison across all six campaigns, response rates broken down by education level and marital status, a donut chart showing campaign engagement by kids category and a scatter plot examining the relationship between income and campaign response rate.


<img width="1512" height="677" alt="Digital Campaign " src="https://github.com/user-attachments/assets/1f578a27-852f-493c-b7d4-8daf9d863bbd" />

*Figure 7: Campaign Performance Page*


**Page 4: Commercial Performance**

Answers Q3 and Q4. Bar charts comparing all six product categories by total revenue and all four channels by total purchase volume. Donut chart showing channel share as a percentage of total purchases.


<img width="1512" height="685" alt="Digital sales performance" src="https://github.com/user-attachments/assets/aaac9c2a-1a01-4f41-83ad-ec2119c5d8cd" />

*Figure 8: Sales Performance Page*


**Page 5: Web Purchase Behaviour**

Answers Q5. Analyzes average web purchases broken down by income category, education level, age group and kids category. Includes a scatter plot of individual customer income versus average web purchases.

<img width="1526" height="682" alt="Driver Digital 5" src="https://github.com/user-attachments/assets/a991bb08-b309-4599-affe-d286bfdeaa1d" />

*Figure 9: Web Behaviour Page*


---

## Key Insights and Findings

**Customer Profile**

The average Maven Marketing customer is a 45-year-old married graduate earning approximately $52,000 annually.

- 72% of customers have children at home (1,610 out of 2,237 customers)
- 39% of customers are married (864 customers)
- 1,126 customers hold a graduation-level degree, the largest education group
- 1,165 customers fall in the medium income bracket ($30,000 to $70,000)
- Spain leads with 1,094 customers, nearly half the entire customer base
- Saudi Arabia follows with 335 customers, then Canada with 268

This is a predominantly middle-aged, educated, mid-income, family-oriented customer base concentrated in Spain.

**Campaign Performance**

The final campaign (Response) was the strongest performer with 334 acceptances.

| Campaign | Acceptances |
|---|---|
| Response (Last Campaign) | 334 |
| Campaign 4 | 167 |
| Campaign 3 | 163 |
| Campaign 5 | 162 |
| Campaign 1 | 144 |
| Campaign 2 | 30 |

Overall campaign response rate: 45%

Campaign 2 recorded only 30 acceptances from 2,237 customers, a response rate of just 1.3%. The final campaign outperformed it by 11 times with exactly the same customer base.

Education had a clear effect on campaign response:

| Education Level | Campaign Response Rate |
|---|---|
| PhD | 54% |
| Graduation | 44% |
| Master | 43% |
| 2n Cycle | 36% |
| Basic | 15% |

A 39 percentage point gap separates the most and least responsive education groups.

**Product Performance**

| Product | Total Revenue |
|---|---|
| Wine | $680,000 |
| Meat | $373,000 |
| Gold | $98,000 |
| Fish | $84,000 |
| Sweets | $61,000 |
| Fruits | $59,000 |

Wine generates approximately 50% of total product revenue alone. Wine and Meat combined account for nearly 80% of all revenue.

**Channel Performance**

| Channel | Total Purchases |
|---|---|
| Store | 13,000 |
| Web | 9,100 |
| Catalog | 6,000 |
| Deals | 5,200 |

The physical store is the dominant channel at 39% of all purchases. Deals is the weakest at 16% of total purchases. Customers are largely ignoring discount-based promotions.

**Web Purchase Behaviour**

98% of customers have made at least one web purchase. Total web purchases across the customer base: 9,100 transactions.

Average web purchases by income category:

| Income Category | Average Web Purchases |
|---|---|
| Medium ($30K to $70K) | 5.4 |
| High (above $70K) | 4.7 |
| Low (below $30K) | 2.6 |

Average web purchases by age group:

| Age Group | Average Web Purchases |
|---|---|
| Seniors (46 to 60) | 4.6 |
| Mid-Age (31 to 45) | 4.5 |
| Adults | 3.9 |
| Young Adults (18 to 30) | 3.3 |

Average web purchases by education:

| Education | Average Web Purchases |
|---|---|
| PhD | 4.4 |
| Graduation | 4.1 |
| Master | 4.0 |
| 2n Cycle | 3.8 |
| Basic | 1.9 |

The pattern is consistent across all three dimensions. Higher income, older age and higher education all connect with more web purchases. The strongest online buyers are medium income, senior, PhD-level customers.

---

## What the Data Got Interesting

**The Kids Paradox: Engagement Does Not Equal Conversion**

72% of Maven Marketing customers have children at home. They showed 63.49% campaign engagement. They are the majority. They look like the primary target audience.

But customers without children averaged 4.4 web purchases compared to 4.0 for customers with children.

The group that engages more with campaigns buys online less frequently. This is the most important finding for Maven Marketing's marketing strategy.

Engagement and conversion are not the same thing.

**Seniors Outbuy Young Adults Online**

Senior customers aged 46 to 60 record the highest average web purchases at 4.6 per customer. Young adults aged 18 to 30 average just 3.3 web purchases. That is a 1.3 purchase gap per customer in favour of the older segment.

The assumption that younger customers dominate online shopping is contradicted by this data. Maven Marketing's most established, higher income older customers are quietly driving online revenue.

**Campaign 2 Was Nearly Invisible**

30 acceptances. 2,237 customers. 1.3% response rate.

The final campaign achieved 334 acceptances with exactly the same customer base. The gap between the best and worst campaign is 11 times. Whatever drove Campaign 2's messaging, targeting or offer failed significantly and deserves investigation before the next campaign cycle.

**Wine Is a Single Point of Failure**

Wine generating $680,000 and 50% of total revenue is impressive. It is also a concentration risk. A supply disruption, price increase or shift in customer wine preference could cut total revenue in half.

---

## Summary and Conclusion

This analysis set out to understand who Maven Marketing's customers are, what they respond to, what they buy and where they buy it. The data answered every question and challenged several assumptions along the way.

The customer base is clear. 2,237 customers, average age 45, average income $52,000, 72% with children, predominantly married graduates from Spain. Medium income customers form the largest segment at 1,165 customers. Married customers account for 864. Spain contributes 1,094 customers, nearly half the total.

Campaign performance is uneven. The final campaign generated 334 acceptances at a 45% overall response rate while Campaign 2 managed just 30 acceptances from the same audience. Education is the strongest predictor of campaign response. PhD holders respond at 54%. Basic education customers respond at 15%. The 39 percentage point gap between them tells the business exactly where to focus campaign spend.

Product revenue is heavily concentrated. Wine at $680,000 and Meat at $373,000 account for nearly 80% of total revenue combined. The remaining four products (Gold at $98,000, Fish at $84,000, Sweets at $61,000 and Fruits at $59,000) collectively contribute just 22% of revenue.

Channel performance follows a similar pattern. The store leads with 13,000 purchases (39% of total). Web follows with 9,100 purchases (27%). Catalog records 6,000 (18%) and Deals trails at 5,200 purchases (16%). Discount-based promotions are the weakest channel despite representing an ongoing business investment.

Web purchasing behaviour is driven by income, education and age rather than lifestyle factors. Medium income customers average 5.4 web purchases. Seniors average 4.6. PhD holders average 4.4. The customers who engage most with campaigns (parents at 63.49% engagement) are not the customers who buy online most. Customers without children average 4.4 web purchases versus 4.0 for customers with children.

This connects directly back to the opening finding. The assumption that busy parents would be the biggest online shoppers was wrong. The customers most likely to convert online are medium to high income, educated, slightly older customers without children at home. That is who Maven Marketing should be building its digital experience for.

---

## Recommendations

**1. Protect Wine revenue but start diversifying now.**
Wine generating 50% of revenue from one product category is a risk. Investing in growing Meat and Gold product lines will reduce dependency on a single category.

**2. Redirect Deals channel budget to the Web experience.**
The Deals channel records just 5,200 purchases (16% of total). Meanwhile 98% of customers have made at least one web purchase and the Web channel records 9,100 purchases. Discount promotions are not driving behaviour. Digital experience investment will deliver stronger returns.

**3. Concentrate campaign spend on educated and higher income segments.**
PhD holders respond at 54% versus 15% for Basic education customers. Campaign budgets that treat all customers equally are inefficient. Targeting graduation-level and above customers will deliver stronger response rates based on the data.

**4. Separate campaign engagement metrics from conversion metrics.**
Customers with children engage with campaigns at 63.49% but average fewer web purchases than customers without children. Reporting engagement as a proxy for conversion is misleading. Future campaign analysis should track both engagement and actual purchase conversion separately.

**5. Investigate Campaign 2 before the next campaign cycle.**
30 acceptances from 2,237 customers represents a near-total failure of messaging, targeting or offer design. Understanding what differentiated Campaign 2 from the final campaign (334 acceptances, 11x better performance) is essential before committing budget to the next campaign round.

**6. Build the digital experience for medium income seniors and educated customers.**
The strongest online buyers are medium income (5.4 avg purchases), seniors aged 46 to 60 (4.6 avg purchases) and PhD-educated customers (4.4 avg purchases). UX, product recommendations and digital marketing should be optimized for this segment.

---

## Live Dashboard

[Explore the Interactive Dashboard here](https://app.powerbi.com/links/XuaBYmn5kj?ctid=f6f117ef-72a8-4267-9390-7c30e90fd172&pbi_source=linkShare)

---

## Author and Contact

**Anetoh Olivia Chinecherem**
Data Analyst | Geology Graduate | Digitaley Drive Data Analytics Bootcamp

[LinkedIn](https://www.linkedin.com/in/olivia-anetoh-955b94328) | [GitHub](https://github.com/Olivia-Micheal) | [Email](mailto:anetohchinecherem@gmail.com)

---

*This project was independently completed as part of the Digitaley Drive Data Analytics Bootcamp. All analysis, data modeling, DAX measures and dashboard design were done using Microsoft Power BI.*
