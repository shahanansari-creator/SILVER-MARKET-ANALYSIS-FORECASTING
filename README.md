# SILVER-MARKET-ANALYSIS-FORECASTING

# 🥈 Silver Market Analysis & Price Forecasting

A 10-year exploratory data analysis, visualization, and time series forecasting project on daily silver (XAG/USD) trading data — from **January 29, 2016** to **January 23, 2026**.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)
![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

---

## 📖 Overview

Silver is a uniquely dual-natured asset — it trades simultaneously as a **monetary / safe-haven metal** (correlated with gold, the US dollar, and interest-rate expectations) and an **industrial commodity** (driven by demand from photovoltaics, electronics, and medical applications). This makes its price history a rich subject for multifactor analysis and regime-detection studies.

This project analyzes **2,606 daily trading sessions** spanning pre-pandemic stability, the March 2020 crash, the post-pandemic surge, and the high-volatility 2022–2026 environment. It walks through the full data science pipeline:

1. **Exploratory Data Analysis (EDA)** — price trends, returns, volatility, seasonality, correlations
2. **Time Series Diagnostics** — seasonal decomposition, stationarity testing
3. **Forecasting** — Naive baseline, ARIMA, and Random Forest models, compared on a held-out test window, plus a 30-day-ahead forecast

---

## 📂 Repository Contents

| File | Description |
|---|---|
| `Silver_Market_Analysis_Forecasting.ipynb` | The main, fully-executable Google Colab / Jupyter notebook — run top to bottom to reproduce all analysis, charts, and forecasts |
| `Silver_Market_Project_Report.docx` | A detailed written project report (Word doc) summarizing methodology, findings, and recommendations with embedded charts and tables |
| `silver_prices_10y.csv` | The source dataset (2,606 rows × 16 columns) |
| `README.md` | This file |

---

## 📊 Dataset

**File:** `silver_prices_10y.csv`
**Shape:** 2,606 rows × 16 columns
**Period:** 2016-01-29 → 2026-01-23 (business days only; weekends/holidays excluded)

| Column | Description |
|---|---|
| `Date` | Trading date (`YYYY-MM-DD`) |
| `Open`, `High`, `Low`, `Close` | Standard OHLC prices, USD per troy ounce |
| `Adj Close` | Close price adjusted for splits/distributions |
| `Volume` | Total units traded in the session |
| `Daily_Return` | % change vs. previous close (null on row 1 — no prior reference) |
| `MA_20`, `MA_50`, `MA_200` | Simple rolling means of `Close` (20/50/200-day windows) |
| `Volatility_20` | 20-day rolling standard deviation of `Daily_Return` |
| `Year`, `Month`, `Day_of_Week`, `Quarter` | Calendar decomposition of `Date` |

> **Note on missing values:** All nulls are explained by rolling-window warm-up periods, not data gaps — e.g. `MA_200` is null for the first 199 rows because a 200-day rolling mean has no value until 200 observations exist. This is verified in the notebook's data-quality section.

---

## 🔍 Key Findings

- Silver ranged from **$12.44 to $35.36/oz** over the decade (mean $21.73, std dev $5.66), moving through several distinct multi-year regimes rather than a single trend.
- Daily returns average **+0.042%** with a **1.49%** standard deviation, near-zero skew, and mildly fat tails versus a normal distribution.
- Volatility clusters in time rather than appearing as isolated spikes — the five most turbulent 20-day windows in the whole sample all fall in **July 2020**, in the aftermath of the COVID-19 shock.
- An Augmented Dickey-Fuller test confirms price **levels are non-stationary** (p = 0.674) while **first differences are stationary** (p < 0.001) — supporting an ARIMA(p,1,q) specification.
- On a 60-day test window, a simple **naive "last value" baseline (MAPE 1.29%)** was hard to beat — Random Forest came close (MAPE 1.39%), while ARIMA(5,1,0) underperformed both (MAPE 3.65%), underscoring how close short-horizon commodity prices are to a random walk.
- The 30-day-ahead ARIMA forecast projects silver at **≈$31.52 by March 6, 2026** (95% CI: $28.20–$34.85), essentially flat versus the last observed close of $31.48.

📄 See `Silver_Market_Project_Report.docx` for the full write-up with charts, tables, and methodology.

---

## 🧰 Tech Stack

- **Data handling:** `pandas`, `numpy`
- **Visualization:** `matplotlib`, `seaborn`, `plotly`
- **Time series & stats:** `statsmodels` (ADF test, seasonal decomposition, ARIMA)
- **Machine learning:** `scikit-learn` (Random Forest, evaluation metrics)

---

## 🚀 Getting Started

### Option A — Google Colab (recommended, zero setup)

1. Open [Google Colab](https://colab.research.google.com/) and upload `Silver_Market_Analysis_Forecasting.ipynb` (`File → Upload notebook`).
2. Run all cells (`Runtime → Run all`).
3. When prompted, upload `silver_prices_10y.csv` from your computer.

### Option B — Run locally

```bash
# Clone the repository
git clone https://github.com/<your-username>/silver-market-analysis.git
cd silver-market-analysis

# Install dependencies
pip install pandas numpy matplotlib seaborn plotly statsmodels scikit-learn jupyter

# Launch the notebook
jupyter notebook Silver_Market_Analysis_Forecasting.ipynb
```

Make sure `silver_prices_10y.csv` is in the same directory as the notebook — it will load automatically when running locally (no upload prompt needed).

---

## 📈 What's Inside the Notebook

1. **Setup & Data Loading** — environment-aware loading (Colab upload widget or local file)
2. **Data Overview & Quality Checks** — shape, dtypes, missing-value audit, OHLC consistency checks
3. **Exploratory Data Analysis**
   - Long-term price trend with 50/200-day moving averages
   - Interactive OHLC candlestick chart (last 12 months)
   - Trading volume analysis
   - Daily return distribution & rolling volatility
   - Seasonality (year, quarter, month, day-of-week)
   - Feature correlation heatmap
4. **Time Series Decomposition & Stationarity** — trend/seasonal/residual decomposition, ADF tests
5. **Forecasting Models** — Naive, ARIMA(5,1,0), Random Forest, with MAE/RMSE/MAPE comparison
6. **30-Day Forecast** — ARIMA projection with 95% confidence interval
7. **Summary of Key Findings** — takeaways and suggested next steps

---

## 🔮 Suggested Next Steps

- Add exogenous variables (gold price, USD index, real interest rates) for a multivariate model (ARIMAX, VAR)
- Replace the single train/test split with walk-forward (rolling-origin) validation
- Benchmark against LSTM/GRU or Facebook Prophet
- Apply regime-detection models (e.g., Hidden Markov Models) to formalize the volatility regimes identified in the EDA
- Extend volatility analysis with a GARCH-family model

---

## ⚠️ Disclaimer

This project is an educational demonstration of exploratory data analysis and time series forecasting techniques. It is **not financial or investment advice**. Forecasts are based solely on historical price patterns and cannot account for exogenous shocks such as geopolitical events, central bank policy changes, or sudden shifts in industrial demand.

---

## 📄 License

This project is licensed under the MIT License — feel free to use, modify, and share.
