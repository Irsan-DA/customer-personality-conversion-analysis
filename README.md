# Customer Personality & Conversion Rate Analysis

Predict Customer Personality to Boost Marketing Campaign using Machine Learning - analyzing customer income, spending, and demographic patterns to engineer a conversion rate metric, segment customers via clustering, and identify which segments are most likely to respond profitably to future campaigns.

## Background

A retail/FMCG company runs periodic marketing campaigns but lacks a clear picture of who actually converts. This project engineers a conversion rate metric from customer transaction and demographic data, explores which customer attributes relate to conversion behavior, builds a customer segmentation model via K-Means clustering, and translates the findings into a concrete retargeting recommendation with estimated financial impact.

## Objective

- Engineer a **Conversion Rate** metric (`Response / NumWebVisitsMonth`) from raw behavioral data
- Identify which customer attributes (age, income, spending, household composition) relate to conversion
- Prepare the dataset (cleaning, encoding, standardization) for machine learning
- Segment customers into clusters using K-Means
- Calculate the potential financial impact of retargeting each cluster and recommend where to focus

## Dataset

Customer Personality Analysis dataset - 2,240 customers, 29 attributes covering demographics, spending (last 2 years), purchase behavior, and campaign response history.

## Methodology

1. **Data Cleaning** - handled missing values (median imputation), removed rows with logically inconsistent outliers
2. **Feature Engineering** - Age, Age_Group, Total_Children, Total_Spending, Total_Transactions, Conversion_Rate
3. **EDA & Statistical Testing** - Shapiro-Wilk, Kruskal-Wallis, Spearman correlation, correlation heatmap
4. **Preprocessing for ML** - dropped irrelevant columns, feature encoding (Label/One-Hot), feature standardization (z-score)
5. **Data Modeling** - K-Means clustering with k selected via Elbow Method + Silhouette Score, PCA visualization
6. **Business Recommendation** - cluster profiling, retargeting selection, potential impact calculation

## Business Understanding

- **Age & Age Group** - segment customers to test whether age relates to conversion rate.
- **Conversion Rate (Response / NumWebVisitsMonth)** - a custom per-customer metric measuring response relative to visit frequency, better read as "response efficiency" rather than the standard aggregate conversion rate used in marketing.
- **Total_Spending & Total_Transactions** - proxies for Monetary and Frequency from the RFM framework.
- **Total_Children (Kidhome + Teenhome)** - household composition as a demographic segmentation variable; direction of effect is not assumed upfront. It turned out to be the strongest predictor of conversion in this analysis.

### References

- Enhancing customer repurchase prediction: Integrating classification algorithms with RFM analysis -> https://www.sciencedirect.com/science/article/pii/S0970389625000266
- RFM Modeling in Marketing: The Complete Guide -> https://datacx.ai/p/rfm-modeling/
- RF-LightGBM: A probabilistic ensemble way to predict customer repurchase behaviour -> https://arxiv.org/pdf/2109.00724
- The Impact of Household Size on Consumer Behavior -> https://fastercapital.com/topics/the-impact-of-household-size-on-consumer-behavior.html

## Tools

Python, Pandas, Matplotlib/Seaborn, SciPy, scikit-learn (KMeans, StandardScaler, PCA), Google Colab

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

### Conversion Rate Analysis

- **~85% of customers** (1,898/2,232) never responded to the last campaign - Conversion_Rate distribution is heavily zero-inflated.
- **Age is NOT a statistically significant factor** (Kruskal-Wallis p=0.317). An initial chart suggested Youth had the highest conversion rate, but this wasn't supported once sample size (Youth: 116 customers) and distribution shape were accounted for.
- **Total_Children is the strongest predictor found** (Kruskal-Wallis p≈0.000000). Customers with no children convert 7-23x more often (0.115) than those with children (0.005-0.016).
- **Total_Spending has a weak but significant relationship** (Spearman rho=0.26, p≈0.000000).

| Variable | Test | p-value | Result | Strength |
|---|---|---|---|---|
| Age Group | Kruskal-Wallis | 0.317 | Not significant | - |
| Total_Children | Kruskal-Wallis | ≈0.000000 | Significant | Very strong (7-23x gap) |
| Total_Spending | Spearman | ≈0.000000 | Significant | Weak (rho = 0.26) |

### Customer Segmentation (K-Means, k=3)

Optimal k determined via Elbow Method (bend around k=3) and validated with Silhouette Score (k=3: 0.2071 - modest but the best actionable option; all tested k values scored below 0.5, indicating a real but weak cluster structure).

| Cluster | Size | Income | Spending | Children | Conversion Rate |
|---|---|---|---|---|---|
| 0 - Budget-Conscious Browser | 46.3% (1,034) | 34.7M | 97.5K | 1.23 | 0.01 (lowest) |
| 1 - High-Value, Low-Commitment | 25.5% (570) | 76.0M | 1.39M | 0.20 | 0.13 (highest) |
| 2 - Mid-Tier Family Spender | 28.1% (628) | 57.8M | 731K | 1.17 | 0.02 |

### Retargeting Potential Impact

Using the dataset's own cost/revenue assumptions (cost per contact = 3, revenue per response = 11):

| Cluster | Pool Size | Response Rate | Net Profit |
|---|---|---|---|
| 0 | 942 | 8.90% | **-1,904 (loss)** |
| 1 | 403 | 29.30% | **+90 (profit)** |
| 2 | 553 | 11.94% | **-933 (loss)** |

Only Cluster 1 is financially viable to retarget under the current campaign model.

## Recommendations

- **Prioritize customers without children (Cluster 1)** for future campaign retargeting - the only segment with positive expected ROI.
- **Avoid segmenting campaigns primarily by age** - it was not found to be a statistically reliable differentiator.
- **Use spending level as a secondary signal**, not a primary targeting criterion.
- **Explore lower-cost engagement channels** (email, loyalty programs) for Cluster 0 and Cluster 2, since standard paid campaigns lose money for these segments.
- **Investigate further why customers with children convert less often** to design more relevant campaigns for that segment in the future.

## Limitations

- `Conversion_Rate` is a custom, per-customer metric defined by the project brief, and differs from the standard aggregate definition of conversion rate used in marketing analytics.
- The metric only captures web-channel activity - it cannot account for catalog, in-store, or word-of-mouth influence.
- Silhouette Scores across all tested k values are relatively low (max 0.29), indicating the underlying cluster structure is weak rather than sharply defined; the 2D PCA visualization only captures 48.38% of total variance.
- Some subgroups have small sample sizes (e.g. Youth: 116 customers; Total_Children=3: 53 customers) and should be interpreted cautiously.
- All relationships reported here are correlational, not causal.

---

## Source Code

Notebook: [link to Colab / notebook file](https://colab.research.google.com/drive/1B0B51O3NqYDAmjiTz-qDqm-YYf_8rSYU?usp=sharing)

---

**Author:** Irsan Maulana Yusuf ([Personal Portfolio](https://github.com/Irsan-DA))
