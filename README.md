# Client Sales Forecast

A hybrid monthly sales forecasting notebook that combines:

1. **Multivariate regression** for external business drivers  
2. **ARIMA on regression residuals** to capture time-based structure not explained by the drivers  
3. **95% forecast confidence intervals**  
4. **Explainability outputs** for understanding driver impact on forecasts  

This project is designed as an interpretable forecasting workflow for monthly sales data.

## Notebook Overview

The notebook implements a hybrid forecasting pipeline using:

- **OLS regression** from `statsmodels` to model sales as a function of external variables
- **ARIMA** from `statsmodels` to model autocorrelation in the regression residuals
- **Permutation importance** from `scikit-learn` to provide model-agnostic feature importance
- **Driver contribution decomposition** to show how each feature contributes to each forecasted period

## Expected Input File

The notebook expects a CSV file named:

```text
synthetic_monthly_sales.csv
```

Expected columns:

```text
date
sales
marketing_spend
avg_price
economic_index
holiday_flag
competitor_promo_index
```

## Forecasting Approach

### Step 1: Feature engineering
The notebook adds deterministic time-based features:

- `t` for trend
- `month_num` for calendar month
- `month_sin` and `month_cos` for cyclical seasonality encoding

### Step 2: Regression model
An **OLS regression** is fit using:

- `marketing_spend`
- `avg_price`
- `economic_index`
- `holiday_flag`
- `competitor_promo_index`
- `t`
- `month_sin`
- `month_cos`

This provides an interpretable estimate of how external drivers influence sales.

### Step 3: Residual modeling with ARIMA
After regression predictions are generated, the notebook computes residuals:

```text
residual = actual sales - regression prediction
```

An **ARIMA(1, 0, 1)** model is then fit on the training residuals to capture remaining temporal structure.

### Step 4: Hybrid forecast
Final forecast:

```text
hybrid_forecast = regression_pred + residual_forecast
```

This combines business-driver effects with time-series residual correction.

### Step 5: Confidence intervals
The notebook produces lower and upper 95% forecast bounds from the ARIMA forecast interval, added back onto the regression component.

### Step 6: Explainability
The notebook produces:

- ranked regression coefficients
- permutation importance scores
- per-period driver contribution breakdowns

## Configuration

The notebook includes the following configuration values:

```python
DATA_PATH = "synthetic_monthly_sales.csv"
TRAIN_RATIO = 0.80
ARIMA_ORDER = (1, 0, 1)
FORECAST_HORIZON = 6
```

## Output Files

Running the notebook writes the following CSV outputs:

- `hybrid_forecast_output.csv`  
  Holdout-period forecast results

- `regression_coefficients_explainability.csv`  
  OLS coefficients ranked for interpretation

- `permutation_importance_explainability.csv`  
  Model-agnostic feature importance for the regression component

- `forecast_with_driver_contributions.csv`  
  Holdout forecasts with per-driver contribution columns

- `future_forecast_with_explainability.csv`  
  Forward forecast using the last known rows as stand-ins for future driver plans

## Python Libraries Used

- `numpy`
- `pandas`
- `statsmodels`
- `scikit-learn`

## Repository Structure

```text
.
├── Client Sales Forecast.ipynb
├── synthetic_monthly_sales.csv
├── README.md
```

## How to Run

### 1. Install dependencies

```bash
pip install numpy pandas statsmodels scikit-learn
```

### 2. Place the input CSV in the same working directory

Required file:

```text
synthetic_monthly_sales.csv
```

### 3. Run the notebook

Open the notebook in Jupyter and execute the cells in order.

## Notes and Assumptions

- The notebook uses a simple chronological split based on `TRAIN_RATIO`
- ARIMA order is fixed at `(1, 0, 1)` for demonstration
- The future forecast section reuses the last observed rows as placeholders for future external drivers
- In a real production setting, future values for marketing, pricing, macro indicators, holidays, and competitor activity should be provided explicitly
- The `display()` function is referenced in the notebook output section and may need `from IPython.display import display` depending on your environment

## Use Cases

This notebook is useful for:

- sales forecasting
- scenario planning
- marketing impact analysis
- price sensitivity exploration
- interpretable business forecasting prototypes

## Possible Improvements

Potential next steps for this project:

- tune ARIMA order using AIC or grid search
- compare against SARIMAX, Prophet, XGBoost, or LightGBM
- add backtesting across multiple rolling windows
- add forecast plots and diagnostics
- validate feature assumptions and multicollinearity
- support future driver scenario simulation

## License

This project is intended for educational and analytical use.
