# Apple Stock Analysis

![Python](https://img.shields.io/badge/Python-3.10+-blue) ![pandas](https://img.shields.io/badge/pandas-2.2+-150458) ![SciPy](https://img.shields.io/badge/SciPy-statistical%20testing-8CAAE6) ![License](https://img.shields.io/badge/License-MIT-green)

A statistical analysis of Apple (AAPL) daily trading data over five years, covering price performance, risk, trading volume, and calendar effects.

**August 2021 – August 2026 · 1,254 trading days**

---

## Overview

Three widely repeated claims about stocks drive real decisions but are rarely tested:

- Risk rises as a stock climbs
- Trading volume signals where the price is heading
- Certain months and weekdays are reliably better or worse

This analysis tests all three against Apple's daily trading record. Two of them do not survive the test.

| Question | Method | Answer |
|---|---|---|
| Has AAPL become riskier as its price climbed? | Rolling volatility on % returns, by year | **No** — volatility peaked in 2022 and is near average today |
| Does volume signal price direction? | t-test on up-days vs down-days | **No** — statistically indistinguishable (p = 0.267) |
| Do calendar patterns exist? | Monthly and weekday return averages | **Weakly** — a monthly pattern appears, no weekday effect at all |

Data sources, field definitions, and calculation formulas are documented in the **[Data Dictionary](DATA_DICTIONARY.md)**.

---

## Key Findings

### 1. Strong returns, with one severe drawdown

AAPL returned **+112.8%** over five years — a **16.3% CAGR** — rising from $146 to $311. Growth was not smooth. Every year was positive except **2022, which fell -28.6%**. Realising the five-year return required holding through a year that erased more than a quarter of the position's value.

![Price trends and seasonality](images/trends_seasonality.png)

---

### 2. The stock did not become riskier

Measured on percentage returns, volatility shows **no upward trend**. 2022 was the most turbulent year at 2.20% average daily movement; 2023–2024 were the calmest at ~1.3–1.4%; 2026 sits near the five-year average of 1.65%.

The common claim that Apple has grown riskier comes from measuring volatility in dollars. A $300 stock naturally moves more dollars per day than a $150 one, so a dollar-based measure rises automatically with price. Correcting the measurement reverses the conclusion.

![Volatility analysis](images/volatility_analysis.png)

---

### 3. Daily moves are far more extreme than normal models assume

Returns fail a normality test (**p < 0.001**) and carry **kurtosis of 6.64** — extreme days occur far more often than a bell curve predicts. Any risk model assuming normally distributed returns will understate the probability of large single-day moves.

![Return distribution](images/return_distribution.png)

---

### 4. Volume carries no information about price direction

Volume on up-days (63.9M shares) is statistically indistinguishable from down-days (65.7M): **p = 0.267**, correlation ≈ 0.01.

An apparent "low prices attract heavy volume" effect measured **4.8x** across the full period, but collapsed to **1.15x** once each day was compared against its own year's averages. The original signal was a time-trend artifact: prices rose while volume fell, so any full-period comparison conflates the two.

![Volume and price relationship](images/volume_price.png)

The genuine volume story is structural — **average daily volume fell ~39%** (85M to 51M shares) while the price more than doubled, producing a -0.40 correlation between price and volume.

![Correlation matrix](images/correlation_matrix.png)

---

### 5. Monthly patterns are weak; weekday patterns are absent

July (+6.7%), November (+5.3%), May (+4.3%) and October (+3.9%) averaged strongest, September weakest (-3.3%). Each average rests on only five observations, so a single unusual year can flip the sign.

Weekday returns range from -0.09% (Thursday) to +0.16% (Wednesday) — trivial against daily noise of 1.6–1.9%. There is no weekday effect.

![Weekday effect](images/weekday_effect.png)

---

## Recommendations

1. **Drop volume as a direction signal.** It carries no detectable information about daily price direction. Alerts built on "unusual volume precedes a move" track noise.
2. **Use fat-tail-aware risk measures.** With kurtosis of 6.64, normality-based models (including standard Value-at-Risk) understate extreme-day risk. Use empirical return percentiles instead.
3. **Benchmark volume against the current year.** Volume declined ~39% structurally; comparing against a five-year average will flag normal days as anomalies.
4. **Do not trade the monthly patterns.** Five observations per month is insufficient. Re-test with 15–20 years before acting.
5. **Size positions for the drawdown, not the average.** A 16.3% CAGR contained a -28.6% year.

---

## Method

Standard analyst workflow: data acquisition → quality checks → feature engineering → descriptive statistics → distribution testing (D'Agostino, skewness, kurtosis) → correlation analysis → hypothesis testing (t-test) → time-based analysis.

Two methodological choices materially changed the conclusions:

- **Volatility measured on percentage returns, not dollar prices**, removing the price-level distortion that made risk appear to rise (Finding 2)
- **Within-year comparison in the volume analysis**, controlling for the confound between rising prices and falling volume (Finding 4)

---

## Limitations

- Single ticker over a five-year window; findings describe this period rather than predict the next
- Monthly seasonality rests on five observations per month and is not statistically robust
- No benchmark comparison against S&P 500 or NASDAQ — noted as future work
- Returns are price returns and exclude dividend income
- Informational analysis; not investment advice

---

## Repository

| File | Contents |
|---|---|
| `README.md` | This summary — questions, findings, recommendations |
| `Apple_Inc.ipynb` | Full analysis: code, outputs, charts, written insight after every result |
| `DATA_DICTIONARY.md` | Data flow, field definitions, formulas, caveats |
| `images/` | Charts exported from the notebook |
| `requirements.txt` | Python dependencies |

## Reproduce

```bash
git clone https://github.com/YOUR_USERNAME/apple_stock_analysis.git
cd apple_stock_analysis
pip install -r requirements.txt
jupyter notebook Apple_Inc.ipynb
```

Requires Python 3.10+ and pandas 2.2+. The notebook downloads fresh data on each run, so figures shift slightly with the run date.

---

*Questions about the methodology or data? Open an issue in this repository.*
