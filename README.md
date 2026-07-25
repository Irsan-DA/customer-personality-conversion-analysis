# Customer Personality & Conversion Rate Analysis

Analyzing customer income, spending, and demographic patterns to understand what drives marketing campaign conversion — and identifying which customer segments are most likely to respond to future campaigns.

## Background

A retail/FMCG company runs periodic marketing campaigns but doesn't have a clear picture of *who* actually converts. This project analyzes customer transaction and demographic data to engineer a conversion rate metric, segment customers by age and behavior, and surface actionable patterns the marketing team can use to target future campaigns more efficiently.

## Objective

- Engineer a **Conversion Rate** metric (`Response / NumWebVisitsMonth`) from raw behavioral data
- Build supporting features: age, age group, total children, total spending, total transactions
- Explore the relationship between conversion rate and customer attributes (age, income, spending)
- Identify which customer segments are most likely to respond to marketing campaigns
- Translate findings into marketing recommendations

## Dataset

Customer Personality Analysis dataset — 2,240 customers, 29 attributes covering:

| Category | Columns |
|---|---|
| Demographics | `Year_Birth`, `Education`, `Marital_Status`, `Income`, `Kidhome`, `Teenhome` |
| Spending (last 2 years) | `MntCoke`, `MntFruits`, `MntMeatProducts`, `MntFishProducts`, `MntSweetProducts`, `MntGoldProds` |
| Purchase behavior | `NumDealsPurchases`, `NumWebPurchases`, `NumCatalogPurchases`, `NumStorePurchases`, `NumWebVisitsMonth` |
| Campaign response | `AcceptedCmp1`–`AcceptedCmp5`, `Response` |
| Other | `Dt_Customer`, `Recency`, `Complain` |

## Methodology

1. **Data Cleaning** — handle missing values, check duplicates, fix data types
2. **Feature Engineering**
   - `Age` = current year − `Year_Birth`
   - `Age_Group` = binned age categories
   - `Total_Children` = `Kidhome` + `Teenhome`
   - `Total_Spending` = sum of all `Mnt*` columns
   - `Total_Transactions` = sum of all `Num*Purchases` columns
   - `Conversion_Rate` = `Response` / `NumWebVisitsMonth` (with zero-visit handling)
3. **Exploratory Data Analysis** — univariate & bivariate analysis across income, spending, age group, and conversion rate
4. **Insight & Recommendation** — translate patterns into marketing actions

## Tools

Python, Pandas, Matplotlib/Seaborn, Google Colab

## Repository Structure

```
customer-personality-conversion-analysis/
├── data/                # raw dataset
├── notebooks/           # analysis notebook (Google Colab / Jupyter)
├── images/              # exported plots used in the report
├── reports/             # written EDA summary & insights
└── README.md
```

## Key Findings

_(to be filled in after analysis)_

## Source Code

- Notebook: [link to Colab / notebook file]
