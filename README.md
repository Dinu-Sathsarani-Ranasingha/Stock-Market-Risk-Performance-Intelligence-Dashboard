# Stock Market Risk & Performance Intelligence Dashboard

**Question this project answers:** Across major sectors (Tech, Financials, Energy,
Healthcare, Consumer Staples), which stocks delivered the best *risk-adjusted*
returns over the last 5 years — and how did volatility shift around major
macro events?

## Why this project
Anyone can plot a stock price line. This project instead builds a full
pipeline — data engineering, SQL analytics, statistics, and an interactive
dashboard — to answer a real investment question a portfolio manager would
actually ask.

## Architecture
```
yfinance API --> raw CSVs --> PostgreSQL (relational schema) --> SQL window-function
analytics --> Python risk metrics (Sharpe, beta, drawdown) --> Tableau dashboard
```

## Tech stack
- **Python**: yfinance, pandas, numpy for data collection and risk metrics
- **SQL (PostgreSQL)**: relational schema + window-function analytics
- **Tableau**: interactive dashboard connected live to the database

## Project structure
```
stock-risk-dashboard/
├── data/
│   ├── raw/            # raw CSVs pulled from yfinance
│   └── processed/       # cleaned, metric-enriched data
├── python/
│   ├── 01_fetch_data.py     # pulls historical prices
│   └── 02_risk_metrics.py   # computes Sharpe, beta, drawdown, volatility
├── sql/
│   ├── 01_schema.sql          # relational schema (sectors, stocks, daily_prices)
│   └── 02_analysis_queries.sql # window-function analytics (returns, rolling vol, MAs)
├── notebooks/            # exploratory analysis / forecasting notebook goes here
├── requirements.txt
└── README.md
```

## How to run
1. `pip install -r requirements.txt --break-system-packages`
2. `python python/01_fetch_data.py` — pulls raw price data into `data/raw/`
3. `python python/02_risk_metrics.py` — computes risk summary into `data/processed/`
4. Load `sql/01_schema.sql` into PostgreSQL, then load the CSVs into the tables
   (or use `sql/*` as reference and adapt to SQLite if you don't want to run a server)
5. Run the queries in `sql/02_analysis_queries.sql` for the window-function analytics
6. Open Tableau → connect to your database (or the `risk_summary.csv` as a quick start)
   → build the 3 views described below

## Dashboard views (Tableau)
1. **Sector performance comparison** — bar chart, avg return by sector
2. **Risk-return scatter** — x = volatility, y = return, bubble size = market cap,
   color = sector. This is the "money view" — instantly shows who's
   over-compensated for risk and who isn't.
3. **Correlation heatmap** — which stocks move together (useful for diversification story)

## Key findings (fill in once you run it)
- Highest Sharpe ratio: ___
- Highest max drawdown (riskiest): ___
- Two most correlated stocks: ___
- Insight from volatility around a macro event: ___

## Next steps / extensions
- Add a Prophet or ARIMA forecast for 1–2 tickers (30-day horizon)
- Add a t-test comparing volatility before/after a chosen macro event (e.g., a
  Fed rate decision) to make a statistically-backed claim, not just a chart

## Video content
- 60–90 sec short: hook with one surprising risk/return insight → screen-record
  the Tableau scatter plot → CTA to GitHub
- Full YouTube walkthrough: problem → Python data pull → SQL window functions →
  key stat → live Tableau click-through → wrap-up
