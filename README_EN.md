# Credit Card Customer Segmentation using K-Means

## About the Project

This project applies **unsupervised Machine Learning**, using the **K-Means** algorithm, to identify and interpret different customer profiles based on their credit behavior and relationship with a financial institution.

The objective was not to predict a predefined target variable, but to **discover groups of customers with similar behaviors** and transform these groups into meaningful business segments.

## Dataset

The project uses the [Credit Card Customer Data](https://www.kaggle.com/datasets/aryashah2k/credit-card-customer-data) dataset available on Kaggle.

The original dataset contains **660 records and 7 variables**:

- `Sl_No`
- `Customer Key`
- `Avg_Credit_Limit`
- `Total_Credit_Cards`
- `Total_visits_bank`
- `Total_visits_online`
- `Total_calls_made`

Five behavioral features were selected for clustering:

- `Avg_Credit_Limit`
- `Total_Credit_Cards`
- `Total_visits_bank`
- `Total_visits_online`
- `Total_calls_made`

`Sl_No` and `Customer Key` were kept only as identifiers and were not used as inputs for the clustering model.

## Methodology

The project followed a structured Machine Learning workflow:

**Problem Understanding → Data Collection → Exploratory Data Analysis → Data Preparation → Standardization → Cluster Selection → K-Means → Cluster Interpretation → Model Operationalization**

The initial analysis showed that there were no missing values and no exact duplicate records.

Five repeated `Customer Key` values were identified, representing 10 records. Since the records associated with the same keys presented different characteristics, they were not considered exact duplicates and were therefore retained in the analysis.

Potential outliers were also identified in `Avg_Credit_Limit` and `Total_visits_online`. These observations were kept because they may represent legitimate and meaningful customer behaviors.

## Exploratory Data Analysis

The exploratory analysis revealed important relationships between the variables:

- `Avg_Credit_Limit` × `Total_Credit_Cards`: correlation of **0.61**
- `Avg_Credit_Limit` × `Total_visits_online`: correlation of **0.55**
- `Total_Credit_Cards` × `Total_calls_made`: correlation of **-0.65**
- `Total_visits_bank` × `Total_calls_made`: correlation of **-0.51**

These relationships indicated that customers have different patterns of credit usage and channel preferences.

## Standardization

The five features have significantly different scales. For example, `Avg_Credit_Limit` ranges from **3,000 to 200,000**, while the other variables have much smaller ranges.

Since K-Means relies on distance calculations to determine customer similarity, **StandardScaler** was applied to place all features on a comparable scale.

After standardization, the variables had approximately:

- Mean = **0**
- Standard deviation = **1**

This prevents features with larger numerical values from disproportionately influencing the clustering process.

## Selecting the Number of Clusters

Different values of **K from 2 to 10** were evaluated using:

- **Elbow Method**
- **Silhouette Score**

The best result was obtained with:

**K = 3**

**Silhouette Score = 0.5157**

The three-cluster configuration achieved the highest Silhouette Score and provided a strong balance between cluster cohesion and separation.

## Segmentation Results

The model identified three customer segments:

| Cluster | Customers | % of Base | Profile |
|---|---:|---:|---|
| 0 | 386 | 58.48% | Relationship-focused on Branches |
| 1 | 50 | 7.58% | Digital Relationship |
| 2 | 224 | 33.94% | Assisted Relationship |

### Branch Relationship

This is the largest segment, representing **58.48% of the dataset**.

It is characterized by greater use of physical bank branches, lower online activity, and an above-average number of credit cards.

**Potential opportunities:** encourage digital channel adoption, identify transactions that can be moved to self-service channels, and use branch interactions to strengthen customer relationships.

### Digital Relationship

This segment represents **7.58% of the dataset**.

It has the highest average credit limit, the highest number of credit cards, and very strong online usage, combined with low branch and phone usage.

**Potential opportunities:** personalized offers, cross-selling, up-selling, retention strategies, and predominantly digital customer engagement.

### Assisted Relationship

This segment represents **33.94% of the dataset**.

It has a lower average credit limit and fewer credit cards, but significantly higher usage of phone-based service.

**Potential opportunities:** investigate the reasons for phone interactions, identify potential difficulties with digital channels, encourage self-service, and evaluate opportunities to increase customer engagement.

The segment names represent **behavioral patterns identified in the data** and should not be interpreted as a ranking of customer quality or value.

## PCA and Cluster Visualization

**PCA (Principal Component Analysis)** was used to visualize the three clusters in two dimensions.

The first two principal components explained:

- **PC1:** 45.74%
- **PC2:** 37.43%
- **Cumulative variance explained:** 83.16%

PCA was used exclusively for visualization. The K-Means model continued to use the five original standardized features.

## Model Operationalization

After validation, the model was converted into a **reusable Machine Learning Pipeline** combining:

**StandardScaler → K-Means**

This ensures that new customers can be processed using the same transformation and clustering logic applied during model development.

Two main artifacts were generated:

- `segmentacao_clientes_kmeans.pkl`: trained model for assigning new customers to segments.
- `clientes_segmentados.csv`: dataset containing customers, cluster assignments, and segment profiles.

The final project therefore provides two complementary outcomes:

**1. Insight:** an understanding of the different customer segments within the portfolio.

**2. Model:** the ability to apply the segmentation to new customers.

## Key Insights

The analysis showed that customers differ not only in their level of credit exposure, but also in **how they interact with the financial institution**.

The Digital Relationship segment concentrates customers with higher credit limits, more cards, and strong online engagement. The Branch Relationship segment represents the majority of the portfolio and has a stronger preference for physical banking. The Assisted Relationship segment shows lower financial engagement combined with higher phone-based interaction.

These patterns can serve as a starting point for differentiated strategies involving customer service, communication, relationship management, and personalized offers.

## Limitations

The segmentation was built using only five behavioral features and does not include information such as income, profitability, delinquency, account balances, transaction history, products held, or relationship tenure.

Therefore, the clusters primarily represent **behavioral and channel-relationship patterns**, rather than a complete assessment of customer economic value.

A business application could incorporate financial, transactional, and profitability data to make the segmentation more precise and actionable.

## Conclusion

The project demonstrates how **K-Means can be used to discover customer segments without a predefined target variable**.

Based on the available behavioral characteristics, three distinct customer groups were identified and transformed into interpretable business profiles.

Beyond analyzing the current portfolio, the project was converted into a reusable model capable of assigning new customers to the identified segments.

The final solution combines **exploratory analysis, Machine Learning, business interpretation, and model operationalization**, providing a foundation for future applications in CRM, personalization, customer service, and data-driven decision-making.

## Future Improvements

Potential next steps include:

- Adding financial and transactional data;
- Incorporating profitability and delinquency indicators;
- Including products held by each customer;
- Adding customer relationship history;
- Monitoring cluster stability over time;
- Building a dashboard to monitor customer segments;
- Deploying an API or application for automatic segmentation of new customers.
