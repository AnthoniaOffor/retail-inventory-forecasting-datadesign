# retail-inventory-forecasting-datadesign

Anthonia — Data Engineer
Dataset: Retail Store Inventory Forecasting (Kaggle)
Snowflake Tool Explored: Snowflake Cortex AI/ML — Forecasting

## 📌 1. Dataset Structure
The dataset consists of synthetic but realistic retail store records, containing 73,100 daily observations across multiple stores, products, and regions.
Each row represents:
Store + Product + Date → daily sales & influencing factors


✅ Granularity

Daily time-series data
Per product, per store
Data spans multiple years

✅ Entity Structure (List Format)
Identifiers

Date
Store ID
Product ID

Product / Store Attributes

Category
Region

Sales & Inventory Metrics

Units Sold
Inventory Level
Units Ordered

Pricing & Promotion

Price
Discount
Holiday / Promo Flag

External Factors

Weather Condition
Competitor Pricing

Calendar / Seasonality

Seasonality
Extracted date features (Day of Week, Month, Quarter)


## 🎯 2. Target Variable
✅ Target Variable: Units Sold
Although the dataset includes a column labeled Demand Forecast, this represents the previous system's prediction, not actual demand.
The business goal is to forecast true customer demand, which is accurately measured by:

Units Sold = Actual daily demand per product

Forecasting Units Sold enables stores to:

✅ Prevent stockouts
✅ Avoid excess inventory
✅ Improve purchasing decisions
✅ Optimize supply chain planning


## 🔍 3. Key Features (Predictors)
Only features that meaningfully influence demand were selected.
✅ Product & Store Attributes

Category
Region

✅ Pricing & Promotions

Price
Discount
Holiday / Promo indicator

✅ External Factors

Weather Condition
Competitor Pricing

✅ Calendar Effects

Seasonality
Date-derived features (Month, Day of Week)

❌ Features intentionally excluded

























Product ID - Identifier, not predictive
Store ID - Identifier, no behavioral meaning
Inventory Level - Reflects stock constraints, not demand
Previous Demand Forecast - Legacy system output, not causal
These final features align best for forecasting requirements and industry best practices.

## ⚠️ 4. Data Quality & Bias Risks
✅ A. Stockout Bias
If inventory is zero, Units Sold becomes zero even though demand may be higher.
✅ B. Date Formatting Issues
Some rows display “########” (Excel overflow), requiring cleanup before ingestion.
✅ C. Missing or Noisy External Data
Weather and competitor pricing fields may contain inconsistencies.
✅ D. Pricing Outliers / Data Entry Errors
Extreme prices or incorrect discounts may distort predictions.
✅ E. Class Imbalance
Pivot table analysis shows well-balanced values across regions and categories.
However, bias may still occur from:

Stockouts
Incorrect pricing entries
Synthetic patterns not matching real-world behavior


## 🧠 5. How the Dataset Influenced Design Decisions

The dataset's daily granularity supports Snowflake Cortex’s time-series forecasting capabilities.
Balanced distribution across regions/categories allows reliable model training.
Presence of multiple explanatory variables (weather, price, promo) justifies a multivariate forecasting approach.
The existing Demand Forecast column guided the decision to ignore it as a target and treat it as legacy output.


## ⚖️ 6. Key Trade-offs & Risks
✅ Trade-Offs

Excluding inventory data avoids modeling supply constraints but prevents modeling unmet demand.
Using competitor pricing improves realism but may add noise.
Keeping both weather and seasonality increases model complexity but captures multi-scale patterns.

✅ Technical Risks

Incorrect preprocessing may introduce seasonality leakage or date misalignment.
The use of Demand Forcast in prediction will lead to data leakage
External feature sparsity (weather, competitor pricing) can reduce accuracy.

✅ Ethical Risks

Forecasting models impact resource allocation and staff planning; errors may disproportionately affect certain stores.
Synthetic data may give a misleading sense of accuracy when deployed in real settings.


## 💼 7. Real-World Business Use Case
Retail Inventory Optimization for Multi-Store Chains
A retail chain uses forecasting to predict how many units of each product customers will buy next week.
This forecast allows the business to:

Reduce stockouts
Lower holding costs
Plan staffing and deliveries
Improve customer satisfaction

This dataset enables exactly this type of predictive inventory management.

## 🪞 8. Reflection (Technical + Ethical)
✅ Technical Reflection
This project strengthened my understanding of multivariate time-series forecasting and feature selection. Choosing Units Sold as the target was critical because it represents real demand. Snowflake’s Cortex Forecasting tool simplifies model training but still requires careful data preparation, feature engineering, and time-series handling.
✅ Ethical Reflection
Forecasting systems shape operational decisions, and inaccurate models can harm customers or specific store locations. Even though the dataset is synthetic, in real-world deployments it is essential to ensure fairness, transparency, and continuous monitoring to avoid unintentional bias.
