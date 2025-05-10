The **Auto-Regressive Integrated Moving Average (ARIMA)** model is a foundational tool in time series analysis, widely applied across domains like finance, economics, and supply chain management for forecasting temporal data.

### **ARIMA**

**Core concepts and algorithms**

ARIMA is a statistical model that forecasts time series data by leveraging its historical patterns through three components: **Auto-Regressive (AR)**, **Integrated (I)** and **Moving Average (MA)**. It is denoted as **ARIMA (p, d, q)**

- $p$ _ # lagged observations (AR terms)
- $d$ _ # differences to achieve stationarity
- $q$ _ # lagged forecast errors (MA terms)

**Math representation**

The ARIMA model combines

**AR**

$$y_t = c + \phi_1 y_{t-1} + \phi_2 y_{t-2} + ... + \phi_p y_{t-p} + \epsilon_t$$

where
- $\phi_i$ are AR coefficients 
- $\epsilon_t$ is white noise

**I** _ differencing to make the series stationary

$$y'_t = y_t - y_{t - 1}$$

**MA**

$$y_t = c + \epsilon_t + \theta_1 \epsilon_{t - 1} + \theta_2 \epsilon_{t - 2} + ... + + \theta_q \epsilon_{t - q}$$

where
- $\theta_i$ are MA coefficients 

For example, the paper *Forecasting of Demand Using ARIMA Model* specifies the ARIMA (1, 0, 1) model for demand forecasting:

$$y_t = 125.524 + 0.90792y_{t-1} - 0.6388\epsilon_{t-1}$$

This model uses one AR term ($p = 1$), no differencing ($d = 0$), one MA term ($q = 1$) and a constant term of 125.52, indicating a near-stationary series with simple autoregressive and error correction dynamics.

### **Algorithm workflow (Box-Jenkins Approach)**

ARIMA modeling follows the **Box-Jenkins methodology**, an iterative process detailed in both the paper and standard practice:

1. **Model identification**
- **Stationarity check** _ use tests like the **Augmented Dickey-Fuller (ADF) test** or visual inspection of **Auto-Correlation Function (ACF)** and **Partial Auto-Correlation Function (PACF)** plots.
- **Order selection** _ ACF identifies $q$ (MA terms) via significant lags while PACF identifies $p$ (AR terms).

2. **Parameter estimation**
- Coefficients ($\phi_i, \theta_i$) are estimated using **Maximum Likelihood Estimation (MLE)** or **Least Squares**. 
- Model selection minimizes **Akaike Information Criterion (AIC)**, **Schwarz Bayesian Criterion (SBC)**, **maximum likelihood** and **standard error**.

3. **Model diagnostics**
- Residuals are checked for randomness (**Ljung-Box test**), normality and homoscedasticity.
- If diagnostics fail, adjust $p$, $d$ or $q$.

4. **Forecasting**
- The fitted model generates point forecasts and confidence intervals.

5. **Iteration**
- Refine the model if diagnostics indicate issues. 

### **Applications of ARIMA**

ARIMA's versatility makes it applicable across domains.

- **Demand forecasting in food manufacturing**
    - **Context** _ The paper models monthly sales (in tons) of a final product from January 2010 to December 2025, forecasting demand for January to October 2016. The ARIMA (1, 0, 1) model accurately captures historical sales dynamics and provides reliable forecasts.

    - **Outcome** _ Forecasts enable precise production planning, raw material procurement and inventory management, reducing costs, stockouts and lead times. For perishable goods, accurate forecasts enhance supply chain efficiency and customer satisfaction.
    
    - **Significance** _ The paper emphasizes that forecasting accuracy drives inventory management performance, critical in a *pull* supply chain where customer demand dictates production.

- **Broader applications**
    - **Finance** _ Forecasting stock prices, returns or volatility. ARIMA's simplicity suits short-term forecasts though it struggles with non-linear market shocks.
    - **Economics** _ Predicting macroeconomic indicators like GDP, inflation or unemployment.
    - **Meteorology** _ Modeling temperature, rainfall or climate trends.
    - **Healthcare** _ Forecasting disease incidence (COVID-19 cases) or hospital admissions.
    - **Energy** _ Predicting electricity load or prices.
    - **Supply chain** _ Beyond food, ARIMA forecasts demand for retail, automotive or maintenance parts.

