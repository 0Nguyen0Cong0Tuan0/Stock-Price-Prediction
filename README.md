# Stock Price Prediction

## Overview

The primary objective is to predict stock prices using time series forecasting models and enhance these predictions with sentiment analysis derived from news articles. The project aims to provide actionable insights for financial analysis, potentially aiding investment decisions by combining quantitative price predictions with qualitative sentiment data. The inclusion of interactive visualizations, as suggested by the repository’s context and attachments, indicates a user-friendly approach to exploring stock trends and sentiment impacts.

## Technologies and Models Used

The project employs a diverse set of models and tools, each tailored to specific aspects of stock price prediction and sentiment analysis:
- **ARIMA (AutoRegressive Integrated Moving Average)**: A statistical model used for time series forecasting, ARIMA captures linear trends and seasonality in stock price data. It’s particularly effective for short-term predictions and serves as a baseline for comparing more complex models. The repository’s context, including screenshots of an ARIMA-based AAPL stock price prediction interface, highlights its role in generating forecasts and backtest signals.
- **Prophet**: Developed by Facebook, Prophet is a forecasting tool designed for time series data with strong seasonal patterns. It handles missing data, trend changes, and holidays, making it suitable for stock price trend analysis. The conversation history confirms its use alongside ARIMA, likely for its robustness in financial time series with irregular patterns.
- **LSTM (Long Short-Term Memory)**: A type of recurrent neural network, LSTM is employed for advanced time series forecasting, leveraging deep learning to model complex, non-linear temporal dependencies. The conversation history indicates its use for stock price prediction, enhancing the project’s ability to capture intricate price movement patterns that statistical models might miss.
- **VADER (Valence Aware Dictionary and sEntiment Reasoner)**: A rule-based sentiment analysis model, VADER is applied to financial texts to gauge market sentiment. It’s noted in the conversation for its role in analyzing news articles, providing quick and interpretable sentiment scores that influence stock price predictions.
- **FinBERT**: A BERT-based model fine-tuned for financial texts, FinBERT extracts nuanced sentiment scores from news and reports. The conversation highlights its use alongside VADER, suggesting a dual approach to sentiment analysis, with FinBERT offering deeper contextual understanding for financial-specific content.


## Supporting Tools and Libraries

The project relies on a robust set of tools and libraries to support data collection, processing, and analysis:

- **Polygon.io**: A financial data provider used to fetch historical stock data, including prices, volume, and technical indicators like VWAP (Volume Weighted Average Price). The data_collection_cleaning.ipynb notebook demonstrates its use for retrieving data at various intervals (e.g., 5-minute, daily).
- **yfinance**: Employed to fetch financial reports, including income statements, balance sheets, and cash flow statements, as seen in the fetch_financial_reports function. It provides critical financial metrics like Diluted EPS, EBITDA, Free Cash Flow, and Net Debt.
- **NewsAPI**: Used to collect news articles related to specific stocks and broader market contexts, as implemented in the fetch_news_data function. It supports fetching company-specific and global/political news.
- **BeautifulSoup**: Utilized for web scraping to extract article content from news URLs, as shown in the extract_articles.ipynb notebook, enabling detailed sentiment analysis.
- **pandas** and **numpy**: Core libraries for data manipulation and numerical computations, used extensively in both notebooks for cleaning and processing data.
- **matplotlib** and **seaborn**: Employed for data visualization, creating plots like price trends and technical indicators, as indicated in the data_collection_cleaning.ipynb imports.
- **tkinter** and **tkcalendar**: Provide a graphical user interface for selecting tickers, dates, and intervals, enhancing user interaction with the data collection process.
- **requests**: Handles HTTP requests for fetching news data and scraping articles, ensuring robust data retrieval.

## Dataset
The project processes multiple datasets, each serving a specific role in the analysis:

- **Historical Stock Data**: Sourced from Polygon.io, this dataset includes columns such as Date, Open, High, Low, Close, Volume, Adj Close, Number of Trades, VWAP, Is OTC, Ticker, SMA_20, RSI, Returns, Volatility, and Sentiment_Score. The data_collection_cleaning.ipynb notebook fetches and cleans this data, ensuring quality by handling missing values and outliers.
- **Financial Reports**: Retrieved via yfinance, these include income statements, balance sheets, and cash flow statements, with year-based column names for metrics like Diluted EPS, EBITDA, Free Cash Flow, and Net Debt. The fetch_financial_reports function saves these as separate CSVs per ticker.
- **News Articles**: Collected using NewsAPI and enriched with scraped content via BeautifulSoup, this dataset includes Ticker, Date, Title, Description, Source, URL, and Content. The extract_articles.ipynb notebook processes these articles for sentiment analysis using VADER and FinBERT.
