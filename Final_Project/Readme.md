Store Item Demand Forecasting
Project Overview: The goal of this project was to forecast 3 months of daily sales for 50 different items across 10 different stores, using 5 years of historical data.

Evaluation Metric: SMAPE (Symmetric Mean Absolute Percentage Error). A custom function was written to handle the competition's specific rule where SMAPE = 0 if both actual and predicted values are 0.

Approach & Process

1. Exploratory Data Analysis (EDA)

- Checked for missing values (there were none).
- Visualized overall trends, monthly seasonality, day-of-week patterns, and store-level differences.
- Key takeaway: There is a very clear upward trend year-over-year, strong summer seasonality, and weekend sales are consistently higher than weekdays.

2. Feature Engineering: Since tree models don't understand time natively, I had to extract features from the dates:

- Basic Time Features: Month, day, day of week, week of year, quarter, and boolean flags for weekends.
- Cyclical Encoding: Used Sin/Cos transformations for month and day of week so the model understands that December 31st and January 1st are close together.
- Lag Features: Shifted the target variable by 1, 7, 14, 28, and 365 days. The 365-day lag ended up being the most important feature.
- Rolling & Expanding Stats: Calculated 7-day and 30-day rolling means/standard deviations.
- Historical Day-of-Year Stats: Found the mean and standard deviation for each specific store-item-day combo from past years to give the model a baseline.

3. Modeling (LightGBM)

- Chose LightGBM over XGBoost/Deep Learning for its speed and high performance on tabular time-series data.
- Combined train and test data before creating lag features to prevent data leakage and ensure the test set got its 365-day lags properly.
- Validation Strategy: Time-series split. Trained on 2014-2016 data, validated on 2017 data. Achieved a Validation SMAPE of ~9.5%.
- Final Training: Retrained the model on the entire dataset (2014-2017) to maximize the data available before generating the final test predictions.

Repository Files

- Celebal_project.ipynb: The main Jupyter Notebook containing all EDA, feature engineering, modeling, and validation plots.
- submission.csv: The final output file containing the predicted sales for the test set.
- (Note: The train.csv and test.csv data files are not uploaded here due to size limits. They can be downloaded directly from the Kaggle competition page).

How to Run

1. Download the dataset from the Kaggle "Store Item Demand Forecasting Challenge" page.
2. Place train.csv and test.csv in the same folder as the Jupyter Notebook.
3. Open the notebook and run all cells.