$\Rightarrow$ The paper's success with *stable*, *non-seasonal demand* aligns with ARIMA's strength for small to medium datasets, contrasting with its limitations in volatile domains like finance.

### **Impact of related fields**
ARIMA has shaped time series analysis and its applications, with the paper reinforcing its practical impact.

- **Supply chain management** _ The paper illustrates ARIMA's role in demand-driven supply chains, enabling collaboration across planning, procurement, manufacturing, and logistics. Accurate forecasts minimize inventory costs and enhance responsiveness, as over 74% of surveyed companies cite poor forecasting as a supply chain challenge.
- **Business analytics** _ ARIMA's interpretable outputs (e.g., coefficients, confidence intervals) provide managers with reliable guidelines as seen in the paper's application to production planning.
- **Statistical modeling** _ the **Box-Jenkins methodology** popularized systematic time series modeling, influencing tools like R, Python, and SPSS. ARIMA remains a benchmark for forecasting models.
- **Finance and Econometrics** _ ARIMA inspired extensions like *GARCH* or volatility modeling, addressing its linear limitations.
- **Machine Learning** _ ARIMA serves as a baseline for comparison with neural network (e.g., LSTM, Prophet) as noted in the paper's planned ANN exploration.
- **Education** _ ARIMA is a staple in statistics, econometrics, and data science curricula, introducing students to time series concepts like stationarity, autocorrelation, and differencing.

### **Strengths and Weaknesses**

**Strength**

- **Interpretability** _ ARIMA's parameters ($\phi, \theta$) have clear statistical meanings, making it easy to explain to stakeholders. 
- **Robust for stationary data** _ after differencing, ARIMA performs well on near-stationary series like stock returns or economic indictors.
- **Flexibility** _ can model a wide range of time series patterns by adjusting $p$, $d$ and $q$. 
- **No external data required** _ relies solely on the time series, making it robust when exogenous variables (e.g.,news sentiment) are unavailable or unreliable.
- **Statistical rigor** _ provides confidence intervals, p-values and goodness-of-fit metrics (AIC, BIC) for model evaluation.

**Weakness**
- **Stationary assumption** _ ARIMA assumes the time series can be made stationary, which is challenging for highly volatile or non-linear series (e.g., stock price during market crashes).
- **Linear model** _ can not capture complex, non-linear relationships or structural breaks (e.g., sudden policy changes).
- **No exogenous variables** _ ignores external drivers (e.g., interest rate, earning reports), limiting its predictive power in finance.
- **Sensitivity to parameters** _ incorrect $p$, $d$ or $q$ can lead to poor forecasts. Manual tuning or overfitting is a risk.
- **Short-term focus** _ ARIMA excels in short-term forecasting but struggles with long-term trends due to compounding errors.
- **Computationally intensive for large datasets** _ parameter estimation can be slow for high-order models or long time series.

### **Variation of ARIMA**

ARIMA has spawned several extensions to address its limitations:

**SARIMA (Seasonal ARIMA)**

**Definition** _ Extends ARIMA to handle seasonality, denoted as **ARIMA(p, d, q)(P, D, Q)m** where:
- $P$ _ seasonal AR order
- $D$ _ seasonal differencing 
- $Q$ _ seasonal MA order
- $m$ _ seasonal period (e.g., 12 for monthly data with yearly seasonality)

**Use case** _ models time series with recurring patterns (e.g., monthly retail sales with annual seasonality)

**Example** _ SARIMA (1, 1, 1)(1, 1, 1)12 for monthly data with yearly cycles.

**ARIMAX** 

**Definition** _ Incorporates exogenous variables into ARIMA, allowing external factors to influence the time series.

**Model** $y_t = \text{ARIMA}(p, d, q) + \beta X_t$

where
- $X_t$ are exogenous variables (e.g., interest rates, news sentiment)

**Use case** _ Stock price forecasting using financial metrics or macroeconomic indicators.

**Fractional ARIMA (ARFIMA)**

**Definition** _ Allows fractional differencing ($d$ is non-linear) to model long-memory processes.

**Use case** _ Time series with persistent autocorrelation (e.g., financial volatility)

