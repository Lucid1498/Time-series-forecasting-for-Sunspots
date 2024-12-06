# Time-series-forecasting-for-Sunspots
This project implements time series forecasting using the Facebook Prophet module to predict sunspot activity. Sunspots, temporary phenomena on the Sun's photosphere, exhibit an 11-year solar cycle. The analysis leverages daily, monthly, and yearly sunspot datasets to explore their behavior and forecast future activity.
Table of Contents

# Table of Content
Datasets
Features
Modeling and Forecasting
Results
How to Run
Folder Structure
License

# Introduction
Sunspots are areas of intense magnetic activity that appear darker on the Sun's surface. Their prediction is crucial for understanding solar activity and its impacts on Earth. This project aims to build a robust and scalable forecasting model.

# Datasets
The datasets used in this project are available from the World Data Center SILSO.

Daily Total Sunspot Numbers: SN_d_tot_V2.0.csv
Monthly Mean Total Sunspot Numbers: SN_m_tot_V2.0.csv
Yearly Mean Total Sunspot Numbers: SN_y_tot_V2.0.csv

# Data Description
Each dataset includes columns for date, sunspot counts, and quality indicators.
Missing values (-1) have been handled during preprocessing.

# Features
Forecasts for daily, monthly, and yearly data.
Flexible codebase that auto-detects time units (days, months, years).
Tuning of:
    Growth models (linear, logistic, flat).
    Seasonality parameters (add_seasonality method).
    Trend changepoints (n_changepoints, changepoint_prior_scale).

# Modeling and Forecasting
The model is trained using Facebook Prophet, which fits non-linear trends with seasonality components. Predictions are made for:
Daily: 100, 200, and 365 days.
Monthly: 1, 6, and 9 months.
Yearly: 1, 10, and 20 years.

# Evaluation Metrics
Models are evaluated using:
Mean Absolute Error (MAE)
Mean Absolute Percentage Error (MAPE)
R² score (via sklearn metrics).

# Results
Visualizations and tabular results highlight historical trends and future forecasts. Key findings are summarized in the notebook.
