# Data Dictionary

This document describes the data used in the analysis
---

## 1. Source Data

Daily trading records for Apple (AAPL) downloaded from Yahoo Finance via the `yfinance` library.

| | |
|---|---|
| **Period** | 2021-08-06 to 2026-08-05 |
| **Rows** | 1,254 — one per trading day (~252 trading days per year; weekends and market holidays are closed) |
| **Exchange / Currency** | NASDAQ · USD |
| **Session** | Regular market hours (9:30–16:00 US Eastern); pre-market and after-hours not included |

### Columns

| Column | Type | Unit | Description |
|---|---|---|---|
| `Date` | date (index) | — | Trading day; row identifier |
| `Open` | float | USD | First traded price of the day |
| `High` | float | USD | Highest price of the day |
| `Low` | float | USD | Lowest price of the day |
| `Close` | float | USD | Final price at market close. Basis for all calculations in the analysis |
| `Adj_Close` | float | USD | Close adjusted for dividends and splits. Not used: the analysis studies daily price behavior, so unadjusted close is the correct basis. Returns therefore exclude dividend income |
| `Volume` | integer | shares | Shares traded during the day |

No stock splits occurred in this period. Apple's quarterly dividend affects `Adj_Close` only.

---

## 2. Data Preparation

Steps applied after download, before any analysis:

1. Column names flattened; `Adj Close` renamed to `Adj_Close`
2. Prices rounded to 2 decimal places
3. Three calendar fields derived from `Date`:

| Column | Type | Values | Purpose |
|---|---|---|---|
| `Year` | int | 2021–2026 | Year-by-year comparison. 2021 and 2026 are partial years (~103 and ~148 days) |
| `Month` | int | 1–12 | Monthly seasonality analysis |
| `Weekday` | text | Monday–Friday | Weekday effect analysis |

4. Quality checks: **0 missing values, 0 duplicate rows**

---

## 3. Engineered Features

Calculated from the prepared data. These are the measures the analysis runs on.

| Column | Unit | Description | Formula |
|---|---|---|---|
| `Daily_Range` | USD | Width of the day's price swing | `High - Low` |
| `Daily_Change` | USD | Intraday move; positive = closed above the open ("up day") | `Close - Open` |
| `Pct_Change` | % | Daily return: change vs the previous day's close. The core measure of the analysis | `Close.pct_change() × 100` |
| `Range_Pct` | % | Day's swing relative to price level, comparable across years | `(Daily_Range / Open) × 100` |
| `MA_30` | USD | 30-day moving average of the close; trend line | `Close.rolling(30).mean()` |
| `Volatility_30` | % per day | Typical size of a daily move over the last 30 trading days. ~1% = calm, above 2% = turbulent. Computed on percentage returns so 2021 and 2026 values are directly comparable despite the price doubling | `Pct_Change.rolling(30).std()` |

`Pct_Change` is empty on the first row and the rolling measures on their first 29–30 rows (window not yet full). These are expected and excluded from all calculations.

---

## 4. Analysis Outputs

Summary tables the notebook computes from the features above (not stored as files).

| Output | Grain | Contents |
|---|---|---|
| Yearly summary | one row per year | Average/min/max close, average volume, average volatility |
| Yearly returns | one row per year | Return within each calendar year (first close to last close) |
| Monthly seasonality | one row per calendar month | Each year-month's return computed first (month-end vs prior month-end), then averaged per month across the 5 years |
| Weekday returns | one row per weekday | Mean, standard deviation, and count of daily returns |
| Correlation matrix | 6 × 6 | Pearson correlations between close, volume, daily change, daily return, volatility, and range |

---

## 5. Notes & Caveats

- Returns are price returns from unadjusted close and exclude dividend income; long-run total shareholder return is slightly higher
- The notebook re-downloads data on every run, so figures shift with the run date; all numbers in the README come from the August 2026 execution
- Yahoo Finance data is widely used for research but is not an official exchange feed

---

## Glossary

| Term | Meaning |
|---|---|
| **OHLCV** | The five standard daily facts: Open, High, Low, Close prices and Volume |
| **Return** | Percentage change in price ($100 → $102 = +2%) |
| **Volatility** | Size of day-to-day price swings; high volatility means large moves in either direction, not a falling price |
| **Moving average** | Average price over the last N days, recalculated daily; smooths noise to show trend |
| **Correlation** | Score from -1 to +1 for whether two measures move together; 0 = unrelated |
| **p-value** | Result of a statistical test; below 0.05 means the pattern is unlikely to be random chance |
