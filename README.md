# Customer Personality & Conversion Rate Analysis

Analyzing customer income, spending, and demographic patterns to understand what drives marketing campaign conversion and identifying which customer segments are most likely to respond to future campaigns.

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

## Business Understanding

This project engineers several features to support the conversion rate analysis, based on the project's core objective and established customer behavior frameworks:

`Age & Age Group` -> used to segment customers and test whether age has a significant relationship with conversion rate.

`Conversion Rate (Response / NumWebVisitsMonth)` -> measures how likely a customer is to respond to a campaign relative to how often they visit the website, turning raw visit and response counts into a single behavioral metric.

`Total_Spending and Total_Transactions` -> derived from the RFM model **(Recency, Frequency, Monetary)**, a well-established framework in customer behavior analytics. Total_Transactions represents purchase Frequency, and Total_Spending represents Monetary value. The core premise of RFM is that customers who purchase more frequently and spend more are more likely to respond positively to new offers, and RFM-derived scores are commonly used as features in predictive models such as churn and response modeling.

`Total_Children (Kidhome + Teenhome)` -> based on household size/composition as a demographic segmentation variable. Literature shows household size does influence spending patterns, but the direction of this effect is inconsistent across studies (some show larger households spend more in certain categories, others show the opposite). This feature is included as an exploratory variable rather than one with an assumed direction of effect.

### References

- Enhancing customer repurchase prediction: Integrating classification algorithms with RFM analysis — https://www.sciencedirect.com/science/article/pii/S0970389625000266
- RFM Modeling in Marketing: The Complete Guide — https://datacx.ai/p/rfm-modeling/
- RF-LightGBM: A probabilistic ensemble way to predict customer repurchase behaviour — https://arxiv.org/pdf/2109.00724
- The Impact of Household Size on Consumer Behavior — https://fastercapital.com/topics/the-impact-of-household-size-on-consumer-behavior.html
- CMU Capstone: Consumer Behaviors (household spending with/without children) — https://www.stat.cmu.edu/capstoneresearch/fall2022/315files_f22/team8.html
- Changes in consumer spending behavior during the COVID-19 pandemic across product categories — https://pmc.ncbi.nlm.nih.gov/articles/PMC9660125/
- Parents and children in supermarkets: Incidence and influence — https://www.sciencedirect.com/science/article/abs/pii/S0969698917301145


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
