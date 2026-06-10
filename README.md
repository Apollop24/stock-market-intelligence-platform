# Stock Market Intelligence and Portfolio Analytics Platform

**Author:** Philip Kibet  
**GitHub:** Apollop24  
**Methodology:** CRISP-DM | Tabachnick and Fidell (2019) | APA 7th Edition  
**Language:** Python 3.10+  
**Status:** Production-Ready Portfolio Project

---

## Overview

This project is a complete end-to-end business analytics and data science engagement
built around three real-world financial datasets totalling 3,315 records across 503
S&P 500 companies and nine years of daily equity prices. It is structured to demonstrate
the full range of skills expected of a senior data analyst or analytics consultant, from
raw data acquisition and statistical validation through predictive modelling and
executive-level visualisation.

The analytical pipeline answers four progressive business questions:

| Level | Question | Method |
|---|---|---|
| Descriptive | What happened in price and volume? | EDA, OHLCV charting, Bollinger Bands |
| Diagnostic | Why did returns behave this way? | Correlation, OLS regression, MACD, RSI |
| Predictive | What price is expected next? | Linear Regression, Ridge Regression |
| Prescriptive | What should a portfolio manager do? | Risk metrics, sector screening, Graham value filter |

---

## Repository Structure

```
stock-market-intelligence/
│
├── notebooks/
│   └── stock_intelligence_platform.ipynb   # Main Jupyter notebook (11 cells)
│
├── dashboard/
│   └── stock_intelligence_dashboard.html   # Self-contained interactive HTML dashboard
│
├── src/
│   ├── stock_intelligence_platform.py      # Full pipeline as a single runnable script
│   └── analytics_helpers.py                # Reusable helper functions and constants
│
├── data/
│   └── DATA_SOURCES.md                     # Full citations for all three datasets
│
├── reports/
│   └── METHODOLOGY.md                      # Detailed statistical methodology notes
│
├── visuals/
│   └── (charts exported here when running the script)
│
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Data Sources

All data is fetched at runtime from public GitHub URLs. No API keys, downloads, or
local file paths are required. The project runs identically on JupyterLab, Google Colab,
Anaconda, and any standard Python environment.

### Dataset 1 — S&P 500 Constituent Fundamentals

| Field | Detail |
|---|---|
| Publisher | DataHub / datasets organisation |
| Records | 503 companies, 14 variables |
| Variables | Symbol, Sector, Price, P/E, P/B, EPS, Market Cap, EBITDA, Dividend Yield |
| Period | Snapshot circa 2017-2018 |
| URL | https://raw.githubusercontent.com/datasets/s-and-p-500-companies-financials/master/data/constituents-financials.csv |

**Citation:** DataHub. (2018). *S&P 500 companies with financial information* [Dataset]. GitHub. https://github.com/datasets/s-and-p-500-companies-financials

---

### Dataset 2 — Apple Inc. Daily OHLCV Price Series

| Field | Detail |
|---|---|
| Publisher | Plotly Technologies Inc. |
| Records | 506 trading days |
| Variables | Date, Open, High, Low, Close, Volume, Adjusted Close, Bollinger Upper/Mid/Lower, Trend |
| Period | 2015-02-17 to 2017-02-16 |
| URL | https://raw.githubusercontent.com/plotly/datasets/master/finance-charts-apple.csv |

**Citation:** Plotly Technologies Inc. (2017). *Finance charts — Apple OHLCV* [Dataset]. GitHub. https://github.com/plotly/datasets

---

### Dataset 3 — Multi-Stock Daily Close Prices

| Field | Detail |
|---|---|
| Publisher | Plotly Technologies Inc. |
| Records | 2,306 trading days |
| Tickers | AAPL, MSFT, IBM, SBUX, GSPC (S&P 500 Index) |
| Period | 2007-01-03 to 2016-03-01 |
| URL | https://raw.githubusercontent.com/plotly/datasets/master/stockdata.csv |

**Citation:** Plotly Technologies Inc. (2016). *Stock market data — AAPL, MSFT, IBM, SBUX, S&P 500* [Dataset]. GitHub. https://github.com/plotly/datasets

---

## Analytical Pipeline

### Cell 1 — Environment Setup
Installs any missing packages, applies the NumPy 2.x compatibility patch for Plotly,
imports all libraries, and defines visual constants and helper functions.

**NumPy 2.x compatibility note:** NumPy 2.0 removed the `bool8` alias. Plotly versions
below 5.14 reference this attribute at import time and will crash with `AttributeError:
module 'numpy' has no attribute 'bool8'`. Cell 1 patches this with two lines before
any Plotly import, making the notebook safe on Anaconda, JupyterLab, and Colab
regardless of the installed NumPy version.

---

### Cell 2 — Data Acquisition and Feature Engineering
Downloads all three datasets and engineers 28 features per trading day.

| Feature Group | Features |
|---|---|
| Returns | DailyReturn, LogReturn, CumulativeReturn |
| Trend | MA20, MA50, MA200 |
| Volatility | Volatility20 (rolling annualised standard deviation) |
| Momentum | RSI(14), MACD(12,26,9), MACD Signal Line, MACD Histogram |
| Risk | Drawdown from rolling peak |
| Volume | Volume 20-day moving average |
| Price | High-Low Spread, Open-Close Change |
| Calendar | Month, Day of Week, Year, Quarter |

---

### Cell 3 — Data Quality Assessment (Tabachnick and Fidell, 2019)

Produces five APA-formatted console tables:

| Table | Test | Finding |
|---|---|---|
| Table 1 | Missing Value Analysis | Zero missing values in source OHLCV; rolling-window fields initialise from day 14/20/50 |
| Table 2 | Descriptive Statistics | Mean, SD, Min, Q1, Median, Q3, Max, Skewness, Kurtosis for six variables |
| Table 3 | Shapiro-Wilk Normality | W statistic and p-value for Close, DailyReturn, LogReturn |
| Table 4 | Z-Score Outlier Detection | 8 extreme returns (1.6%) identified; retained per Mandelbrot (1963) |
| Table 5 | Augmented Dickey-Fuller | Close prices: I(1) non-stationary. Returns: I(0) stationary |

---

### Cell 4 — AAPL Price Intelligence Chart

Three-panel interactive chart rendered inline in Jupyter:

- **Panel 1:** Candlestick with Bollinger Bands (20-day), MA50, MA200
- **Panel 2:** RSI(14) with overbought (>70) and oversold (<30) shaded zones
- **Panel 3:** Daily volume bars (green/red) with 20-day volume moving average

---

### Cell 5 — Multi-Stock EDA

Two interactive charts:

- **Chart 1:** Normalised performance, base 100, for all five tickers 2007-2016
  - AAPL: +807%   MSFT: +207%   IBM: +100%   SBUX: +547%   S&P 500: +40%
- **Chart 2:** Pearson correlation heatmap of daily returns (red-to-green scale)

Followed by **Table 6:** Full Pearson correlation matrix printed in APA format.

---

### Cell 6 — Statistical Analysis

Produces four APA-formatted tables:

**Table 7 — OLS Regression (Close ~ MA20 + Volume + RSI + Volatility20)**

| Metric | Value |
|---|---|
| R-squared | 0.9744 |
| Adjusted R-squared | 0.9742 |
| F-statistic | significant at p < .001 |

**Table 8 — Regression Diagnostics**

| Test | Statistic | Interpretation |
|---|---|---|
| Breusch-Pagan | p < .05 | Heteroscedastic residuals; robust standard errors advised |
| Durbin-Watson | 0.502 | Positive autocorrelation; expected in price-level regression |

**Table 9 — VIF Multicollinearity**  
MA20 shows high VIF by construction (collinear with Close in time series). This is
documented, not corrected, as the regression is illustrative rather than causal.

**Table 10 — Pearson and Spearman Correlation (Close vs Volume)**  
Both methods reported. Spearman rho preferred for monotonic non-linear relationships.

---

### Cell 7 — Risk Analytics

**Table 11 — AAPL Risk and Performance Metrics (2015-2017)**

| Metric | Value |
|---|---|
| Annualised Return | computed from data |
| Annualised Volatility | computed from data |
| Sharpe Ratio | 0.239 |
| Sortino Ratio | computed from data |
| Maximum Drawdown | -32.08% |
| Beta vs S&P 500 | 0.961 |
| VaR (95%, 1-day) | -2.49% |
| CVaR (95%, 1-day) | computed from data |
| Win Rate | computed from data |

Three inline charts:
- Drawdown from rolling peak (filled area, red)
- Return distribution vs theoretical normal (fat-tail visualisation)
- Rolling 30-day Beta for all four stocks vs S&P 500 (2007-2016)

**Table 12 — Multi-Stock Risk Comparison:** annualised return, volatility, Sharpe, VaR,
and 10-year total return for AAPL, MSFT, IBM, SBUX, and S&P 500.

---

### Cell 8 — Business Analytics

**Chart 1 — Sector Treemap:** All 11 S&P 500 sectors sized by total market
capitalisation, coloured by median P/E ratio on a red-yellow-green scale.

**Chart 2 — Valuation Landscape:** PE ratio vs P/B ratio scatter plot for 499 S&P 500
companies. Bubble size represents market capitalisation. Colour represents sector.
Hover shows company name, EPS, dividend yield, and Graham classification.

**Chart 3 — Sector Bubble Chart:** Median PE vs average dividend yield with bubble
size representing number of constituent companies.

**Chart 4 — Monthly Seasonality:** Average daily return by calendar month with
percentage-positive overlay. Bars coloured green (positive) or red (negative).

**Table 13 — Sector Summary Statistics:** Company count, total market cap, median PE,
median PB, and average dividend yield for all 11 sectors.

**Table 14 — Graham Value Screen:** Top 15 deep-value S&P 500 stocks scoring 100/100
on the composite Graham screen (PE below 33rd percentile, PB below 33rd percentile,
positive EPS, positive dividend yield), ranked by market capitalisation.

---

### Cell 9 — MACD and RSI Technical Indicators

Two multi-panel charts:

- **MACD Chart:** Close price with MA50 (upper panel) + MACD line, signal line, and
  histogram with green/red colouring (lower panel)
- **RSI Chart:** Close price (upper panel) + RSI with overbought/oversold zones
  and statistical summary of days in each zone (lower panel)

---

### Cell 10 — Predictive Analytics

**Training protocol:** 80/20 temporal split with 5-fold TimeSeriesSplit cross-validation.
No data leakage: test observations are strictly future relative to training observations.

**Table 15 — Model Comparison (out-of-sample test set)**

| Model | RMSE | R-squared | MAPE | CV R-squared |
|---|---|---|---|---|
| Linear Regression | low | 0.9650 | 0.719% | computed |
| Ridge (alpha=1.0) | low | 0.9648 | computed | computed |

**Table 16 — Feature Importance:** Absolute standardised coefficients from Linear
Regression normalised to sum to 100%. Top features: Close price level, MA20, MA50.

Three inline charts:
- Actual vs predicted close price on out-of-sample test set (both models overlaid)
- Residual plot with zero-line reference

---

### Cell 11 — Executive Recommendations

**Table 17 — Strategic Recommendations:** Eight business findings, each with a
concise finding statement and a specific, actionable recommendation derived directly
from the computed analytical outputs.

Topics covered: risk-adjusted return, systematic risk, downside risk management,
long-term alpha, value investing opportunity, predictive model deployment, sector
rotation strategy, and seasonal trading patterns.

All numerical values in the recommendations table are populated dynamically from
computed results, not hardcoded.

---

## Key Results Summary

| Finding | Value | Implication |
|---|---|---|
| AAPL 2-year Sharpe Ratio | 0.239 | Positive but below the 0.5 threshold for strong risk-adjusted return |
| AAPL Maximum Drawdown | -32.08% | Significant drawdown tolerance required |
| AAPL Beta vs S&P 500 | 0.961 | Near-market systematic risk |
| AAPL Daily VaR (95%) | -2.49% | Maximum expected single-day loss 19 out of 20 trading days |
| AAPL 10-year total return | +807% | Outperformed S&P 500 (+40%) by 767 percentage points |
| OLS Regression R-squared | 0.974 | MA20 and RSI explain 97.4% of variance in daily close price |
| Linear Regression R-squared (test) | 0.965 | Strong out-of-sample predictive accuracy |
| Prediction MAPE | 0.719% | Average prediction error below 1% of actual price |
| Deep Value stocks (Graham 100/100) | 64 | Out of 503 S&P 500 constituents |

---

## How to Run

### Option 1 — JupyterLab or Jupyter Notebook

```bash
git clone https://github.com/Apollop24/stock-market-intelligence.git
cd stock-market-intelligence
pip install -r requirements.txt
jupyter lab notebooks/stock_intelligence_platform.ipynb
```

Run cells in order from Cell 1 to Cell 11. Each cell is independent and clearly labelled.
If a cell fails, re-run Cell 1 and Cell 2 to restore all variables, then retry.

### Option 2 — Google Colab

1. Open https://colab.research.google.com
2. File, then Open notebook, then GitHub tab
3. Enter: `Apollop24/stock-market-intelligence`
4. Select `notebooks/stock_intelligence_platform.ipynb`
5. Run All

### Option 3 — Python Script

```bash
python src/stock_intelligence_platform.py
```

### Option 4 — Interactive Dashboard (no Python required)

Open `dashboard/stock_intelligence_dashboard.html` directly in any web browser.
All 12 charts are fully interactive with hover, zoom, and pan. No server required.

---

## Compatibility

| Environment | Status |
|---|---|
| JupyterLab 3+ | Verified |
| Jupyter Notebook 6+ | Verified |
| Google Colab | Verified |
| Anaconda (Windows, macOS, Linux) | Verified |
| Python 3.10+ | Required |
| NumPy 2.x | Supported (patch in Cell 1) |

---

## Methodological References

Graham, B., and Dodd, D. (1934). *Security analysis*. McGraw-Hill.

Mandelbrot, B. (1963). The variation of certain speculative prices. *The Journal of Business, 36*(4), 394-419. https://doi.org/10.1086/294632

Sharpe, W. F. (1964). Capital asset prices: A theory of market equilibrium under conditions of risk. *The Journal of Finance, 19*(3), 425-442. https://doi.org/10.1111/j.1540-6261.1964.tb02865.x

Sharpe, W. F. (1966). Mutual fund performance. *The Journal of Business, 39*(1), 119-138. https://doi.org/10.1086/294846

Sortino, F. A., and van der Meer, R. (1991). Downside risk. *The Journal of Portfolio Management, 17*(4), 27-31. https://doi.org/10.3905/jpm.1991.409343

Tabachnick, B. G., and Fidell, L. S. (2019). *Using multivariate statistics* (7th ed.). Pearson Education.

---

## License

This project is released under the MIT License. The underlying datasets carry their
own licenses as documented in `data/DATA_SOURCES.md`.

---

*Built by Philip Kibet — Data Analyst | Nairobi, Kenya*
