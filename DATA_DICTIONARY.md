# Data Dictionary

Dataset: daily AAPL trading data downloaded from Yahoo Finance via `yfinance` (`period="5y"`, `interval="1d"`, `auto_adjust=False`). One row per trading day; the DataFrame index is `Date`. The executed notebook covers 1,254 trading days from 2021-08-06 to 2026-08-05.

## Index

| Field | Type | Description |
|---|---|---|
| `Date` | datetime | Trading date (index of the DataFrame). Weekends and market holidays are absent. |

## Raw Columns (from Yahoo Finance)

| Column | Type | Unit | Description |
|---|---|---|---|
| `Open` | float | USD | Price at market open. |
| `High` | float | USD | Highest traded price of the day. |
| `Low` | float | USD | Lowest traded price of the day. |
| `Close` | float | USD | Price at market close (unadjusted). Used for all return and volatility calculations in this project. |
| `Adj_Close` | float | USD | Close adjusted for dividends and splits (renamed from `Adj Close`). Not used in the core analysis. |
| `Volume` | int | shares | Number of shares traded during the day. |

## Date-Derived Columns

| Column | Type | Values | Description |
|---|---|---|---|
| `Year` | int | 2021-2026 | Calendar year of the trading day. Used for yearly grouping. Note: 2021 and 2026 are partial years (~103 and ~148 trading days). |
| `Month` | int | 1-12 | Calendar month of the trading day. |
| `Weekday` | string | Monday-Friday | Day-of-week name. Used in the weekday-effect analysis. |

## Engineered Features

| Column | Type | Unit | Formula | Description |
|---|---|---|---|---|
| `Daily_Range` | float | USD | `High - Low` | Intraday price range in dollars. |
| `Daily_Change` | float | USD | `Close - Open` | Intraday move in dollars. Positive = closed above the open ("up day"). |
| `Pct_Change` | float | % | `Close.pct_change() * 100` | Day-over-day return in percent (close vs previous close). First row is NaN. Primary return measure for distribution, weekday, and volatility analysis. |
| `Range_Pct` | float | % | `(Daily_Range / Open) * 100` | Intraday range as a percentage of the open, making ranges comparable across price levels. |
| `MA_30` | float | USD | `Close.rolling(30).mean()` | 30-day moving average of the close. Trend indicator. First 29 rows are NaN. |
| `Volatility_30` | float | % (daily) | `Pct_Change.rolling(30).std()` | 30-day rolling standard deviation of daily percentage returns. Computed on percentage returns (not dollar prices) so it is comparable across price levels. First 30 rows are NaN. |

## Derived Analysis Tables (created in the notebook, not stored)

| Object | Grain | Description |
|---|---|---|
| `yearly_summary` | one row per year | Count, mean, std, min, max of `Close`; mean `Volume`; mean `Volatility_30`. |
| `yearly_close` / `Return_%` | one row per year | Within-year return: first close to last close of each calendar year. |
| `monthly_ret` / `avg_monthly` | year-month → month | Each year-month's return (month-end close vs prior month-end), then averaged by calendar month across years (n = 5 per month). |
| `weekday_stats` | one row per weekday | Mean, std, and count of `Pct_Change` by weekday. |
| `correlation_matrix` | 6 × 6 | Pearson correlations between `Close`, `Volume`, `Daily_Change`, `Pct_Change`, `Volatility_30`, `Daily_Range`. |

## Data Quality Notes

- No missing values or duplicate rows in the raw download (verified in the notebook).
- Prices rounded to 2 decimal places after download.
- NaNs in `Pct_Change`, `MA_30`, and `Volatility_30` are expected warm-up artifacts of differencing/rolling windows and are excluded from calculations via `dropna()` or pandas' default NaN handling.
- Data refreshes on every run; figures quoted in the README correspond to the August 2026 execution.