**Vector ARIMA (VARIMA)**

**Definition** _ Extends ARIMA to multivariate time series, modeling multiple interrelated series simultaneously.

**Use case** _ Forecasting related economic indicators (e.g., GDP amd inflation)

**Non-linear ARIMA**

**Definition** _ Combines ARIMA with non-linear transformations or threshold (e.g., TARIMA for threshold ARIMA)

**Use case** _ Modeling time series with regime-switching behavior (e.g., stock price during bull vs bear markets)

### Related algorithms and models

ARIMA is part of broader family of time series models.

**Exponential Smoothing (ETS)**
- Models time series using weighted averages of past observations with weights decaying exponentially.
- **Variants** _ Simple, Holt's Linear, Holt-Winters (for seasonality)
- Comparison:
    - Similarities _ Like ARIMA, ETS is interpretable and handles trends/seasonality.
    - Difference _ ETS is less statistically rigorous (no stationary requirement) but easier to implement. ARIMA is better for complex patterns; ETS excels in short-term forecasting with clear trends/ seasonality.
- **Use case** _ sales forecasting, inventory management.

**GARCH** (Generalized Autoregressive Conditional Heteroskedasticity)
- Models time-varying volatility, often used in finance for stock returns or risk.
- Comparison:
    - Similarities _ Extends ARIMA's autoregressive concepts to volatility modeling.
    - Differences _ GARCH focuses on variance (volatility) while ARIMA models the mean. GARCH handles non-linear volatility clustering which ARIMA can not.
- **Use case** _ volatility forecasting, option pricing.

**Prophet**
- A facebook-developed model for time series forecasting, combining trend, seasonality and holiday effects.

- Comparison:
    - Similarities _ Like ARIMA, Prophet handles trends and seasonality.
    - Differences _ Prophet is more user-friendly, automatically handles missing data and incorporates exogenous variables. ARIMA requires manual tuning but is more statistically rigorous.

**Long Short-Term Memory (LSTM) networks**

- A deep learning model for sequence data, capturing long-term dependencies and non-linear patterns.
- Comparison:
    - Similarities _ Both forecast time series.
    - Differences _ LSTMs handle non-linear, complex patterns and exogenous variables but are less interpretable and require large datasets. ARIMA is simpler and better for small, linear datasets.

- **Use case** _ Stock price prediction, energy load forecasting.

**State space models (e.g., Kalman Filter)**

- Models time series as a system of latent states, allowing flexible modeling of trends, seasonality and noise.
- Comparison:
    - Similarities _ Like ARIMA, state space models are statistically grounded.
    - Differences _ State space models are more flexible (e.g., handle missing data, non-linearities) but computationally complex. ARIMA is simpler but less adaptable. 
- **Use case** _ real-time forecasting, signal processing.

### **Future directions and modern alternatives** 

ARIMA remains relevant but faces competition from modern methods:
- **Hybrid models** _ Combining ARIMA with machine learning (e.g., ARIMA-LSTM) leverages ARIMA's linear modeling and ML's non-linear capabilities.
- **Bayesian ARIMA** _ incorporates Bayesian methods for uncertainty quantification, improving robustness.
- **Transformer** _ Attention-based models (e.g., TimeGPT) outperform ARIMA on large datasets with complex patterns.
- **Real-time forecasting** _ ARIMA is less suited for streaming data, online learning models (e.g., adaptive ETS) are gaining traction. 

### **Conclusion**

ARIMA is powerful, interpretable and widely used model for time series forecasting, particularly in finance, economics and other fields with temporal data.

Its strengths lie in its simplicity, statistical rigor and ability to model linear, near-stationary series without external data.

However, its limitations - linear assumptions, stationarity requirements, and inability to incorporate exogenous factors - make it less suitable for complex, non-linear, or event-driven time series.

Variations like SARIMA, ARIMAX, and ARFIMA address some of these limitations, while related models like GARCH, Prophet, and LSTMs offer alternatives for specific use cases.

ARIMA’s legacy as a foundational model ensures its continued use, especially as a benchmark or component in hybrid approaches.

For practitioners, mastering ARIMA involves understanding its mathematical underpinnings, carefully tuning parameters, and recognizing when to complement it with modern methods.