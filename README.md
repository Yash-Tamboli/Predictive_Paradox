Predictive Paradox: National Grid Power Demand Forecaster

This repository features an end to end Machine Learning pipeline designed for high precision national grid demand forecasting using a gradient boosted ensemble XGBoost.
This model integrates historical PGCB load data, hourly weather metrics, and World Bank economic indicators to predict a robust solution for energy management.

Project Structure
├── Data/
│   ├── PGCB_date_power_demand.xlsx  # Raw Grid Data
│   ├── weather_data.xlsx            # Hourly Weather
│   └── economic_full_1.csv          # World Bank Indicators
├── solution.ipynb               # End-to-end ML pipeline
├── requirements.txt             # Project dependencies 
└── readme.md                    # Project documentation

-Performance Metric
MAPE: 2.46% (Mean Absolute Percentage Error)
Model Architecture: XGBoost (Extreme Gradient Boosting)
Validation Strategy: Strict Chronological Split (Training: 2015-2022 | Testing: 2024)

Author
Name : Yash Tamboli
Roll no : 250121066
