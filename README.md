# Apple Inc. (AAPL) — 5-Year Stock Market Analysis

A data analysis project examining Apple's stock performance from August 2021 to August 2026 using daily trading data. Built in Python with pandas, and combining descriptive statistics, hypothesis testing, and visualization.

## What This Project Answers

1. How has AAPL's price performed over 5 years, and has its risk profile changed?
2. Is trading volume related to price level or daily price direction?
3. Are there calendar patterns in returns, by month or by weekday?

## Key Findings (Plain English)

**Apple more than doubled, but not smoothly.** The stock returned 112.8% over 5 years (16.3% per year compounded), rising from $146 to $311. Every year was positive except 2022, when it fell 28.6%. Anyone holding for the full period had to sit through losing over a quarter of the investment's value in a single year.

**The stock did not get riskier as it got more expensive.** Measured properly (on percentage returns, not dollars), volatility peaked in 2022, was calmest in 2023-2024, and is currently near its 5-year average. A naive dollar-based measure would wrongly suggest risk doubled.

**Volume tells you nothing about direction.** Trading volume on days the stock rose is statistically identical to days it fell (p = 0.267). Meanwhile, overall volume declined about 39% across the period even as the price climbed.

**Daily swings are wilder than textbook models assume.** Returns are not normally distributed. Extreme moves happen far more often than a bell curve predicts (kurtosis 6.6), which matters for anyone modeling risk.

**Calendar patterns are weak.** July and November were the strongest months on average and September the weakest, but with only 5 years of data these are patterns, not predictions. There is no weekday effect at all.

![Price trends and seasonality](images/trends_seasonality.png)

## Technical Highlights

- **Data:** 1,254 trading days of OHLCV data from Yahoo Finance via `yfinance`
- **Feature engineering:** daily returns, price ranges, 30-day moving average, 30-day rolling volatility computed on percentage returns so it stays comparable across price levels
- **Statistical methods:** D'Agostino normality test, skewness/kurtosis, Welch-style t-test for volume by price direction, correlation analysis
- **Bias control:** the volume-vs-price-level comparison is done within each year, because prices rose while volume fell over the sample. The naive full-period comparison inflates the effect from 1.15x to 4.8x, a worked example of controlling for a confounder
- **Seasonality done correctly:** each year-month's return is computed first, then averaged by month, avoiding the trap of measuring multi-year growth instead of seasonality

![Volatility analysis](images/volatility_analysis.png)

## Repository Structure

```
├── Apple_Inc.ipynb        # Full analysis notebook (executed, with outputs)
├── DATA_DICTIONARY.md     # Every column and derived table, with types, units, and formulas
├── images/                # Charts exported from the notebook
├── requirements.txt       # Python dependencies
├── LICENSE
└── README.md
```

## How to Run

```bash
git clone <your-repo-url>
cd <repo-folder>
pip install -r requirements.txt
jupyter notebook Apple_Inc.ipynb
```

Run all cells top to bottom. The notebook downloads fresh data from Yahoo Finance, so exact figures will differ slightly from those quoted here depending on the run date. Requires Python 3.10+ and pandas 2.2+.

## Limitations

- Single ticker over a 5-year window; findings are descriptive, not predictive
- Monthly seasonality averages rest on only 5 observations per month
- Past performance does not guarantee future results; nothing here is investment advice

## Tools Used

Python · pandas · NumPy · Matplotlib · Seaborn · SciPy · yfinance · Jupyter
