# Hybrid Sales Forecasting Notebook

This repository contains a Jupyter notebook that demonstrates a **hybrid approach to sales forecasting** using Python.

The notebook combines:

- **Regression modeling** to capture the impact of business drivers (marketing spend, price, macro indicators, etc.)
- **ARIMA time-series modeling** to capture residual temporal structure
- **Explainability techniques** to understand how each feature contributes to forecasts

The result is an interpretable forecasting workflow suitable for business analytics, demand planning, and financial forecasting.

---

# Notebook

`hybrid_sales_forecasting_notebook_executed.ipynb`

The notebook walks through the entire forecasting pipeline including:

1. Data loading
2. Feature engineering
3. Regression modeling
4. Residual time-series modeling
5. Hybrid forecast generation
6. Model explainability
7. Future forecasting

---

# Forecasting Methodology

The notebook implements a **two-stage hybrid model**.

## 1. Multivariate Regression

Sales are modeled using external business drivers such as:

- marketing spend
- product price
- economic indicators
- holidays
- competitor activity
- trend and seasonality features

This produces an interpretable model showing how each variable impacts sales.

## 2. Residual Time-Series Modeling

After regression predictions are produced:

```
residual = actual_sales - regression_prediction
```

An **ARIMA model** is then trained on the residuals to capture time-based patterns not explained by the regression.

## 3. Hybrid Forecast

Final forecast:

```
final_forecast = regression_prediction + residual_forecast
```

This combines business-driver modeling with time-series correction.

---

# Key Capabilities

The notebook demonstrates:

- interpretable regression modeling
- hybrid regression + ARIMA forecasting
- time-series residual modeling
- feature importance analysis
- forecast explainability
- generation of forecast confidence intervals

---

# Libraries Used

The notebook uses common Python data science libraries:

- `from pathlib import Path`
- `from sklearn.inspection import permutation_importance`
- `from sklearn.linear_model import LinearRegression`
- `from sklearn.metrics import mean_absolute_error, mean_squared_error`
- `from sklearn.pipeline import Pipeline`
- `from sklearn.preprocessing import StandardScaler`
- `from statsmodels.tsa.arima.model import ARIMA`
- `import json`
- `import matplotlib.pyplot as plt`
- `import numpy as np`
- `import os`
- `import pandas as pd`
- `import statsmodels.api as sm`

Typical core dependencies include:

- pandas
- numpy
- statsmodels
- scikit-learn

---

# Example Dataset

The notebook expects a **monthly sales dataset** with fields such as:

- date
- sales
- marketing_spend
- avg_price
- economic indicators
- holiday flags
- competitor signals

The dataset should be structured as a **time series with monthly observations**.

---

# Outputs

The notebook generates several useful outputs:

- forecasted sales values
- model coefficients
- feature importance metrics
- forecast confidence intervals
- driver contribution breakdowns

These outputs help both with **forecast accuracy** and **business interpretability**.

---

# How to Run

## 1. Install dependencies

```bash
pip install pandas numpy statsmodels scikit-learn matplotlib
```

## 2. Start Jupyter

```bash
jupyter notebook
```

## 3. Open the notebook

```
hybrid_sales_forecasting_notebook_executed.ipynb
```

Run the cells sequentially.

---

# Use Cases

This forecasting approach is useful for:

- sales forecasting
- demand planning
- marketing impact analysis
- pricing analysis
- scenario modeling
- business driver analysis

---

# Possible Enhancements

Future improvements could include:

- SARIMAX modeling
- automated ARIMA tuning
- rolling backtests
- cross‑validation
- gradient boosting models (XGBoost / LightGBM)
- scenario simulation for future marketing plans

---

# License

This project is intended for educational and analytical use.

