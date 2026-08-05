# Apple Stock (AAPL): 5-Year Statistical Analysis of Price, Risk & Volume

**A statistical analysis of Apple's daily trading behavior over five years — performance, risk, volume, and calendar effects. Every conclusion is backed by a testable number.**

Analysis period: August 2021 – August 2026 · 1,254 trading days · Python

---

## Table of Contents

1. [Business Problem](#business-problem)
2. [The Data](#the-data)
3. [Methodology](#methodology)
4. [Key Findings](#key-findings)
5. [Recommendations](#recommendations)
6. [Limitations](#limitations)
7. [Repository Guide](#repository-guide)
8. [How to Reproduce](#how-to-reproduce)

---

## Business Problem

Investors and analysts constantly make claims about stocks: "risk is rising," "high volume means the price is moving," "September is always bad for tech." These claims drive real decisions, but they are rarely tested.

This analysis tests three of those claims for Apple, one of the world's most widely held stocks:

| # | Question | Why it matters |
|---|----------|----------------|
| 1 | How has AAPL performed, and has it become riskier as its price climbed? | Determines whether risk models and position sizes need updating |
| 2 | Does trading volume tell us anything about price — level or direction? | Volume is widely used as a trading signal; if it carries no information, dashboards and strategies built on it are noise |
| 3 | Do calendar patterns (month, weekday) actually exist? | "Sell in September"-style rules are common; they should be verified before anyone acts on them |

Every conclusion below is traceable to a specific output in the notebook.

## The Data

| | |
|---|---|
| **Source** | Yahoo Finance, via the `yfinance` Python library |
| **Coverage** | 1,254 trading days, 2021-08-06 to 2026-08-05 |
| **Market / Currency** | NASDAQ, all prices in USD |
| **Grain** | One row per trading day |
| **Fields** | Open, High, Low, Close, Volume (daily OHLCV) plus engineered features |
| **Quality** | Zero missing values, zero duplicates (verified in the notebook) |

Every column, engineered feature, and derived table is documented in plain language in the **[Data Dictionary](DATA_DICTIONARY.md)** — written so a non-technical reader can follow it.

## Methodology

The analysis follows a standard analyst workflow, with two methodological choices worth highlighting:

1. **Data preparation** — download, quality checks, feature engineering (daily returns, rolling averages, rolling volatility)
2. **Descriptive statistics** — overall and by year
3. **Distribution analysis** — D'Agostino normality test, skewness, kurtosis
4. **Correlation analysis** — Pearson correlations across price, volume, and volatility measures
5. **Hypothesis testing** — t-test for volume on up-days vs down-days
6. **Time-based analysis** — yearly returns, monthly seasonality, weekday effects

**Choice 1 — Volatility measured on percentage returns, not dollar prices.** A $300 stock naturally moves more dollars per day than a $150 stock. Measuring volatility in dollars would falsely suggest risk doubled as the price rose. Percentage-based measurement removes that distortion — and reverses the conclusion (see Finding 1).

**Choice 2 — Confounder control in the volume analysis.** Over these 5 years, price rose while volume fell. A naive comparison "do low prices attract high volume?" would pick up that time trend and inflate the answer 4x. The analysis compares each day against its own year's averages instead (see Finding 2).

## Key Findings

### 1. Apple doubled — but did not get riskier

**The stock returned +112.8% (16.3% CAGR)**, rising from $146 to $311. The path was uneven: every year was positive except **2022, which fell -28.6%**. Anyone holding the full period had to sit through losing over a quarter of the investment's value in one year.

Measured correctly (on % returns), **volatility shows no upward trend**. 2022 was the most turbulent year (2.20% daily), 2023–2024 were the calmest (~1.3–1.4%), and 2026 sits near the 5-year average (1.55% vs 1.65%). The "risk is rising" narrative comes from measuring volatility in dollars — a measurement artifact, not a real change.

![Price trends and seasonality](images/trends_seasonality.png)

One risk caveat is real: daily returns are **not normally distributed** (p < 0.001) with **kurtosis of 6.64** — extreme days happen far more often than a bell curve predicts. Any risk model assuming normal returns will understate the chance of large moves.

![Return distribution](images/return_distribution.png)

### 2. Volume tells you nothing about price direction

- Volume on up-days (63.9M shares) is statistically identical to down-days (65.7M): **t-test p = 0.267**, correlation ≈ **0.01**
- The apparent "low prices attract volume" effect collapses from **4.8x to 1.15x** once compared within each year — it was a time-trend artifact, not real behavior
- The genuine volume story is structural: **average daily volume fell ~39%** (85M → 51M shares) while the price more than doubled (correlation **-0.40**)

![Volume vs price](images/volume_price.png)

### 3. Calendar patterns are weak — and one is pure noise

- **Monthly:** July (+6.7%), November (+5.3%), May (+4.3%) and October (+3.9%) were strongest on average; **September was weakest (-3.3%)**. But each monthly average rests on only 5 observations — one unusual year can flip these signs
- **Weekday:** no effect at all. Average returns range from -0.09% (Thursday) to +0.16% (Wednesday), trivial against daily noise of 1.6–1.9%

![Weekday effect](images/weekday_effect.png)

## Recommendations

For teams using this data to inform decisions or build monitoring tools:

1. **Retire volume as a direction signal.** It carries no statistically detectable information about whether the stock rises or falls on a given day. Dashboards and alerts built on "unusual volume = price move coming" are tracking noise.
2. **Use fat-tail-aware risk measures.** With kurtosis of 6.64, models that assume normal returns (including basic Value-at-Risk) will understate extreme-day risk. Use empirical percentiles of actual returns instead.
3. **Benchmark volume against the current year, not long-run history.** Volume declined ~39% structurally; comparing today's volume to a 5-year average will flag normal days as anomalies.
4. **Do not trade the seasonal patterns.** With only 5 observations per month, the July-strong / September-weak pattern is descriptive, not dependable. Revisit with 15–20 years of data before acting on it.
5. **Expect drawdowns inside long-term growth.** The 16.3% CAGR contained a -28.6% year. Any plan built on this stock's historical return must be able to survive its historical drawdown.

## Limitations

- Single ticker over a 5-year window — findings describe this period, they do not predict the next one
- Monthly seasonality is based on 5 observations per month; not statistically robust
- No benchmark comparison (vs S&P 500 / NASDAQ) — listed as future work
- Returns are computed on unadjusted close prices and therefore exclude dividend income; long-run total shareholder return is slightly higher than the price return reported here
- Data refreshes on every notebook run; figures above correspond to the August 2026 execution
- This analysis is informational and does not constitute investment advice

## Repository Guide

| File | What it is | Who it's for |
|---|---|---|
| `README.md` | This overview — problem, findings, recommendations | Everyone; no code required |
| `Apple_Inc.ipynb` | The full analysis: code, outputs, charts, and written insights after every result | Technical reviewers; GitHub renders it in the browser |
| `DATA_DICTIONARY.md` | Every column and metric explained in plain language, with formulas | Both audiences |
| `images/` | All charts exported from the notebook | Referenced by this README |
| `requirements.txt` | Python dependencies | Anyone reproducing the analysis |

## How to Reproduce

```bash
git clone https://github.com/YOUR_USERNAME/apple_stock_analysis.git
cd apple_stock_analysis
pip install -r requirements.txt
jupyter notebook Apple_Inc.ipynb
```

Run all cells top to bottom. Requires Python 3.10+ and pandas 2.2+. The notebook downloads fresh data, so exact figures will shift slightly with the run date.

## Tools

Python · pandas · NumPy · SciPy (statistical tests) · Matplotlib · Seaborn · yfinance · Jupyter

---

*Questions about the methodology or data? Open an issue in this repository.*
