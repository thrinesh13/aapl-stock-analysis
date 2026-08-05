# Data Dictionary

This document explains every piece of data used in the analysis — what it is, where it comes from, and what it means in plain language. It is written for both technical and non-technical readers: each field has an everyday explanation first, with the exact formula alongside for anyone who wants to verify the calculation.

## The Dataset in One Paragraph

The dataset is Apple's daily stock trading record for 5 years (August 2021 – August 2026), sourced from Yahoo Finance. Each row is one trading day; weekends and market holidays don't appear because the market is closed, which is why 5 years yields 1,254 rows (roughly 252 trading days per year). For each day we know the price the stock started at, the highest and lowest it reached, where it ended, and how many shares changed hands. The data contains no gaps and no duplicates.

## Market Context

| | |
|---|---|
| **Ticker / Exchange** | AAPL, NASDAQ |
| **Currency** | All prices in US dollars |
| **Trading session** | Prices reflect regular market hours (9:30–16:00 US Eastern); pre-market and after-hours trading are not included |
| **Corporate actions** | No stock splits occurred in this window. Apple pays a quarterly dividend, which affects `Adj_Close` but not `Close` — see the dividend note under Raw Columns |

## Quick Glossary (for non-technical readers)

| Term | Plain meaning |
|---|---|
| **OHLCV** | The five standard facts recorded for each trading day: **O**pen, **H**igh, **L**ow, **C**lose prices and **V**olume |
| **Return** | How much the price changed, as a percentage. A stock going from $100 to $102 has a +2% return |
| **Volatility** | How much the price moves day to day. High volatility = large daily swings; low = steady prices. It does not mean the stock is falling — just moving |
| **Moving average** | The average price over the last N days, recalculated daily. Smooths out daily noise to reveal the trend |
| **CAGR** | Compound Annual Growth Rate — the steady yearly growth rate that would produce the same end result. Lets you compare investments over different time spans |
| **p-value** | The result of a statistical test. A small p-value (below 0.05) means a pattern is probably real; a large one means it could easily be random chance |
| **Correlation** | A score from -1 to +1 measuring whether two things move together. +1 = always together, -1 = always opposite, 0 = unrelated |

## Raw Columns (as downloaded from Yahoo Finance)

| Column | Type | Unit | What it means |
|---|---|---|---|
| `Date` | date (index) | — | The trading day. This is the row identifier |
| `Open` | float | USD | The first traded price of the day, when the market opened |
| `High` | float | USD | The most expensive the stock got that day |
| `Low` | float | USD | The cheapest the stock got that day |
| `Close` | float | USD | The final price when the market closed. **This is the price used for all calculations in this analysis** |
| `Adj_Close` | float | USD | The close adjusted for dividends and stock splits. Kept for reference but not used: this analysis studies daily price behavior, for which the unadjusted close is the correct basis. Consequence: returns quoted here are price returns and exclude dividend income |
| `Volume` | integer | shares | How many shares were bought and sold that day. A measure of trading activity, not price |

## Date Columns (derived from the Date)

| Column | Type | Values | What it means |
|---|---|---|---|
| `Year` | int | 2021–2026 | Calendar year, used for year-by-year comparisons. Note: 2021 and 2026 are partial years (~103 and ~148 trading days) |
| `Month` | int | 1–12 | Calendar month, used for the seasonality analysis |
| `Weekday` | text | Monday–Friday | Day of the week, used to test whether any weekday is systematically better or worse |

## Engineered Features (calculated in the notebook)

| Column | Unit | What it means | Exact formula |
|---|---|---|---|
| `Daily_Range` | USD | How wide the day's price swing was, in dollars. A big range = an eventful day | `High - Low` |
| `Daily_Change` | USD | Whether the stock ended above or below where it started the day. Positive = "up day" | `Close - Open` |
| `Pct_Change` | % | **The day's return** — how much the price changed vs the previous day's close, in percent. The single most-used measure in this analysis | `Close.pct_change() × 100` |
| `Range_Pct` | % | The day's swing as a percentage of the price, so a $2 swing on a $150 stock and a $4 swing on a $300 stock read the same | `(Daily_Range / Open) × 100` |
| `MA_30` | USD | The average closing price over the last 30 trading days. Drawn on charts to show the underlying trend beneath daily noise | `Close.rolling(30).mean()` |
| `Volatility_30` | % per day | **How bumpy the last 30 days were** — the typical size of a daily move over the last 30 trading days. Around 1% = calm; above 2% = turbulent. Deliberately calculated on percentage returns (not dollar prices) so that values from 2021 and 2026 are comparable even though the price doubled | `Pct_Change.rolling(30).std()` |

Note on blank values: `Pct_Change` is blank on the first day (nothing to compare to), and the rolling measures are blank for their first 29–30 days (the window isn't full yet). These blanks are expected and are excluded from all calculations.

## Summary Tables Built During the Analysis

These are not stored files — they are tables the notebook computes from the columns above.

| Table | One row per... | What it shows |
|---|---|---|
| Yearly summary | year | Average/min/max price, average volume, and average volatility for each year — the "year at a glance" view |
| Yearly returns | year | How much the stock gained or lost within each calendar year (first close to last close) |
| Monthly seasonality | calendar month | Each year-month's return is computed first (this month-end vs last month-end), then the 5 values for each month are averaged. This two-step order is what makes it a true seasonality measure |
| Weekday returns | weekday | Average and spread of daily returns for each day of the week |
| Correlation matrix | — | A 6×6 grid of correlation scores between price, volume, returns, volatility, and range |

## Data Quality Notes

- No missing values or duplicate rows in the raw download (verified in the notebook)
- Prices are rounded to 2 decimal places
- The data re-downloads on every notebook run, so figures shift slightly depending on the run date. All numbers quoted in the README come from the August 2026 execution
- Source data is provided by Yahoo Finance and is widely used for research, but is not an official exchange feed
