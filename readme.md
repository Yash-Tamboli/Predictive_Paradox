Predictive Paradox: National Grid Power Demand Forecaster

This repository features an end to end Machine Learning pipeline designed for high precision national grid demand forecasting using a gradient boosted ensemble XGBoost.
This model integrates historical PGCB load data, hourly weather metrics, and World Bank economic indicators to predict a robust solution for energy management.

-Data Sources (can be seen in data folder)
weather_data.xlsx
PGCB_date_power_demand.xlsx
economic_full_1.csv

-Performance Metric
MAPE: 2.46% (Mean Absolute Percentage Error)
Model Architecture: XGBoost (Extreme Gradient Boosting)
Validation Strategy: Strict Chronological Split (Training: 2015-2022 | Testing: 2024)
 
-How I handeled the missing data and anomalies ?
For missing data I used Simple Linear Interpolation if the data was within 6 hrs or else I marked it as NaN , this was done for demand and weather data . For Economic indicators I used Forward Fill to fill all the missing values .
To handle anomalies I used a Simple Rolling Median method which identified the spkies and marked them as NaN.

-Feature Engineering
The temporal features are engineered using sine and cosine functions.
demand_lag_1h , temp_lag_24h and demand_lag_24h was created beacuse demand highly depends upon last hour consumption and daily patterns.
demand_change_1h the change feature tells the model where we are going.
is_weekend added to help the model distinguish between high demand on industrial workdays and the significantly lower energy consumption patterns on weekend.
rolling_mean_6h was made to smooth the noise and rolling_std_6h was created to tell model if sometime the weather is unstable.

-Key insights from feature importance
demand_lag_1h : The best predictor of what happens at 3:00 PM is what happened at 2:00 PM in this case .
demand_change_1h : This is also utilized by model to see where the pattern is going.
Apparent Temperature : The model uses this to adjust the standard load curve up or down based on the daily heat index.
Temporal features like hour_sin, hour_cos, and is_weekend show high importance in defining the shape of the day.

Project Workflow
The pipeline follows a five-stage process.

1.Structural Alignment (The Hourly Spine)
Made a continuous range of every hour in the dataset .

2.Signal Purification
-Used a rolling median and Median Absolute Deviation (MAD) to identify non-physical demand spikes. Detected outliers were marked as NaN .
-Linear Interpolation: Small Gaps upto 6 hrs were filled , while leaving large gaps empty to preserve data integrity.

3.Data Fusion
Hourly demand and weather data was merged with the yearly economic one .

4.Advanced Feature Engineering
Temporal Cyclicality: Encoded hours and months into Sine/Cosine waves to capture the repeating 24 hour and 12 month human cycle.
Historical Memory: Engineered 1h and 24h Lags + 6h Rolling Averages to capture grid momentum and daily load shapes.
Few more features like demand_change_1h and is_weekend were made .

5.Training and Testing .
Employed a strict Chronological Split (Training: 2015–2023 | Testing: 2024). This prevents Data Leakage and ensures the model is tested on truly unseen future data.
Specifically tuned for reg:absoluteerror to minimize MAPE (Mean Absolute Percentage Error).


#Some more plots

-Demand Dependency: Current Hour vs. Previous Hour
This straight line tells thst depend at t highly depends on demand at t-1 (y=x line tells demand at t is almost equal to demand at t-1)

-Partial Dependence: Marginal Effect of Features on Demand
1.Apparent temp curve tells that the demand is flat till 22 Celcius but as temp increases further the demand also increases.
2.As Urban population increases the demand increases.

-1 week window to show actual demand and my model prediction.
Orange line follows the blue line closely representing very low MAPE .
On 2024-02-16. The actual demand (blue) took a massive, sharp plunge down to nearly 7,000 MW, while the prediction (orange) dropped but stayed slightly higher this suggests an Anomaly. Because 2024-02-16, was a Friday, my is_weekend feature helped the model predict a drop, but the actual drop was even more severe than the model expected.

Author
Name : Yash Tamboli
Roll no : 250121066

