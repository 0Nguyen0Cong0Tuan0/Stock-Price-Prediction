# **Main idea**

- To predict the next 7 days of *AAPL - [Apple Inc. for example]* stock closing prices by using historical stock price data and financial fundamentals as additional predictors.

- To evaluate model performance, tune model parameters and backtest a simple trading strategy to demonstrate practical applicability.

- The output includes forecasts, performance metrics, backtest results and visualizations.

Two models used:
1. **ARIMA** *(Auto-Regressive Integrated Moving Average)* _ a baseline time series model for forecasting stock prices based solely on historical closing prices.
2. **Prophet** _ an advanced model incorporating financial fundamentals (e.g., Diluted EPS, Free Cash Flow, Net Debt, EBITDA) as regressors, capturing trends, seasonality and external factors.

--

The workflow includes data preparation, model training and tuning, forecasting, evaluation, backtesting and visualization.

# **Workflow**

**1_Data preparation**

Input by loading daily stock prices and financial foundation information.

Preprocessing by filtering for ticker, sorts by date and remove duplicates.

Handling irregular dates (e.g., missing weekends) by creating a daily date range and forward-filling missing values and extracting key financial metrics and fowrard-filling to align with daily stock data.

Merging stock and financial data into a unified DataFrame.

Output is a DataFrame indexed by `Date` with daily stock prices and financial features.

**2_Model training and tuning**

**ARIMA** (*arima_forecast*)
- Uses `auto_arima` to automatically select the best order (p, d, q) up to `max_p=5`, `max_q=5`, `max_d=2`.
- Fits an ARIMA model on Close prices and forecasts 7 days ahead.
- Evaluates on the last 7 days (if available) using RMSE, MAE and MAPE.

**Prophet** (*prophet_forecast*, *tune_prophet*)
- Prepares data for Prophet (renames `Data` to `ds` and `Close` to `y`).
- Add financial regressors (Diluted EPS, Free Cash Flow, Net Debt, EBITDA).
- Tunes `changepoint_prior_scale` to optimize MAPE. 
- Fits Prophet with the best scale, forecasts 7 days and evaluates metrics.

**3_Walk-Forward validation**
- Performs 5 folds, each training on historical data and testing on the next 7 days.
- Computes average RMSE, MAE and MAPE for **ARIMA** and **Prophet**.
- Ensures realistic evaluation by simulating real-world forecasting scenarios.

**4_Backtesting** 
- Generates historical predictions for **ARIMA** and **Prophet**
- Implements a simple trading strategy (trading, not holding):
    - Buy (Signal = 1) if tomorrow's predicted price > today's
    - Sell (Signal = -1) if tomorrow's predicted price < today's
- Calculates daily strategy returns and cumulative return (%)
- Tracks the number of trades

**5_Visualization**
- Plot historical Close princes, **ARIMA** and **Prophet** 7-day forecasts, **Prophet** confidence intervals and backtest signals (buy/sell triangles)

# **Models used and rationale for selection**

**1_ARIMA (Auto-Regressive Integrated Moving Average)**
- A statistical model that forecasts time series data based on its own past values (auto-regression), differences (integration) and lagged forecast errors (moving average).
- ARIMA is a standard time series model, widely used in finance for stock price forecasting due to its simplicity and interpretability.
- It relies solely on Close price $\rightarrow$ making it robust when financial fundamentals are limited or unreliable - no external data required.

**Strengths**
- Easy to implement and interpret with clear statistical outputs (e.g., AIC, p-values).
- Performs well on differenced stock prices, which are often near-stationary.
- Suitable for small to medium datasets

**Weaknesses**
- Ignores external factors (e.g., financial metrics, news sentiment) $\rightarrow$ potentially missing key drivers of stock prices.
- Struggles with complex, non-linear patterns or sudden market shocks.
- Requires careful differencing $\rightarrow$ sensitive to non-stationary.

**2_Prophet**

- A forecasting model developed by Meta AI, designed for time series with trends, seasonality and holidays. It supports additional regressors (e.g., financial metrics) and is robust to missing data and outliers.
- Prophet handles trends, seasonality and external regressors (leveraging the financial dataset) to improve accuracy.
- Tuning to demonstrate creativity and advanced feature engineering.

**Strengths**
- Captures daily, weekly and yearly seasonality with external regressors $\rightarrow$ making it suitable for stock prices influenced by financial health.
- Handles gaps in dataset without requiring extensive preprocessing.
- Provides uncertainty estimates (confidence intervals), enhancing visualization and decision-making.
- Allow optimization for stock price volatility.

**Weaknesses**
- Slower than **ARIMA** (computationally intensive), esp with multiple regressors and tuning.
- May struggle with highly non-linear or multiplicative trends in stock prices.
- Effectiveness depends on the relevance of financial metrics, which are annual and forward-filled, potentially oversimplifying their impact.

# **Why these models over others?**

**Alternatives considered**
- **LSTM (Long Short-Term Memory)** _ A deep learning model for time series. Excluded because it requires extensive training, tuning and large datasets. Stock prices are also noisy, making LSTM prone to overfitting without significant preprocessing.
- **Random Forest/XGBoost** _ Regression models for time series features. Excluded because they require manual feature engineering (e.g., lagged prices, moving averages) and are less suited for direct time series forecasting compared to ARIMA and Prophet.

**Rationale**
- ARIMA provides a simple, interpretable baseline.
- Prophet leverages the financial datasets, handles data irregularities $\rightarrow$ making it ideal for an advanced model.
- Both models are widely used in finances.

# **Evaluation metrics and rationale**

Using **RMSE**, **MAE** and **MAPE** to evaluate model performance, computing during forecasting and walk-forward validation.

**Metrics defined**

**1_RMSE (Root Mean Squared Error)**

$$\sqrt{\frac{1}{n} \sum^n_{i=1}(y_i - \hat{y}_i)^2}$$

Measures the square root of the average squared differences between actual ($y_i$) and predicted ($\hat{y}_i$) prices.

**2_MAE (Mean Absolute Error)**

$$\frac{1}{n}\sum^n_{i=1}| y_i - \hat{y}_i |$$

Measures the average absolute differences between actual and predicted prices.

$$\frac{100}{n}\sum^n_{i=1} \left| \frac{y_i - \hat{y}_i}{y_i} \right|$$

Measures the average percentage error relative to actual prices.

**Why these metrics?**

**RMSE** 
- Penalizes large errors more heavily due to squaring  $\rightarrow$ making it sensitive to outliers (e.g., sudden stock price spikes) $\rightarrow$ this is critical in finance where large prediction errors can lead to significant losses.
- Highlights models that consistently predict close to actual prices, esp for volatile stocks like *AAPL*
- (Drawback) Overemphasizes outliers which may not always be the primary concern in short-term forecasting.

**MAE**
- Provides a straightforward measure of average error magnitude, less sensitive to outliers than RMSE. It's interpretable as the 'typical' dollar error.
- Useful for understanding the absolute accuracy of forecasts which is valuable for trading decisions.
- (Drawback) Does not penalize large errors as strongly, potentially underestimating risk.

**MAPE**
- Expresses error as a percentage, making it scale-invariant and easy to interpret (e.g., 1% error is intuitive). It's widely used in finance to compare performance across assets with different price levels.
- Ideal for evaluating relative accuracy for ticker's stock price (e.g., $190 - $200 range).
- (Drawback) Can be unstable if actual prices are near zero (maybe not an issue for ticker) and sensitive to small denominators.

**Why not other metrics?**