# visuals/

This folder stores static chart exports when running the full pipeline script.

When you run `src/stock_intelligence_platform.py`, Plotly figures can be exported
here as PNG or SVG using `fig.write_image("visuals/chart_name.png")` after installing
the `kaleido` package:

    pip install kaleido

Charts available in the interactive dashboard (no export required):

| Chart | Description |
|---|---|
| aapl_price_dashboard.png | Candlestick + Bollinger Bands + RSI + Volume (3-panel) |
| multi_stock_performance.png | Normalised performance base 100, all 5 tickers |
| correlation_heatmap.png | Pearson correlation matrix, daily returns |
| return_distribution.png | Return histogram vs normal distribution with VaR line |
| drawdown_chart.png | Drawdown from rolling peak |
| sector_treemap.png | S&P 500 sectors by market cap and median PE |
| valuation_scatter.png | PE vs PB scatter for 499 S&P 500 companies |
| sector_bubble.png | Sector PE vs dividend yield bubble chart |
| monthly_seasonality.png | Average daily return by calendar month |
| rolling_beta.png | Rolling 30-day beta vs S&P 500 |
| macd_chart.png | MACD line, signal, histogram |
| actual_vs_forecast.png | Linear and Ridge regression predictions vs actual |

All charts are fully interactive in:
- `dashboard/stock_intelligence_dashboard.html` (open in any browser)
- `notebooks/stock_intelligence_platform.ipynb` (run in Jupyter or Colab)
