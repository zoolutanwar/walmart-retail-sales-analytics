# Walmart Retail Analytics — Interview Guide

## 60–90 second explanation

I built an end-to-end retail analytics and forecasting project using Walmart's historical weekly store-sales data. I first validated and explored more than 421,000 store-department-week records and found strong yearly seasonality, especially around Thanksgiving and Christmas. During the analysis I noticed negative sales, so I investigated their frequency, sample size, store concentration, holiday relationship, and yearly trend. Department 47 showed a broad company-level pattern, while Departments 49 and 80 were unusually problematic in Store 35.

For forecasting, I aggregated the data to company-wide weekly sales and used a chronological split, with 131 weeks for training and the last 12 weeks for testing. I compared a seasonal baseline, Linear Regression, Decision Tree, and Random Forest using MAE, RMSE, and MAPE. Linear Regression performed best with 1.53% MAPE, so I retrained it on all available data and produced a 12-week forecast. Finally, I exported the results to a Walmart-branded Power BI dashboard for business users.

## Why use a chronological split?

Time-series data must retain its order. A random split could allow future information into the training set, creating data leakage and an unrealistically optimistic score.

## Why include a seasonal baseline?

The baseline predicts each week using the corresponding week from the previous year. Because Walmart sales show strong annual seasonality, this is a meaningful benchmark. A machine-learning model should outperform it to justify its added complexity.

## Why did Linear Regression win?

Linear Regression used calendar features and previous-year sales. It achieved the lowest MAPE at 1.53%, slightly better than Random Forest at 1.57%. It was also easier to interpret, faster to train, and less likely to overfit the relatively small weekly time series.

## What does 1.53% MAPE mean?

On an average test week, the Linear Regression forecast differed from actual company-wide sales by approximately 1.53%.

## Why was Decision Tree worse?

The weekly modelling dataset is small. A single tree can fit training patterns too specifically and generalise poorly. Restricting depth reduced overfitting, but it still underperformed the baseline and the other models.

## Main negative-sales finding

Department 47's negative-sales problem appeared across many stores. Store 35 was especially unusual in Departments 49 and 80: their negative rates were far higher than in other stores. This suggests reviewing Store 35's returns, refunds, stock adjustments, and data-capture process.

## Did holidays cause Department 47's negative sales?

No clear evidence supported that explanation. The negative rate was about 39.4% in normal weeks and 38.2% in holiday weeks.

## Business recommendations

1. Audit Store 35's processes for Departments 49 and 80.
2. Investigate Department 47 across the wider organisation.
3. Use the holiday forecast to plan staffing and inventory.
4. Refresh and monitor the model as new weekly data becomes available.

## Limitations

- The dataset does not identify why a transaction is negative.
- The evaluation uses one 12-week holdout rather than rolling validation.
- The model forecasts total company sales rather than individual store-department combinations.
- External drivers in `features.csv` were not yet incorporated into the forecasting model.

## Strong next improvements

- Add rolling time-series cross-validation.
- Engineer lag and rolling-average features.
- Incorporate temperature, CPI, fuel prices, unemployment, and markdown variables.
- Forecast at store-department level and aggregate predictions.
- Add model monitoring and scheduled dashboard refreshes.
