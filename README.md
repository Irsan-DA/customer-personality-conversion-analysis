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

1. **Data Cleaning** — handled 24 missing values in `Income` (imputed with median), removed 8 rows with outliers that were logically inconsistent (unrealistic `Year_Birth`, and `Income` values that didn't match the customer's spending pattern)
2. **Feature Engineering**
   - `Age` = year of `Dt_Customer` − `Year_Birth` (calculated relative to each customer's registration date, not the current year, since the data was collected between 2012–2014)
   - `Age_Group` = binned into Youth (16–25), Young Adult (26–35), Adult (36–45), Middle Age (46–55), Senior (56+)
   - `Total_Children` = `Kidhome` + `Teenhome`
   - `Total_Spending` = sum of all `Mnt*` columns
   - `Total_Transactions` = sum of all `Num*Purchases` columns
   - `Conversion_Rate` = `Response` / `NumWebVisitsMonth` (with zero-visit handling)
3. **Exploratory Data Analysis** — univariate & bivariate analysis across income, spending, age group, and conversion rate, supported by a correlation heatmap
4. **Statistical Significance Testing** — Shapiro-Wilk normality test, followed by Kruskal-Wallis (for categorical groupings) and Spearman correlation (for continuous variables), used instead of ANOVA/Pearson since `Conversion_Rate` is not normally distributed
5. **Insight & Recommendation** — translate patterns into marketing actions

## Business Understanding

This project engineers several features to support the conversion rate analysis, based on the project's core objective and established customer behavior frameworks:

`Age & Age Group` -> used to segment customers and test whether age has a significant relationship with conversion rate.

`Conversion Rate (Response / NumWebVisitsMonth)` -> measures how likely a customer is to respond to a campaign relative to how often they visit the website, turning raw visit and response counts into a single behavioral metric. Note: this is a custom, per-customer definition rather than the standard aggregate definition of conversion rate used in marketing analytics (total responders ÷ total visitors) — see Limitations below.

`Total_Spending and Total_Transactions` -> derived from the RFM model **(Recency, Frequency, Monetary)**, a well-established framework in customer behavior analytics. Total_Transactions represents purchase Frequency, and Total_Spending represents Monetary value. The core premise of RFM is that customers who purchase more frequently and spend more are more likely to respond positively to new offers, and RFM-derived scores are commonly used as features in predictive models such as churn and response modeling.

`Total_Children (Kidhome + Teenhome)` -> based on household size/composition as a demographic segmentation variable. Literature shows household size does influence spending patterns, but the direction of this effect is inconsistent across studies (some show larger households spend more in certain categories, others show the opposite). This feature is included as an exploratory variable rather than one with an assumed direction of effect. It ended up being the strongest predictor of conversion in this analysis (see Key Findings).

### References

- Enhancing customer repurchase prediction: Integrating classification algorithms with RFM analysis -> https://www.sciencedirect.com/science/article/pii/S0970389625000266
- RFM Modeling in Marketing: The Complete Guide -> https://datacx.ai/p/rfm-modeling/
- RF-LightGBM: A probabilistic ensemble way to predict customer repurchase behaviour -> https://arxiv.org/pdf/2109.00724
- The Impact of Household Size on Consumer Behavior -> https://fastercapital.com/topics/the-impact-of-household-size-on-consumer-behavior.html
- CMU Capstone: Consumer Behaviors (household spending with/without children) -> https://www.stat.cmu.edu/capstoneresearch/fall2022/315files_f22/team8.html
- Changes in consumer spending behavior during the COVID-19 pandemic across product categories -> https://pmc.ncbi.nlm.nih.gov/articles/PMC9660125/
- Parents and children in supermarkets: Incidence and influence -> https://www.sciencedirect.com/science/article/abs/pii/S0969698917301145

## Tools

Python, Pandas, Matplotlib/Seaborn, SciPy, Google Colab

## Repository Structure

```
customer-personality-conversion-analysis/
├── data/ # raw dataset
├── notebooks/ # analysis notebook (Google Colab / Jupyter)
├── images/ # exported plots used in the report
├── reports/ # written EDA summary & insights
└── README.md
```

## Key Findings

**Conversion Rate distribution.** The metric is heavily right-skewed and zero-inflated: ~85% of customers (1,898 of 2,232) have a Conversion Rate of 0.0, meaning they did not respond to the last campaign regardless of visit frequency. A small group converted with varying efficiency (0.1–0.5), and 30 customers converted on their very first visit (Conversion Rate = 1.0).

**Age is not a statistically significant factor.** While a bar chart initially suggested Youth (16–25) had the highest average conversion rate (0.062) compared to Middle Age (0.035), a Kruskal-Wallis test (the appropriate method given the non-normal distribution of Conversion Rate, confirmed via Shapiro-Wilk) found this difference **not significant** (p = 0.317). This is also consistent with the Youth group's small sample size (116 customers vs. 420–727 in other groups). An ANOVA test on the same data returned p = 0.047, but this result is unreliable since ANOVA assumes normally distributed data — an assumption violated here.

**Household composition (Total_Children) is the strongest predictor found.** A Kruskal-Wallis test found a highly significant relationship between number of children and Conversion Rate (p ≈ 0.000000). Customers with no children convert at a notably higher rate (0.115) compared to customers with children (0.005–0.016) — a 7 to 23x difference.

**Total_Spending has a weak but significant positive relationship.** A Spearman correlation test found a significant relationship (p ≈ 0.000000) but the correlation itself is weak (rho = 0.26), meaning spending alone explains only a small part of the variation in conversion behavior.

| Variable | Test | p-value | Result | Strength |
|---|---|---|---|---|
| Age Group | Kruskal-Wallis | 0.317 | Not significant | — |
| Total_Children | Kruskal-Wallis | ≈0.000000 | Significant | Very strong (7–23x gap) |
| Total_Spending | Spearman | ≈0.000000 | Significant | Weak (rho = 0.26) |

## Recommendations

- **Prioritize customers without children** when targeting future campaigns — this segment shows by far the highest conversion likelihood in this dataset.
- **Avoid segmenting campaigns primarily by age** — it was not found to be a statistically reliable differentiator here, despite initial visual appearance.
- **Use spending level as a secondary signal**, not a primary targeting criterion, given its weak (though significant) relationship with conversion.
- **Investigate further why customers with children convert less often** (time constraints, differing purchase priorities) to design more relevant campaigns for that segment going forward.

## Limitations

- `Conversion_Rate` is a custom, per-customer metric (`Response / NumWebVisitsMonth`) defined by the project brief, and differs from the standard aggregate definition of conversion rate used in marketing analytics. It is best read as an "efficiency" measure — how few visits a customer needed before responding — rather than a general conversion likelihood.
- The metric only captures web-channel activity, so it cannot account for other factors that may influence a customer's decision to respond (e.g. catalog or in-store interactions, word-of-mouth, social media exposure).
- Some subgroups have small sample sizes (Youth: 116 customers; Total_Children = 3: 53 customers), so findings for these groups should be interpreted cautiously.
- All relationships reported here are correlational, not causal.

## Source Code

- Notebook: [link to Colab / notebook file](https://colab.research.google.com/drive/1B0B51O3NqYDAmjiTz-qDqm-YYf_8rSYU?usp=sharing)

---
**Author:** Irsan Maulana Yusuf ([Personal Portfolio](https://github.com/Irsan-DA))
