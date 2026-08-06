# 📈 Apple Stock Analysis (AAPL)

Statistical analysis examining Apple Inc. stock records over the past five years, using daily OHLCV data from Yahoo Finance.

**August 2021 – August 2026 · 1,254 trading days**

---

## 📌 Project Overview

Apple is among the most heavily traded stocks on the market, so daily volume figures are large and consistent rather than sparse, and five years provides sufficient observations for real differences to surface.

Over this period AAPL moved from $146 to $311 (+112.8%), daily volume averaged 64.7M shares, and typical daily movement ran at 1.65%. Three common assumptions were tested against those figures:

- **Risk rises as a stock climbs.** With the price up 112.8%, daily volatility should have risen alongside it.
- **Volume signals price movement.** Up-days and down-days should differ measurably against the 64.7M average.
- **Certain months and weekdays perform better.** Calendar averages should separate by more than the 1.65% typical daily movement.

---

## 📊 The Data

| | |
|---|---|
| **Source** | Yahoo Finance via `yfinance` |
| **Period** | August 2021 – August 2026 |
| **Grain** | One row per trading day (1,254 rows) |
| **Fields** | Open, High, Low, Close, Adj Close, Volume |

The period covers three distinct market regimes: the end of the post-pandemic bull run, the 2022 rate-hike selloff that reduced Apple by nearly a third, and the recovery through 2026. Conclusions therefore do not rest on a single market condition.

Daily returns, rolling volatility, and trend measures are derived from these fields. Definitions and formulas are documented in the **[Data Dictionary](DATA_DICTIONARY.md)**.

---

## 🧰 Tech Stack

* **Language:** Python
* **Libraries:** pandas, NumPy, SciPy, Matplotlib, Seaborn, yfinance
* **Environment:** Jupyter Notebook

---

## 🔄 Workflow

**1. Data acquisition.** Retrieved 1,254 daily OHLCV records via `yfinance` and validated completeness before analysis.

**2. Feature engineering.** Derived daily returns, intraday range, a 30-day moving average for trend, and 30-day rolling volatility. Volatility was calculated on percentage returns rather than dollar prices so that values remain comparable across a period in which the price doubled.

**3. Descriptive and distribution analysis.** Summarised price, volume, and volatility overall and by year. Tested return normality using the D'Agostino test, with skewness and kurtosis to characterise the distribution.

**4. Correlation and hypothesis testing.** Built a correlation matrix across price, volume, return, volatility, and range. Applied a two-sample t-test comparing volume on up-days against down-days. The opening-price versus volume comparison was run within each year to control for the confound between rising prices and falling volume.

**5. Time-based analysis.** Calculated yearly returns, monthly seasonality (each year-month's return first, then averaged by month), and average returns by weekday.

---

## ✅ Results

Two of the three assumptions did not hold. In both cases the cause was a measurement choice rather than a market pattern.

| Question | Method | Result |
|---|---|---|
| Has AAPL become riskier as its price climbed? | Rolling volatility on % returns, by year | **No.** Volatility peaked in 2022 and sits near average today |
| Does volume signal price direction? | t-test on up-days vs down-days | **No.** Statistically indistinguishable (p = 0.267) |
| Do calendar patterns exist? | Monthly and weekday return averages | **Weakly.** A monthly pattern appears, no weekday effect at all |

Each result is detailed below.

---

## 📈 Key Findings

### 1. Apple more than doubled, but lost 28.6% in 2022

AAPL returned **+112.8%** over five years, a **16.3% CAGR**, rising from $146 to $311. Growth was uneven. Every year was positive except 2022, and the full five-year return was available only to a holder who remained invested through that decline.

![Price trend and yearly returns](images/price_and_yearly_returns.png)

*Left: daily closing price with its 30-day trend line. Right: return in each calendar year.*

---

### 2. The price rose, but risk did not

Measured on percentage returns, volatility shows **no upward trend**. 2022 was the most volatile year at 2.20% average daily movement, 2023 and 2024 were the calmest at approximately 1.3–1.4%, and 2026 sits near the five-year average of 1.65%.

The perception that Apple became riskier stems from measuring volatility in dollars. A $300 stock moves more dollars per day than a $150 stock at identical percentage volatility, so a dollar-based measure rises automatically with price. Correcting the measure reverses the conclusion.

![Volatility analysis](images/volatility_analysis.png)

---

### 3. Large single-day moves occur far more often than expected

Returns fail a normality test (**p < 0.001**) with **kurtosis of 6.64**, indicating that extreme daily moves occur far more frequently than a normal distribution predicts.

The Q-Q plot (top right) shows this directly. Under a normal distribution the points would follow the straight red line; instead they diverge sharply at both ends. Each divergent point represents a day that a normal model treats as near-impossible. Risk measures built on that assumption will understate the frequency of large moves.

![Return distribution](images/return_distribution.png)

---

### 4. Volume shows no relationship with price direction

Volume on up-days (63.9M shares) is statistically indistinguishable from down-days (65.7M): **p = 0.267**, correlation of 0.01.

The left chart compares days with a below-average opening price and above-average volume against days with an above-average open and above-average volume, measured **within each year**. The result is 241 against 210, a ratio of **1.15x**. The same comparison against five-year averages produced **4.8x**, suggesting that low prices attract heavy trading. That larger figure was an artifact: prices rose while volume fell across the period, so a full-period comparison conflates price level with time.

![Volume and price relationship](images/volume_price.png)

That decline is the more substantive volume finding. **Average daily volume fell approximately 39%** (85M to 51M shares) while the price more than doubled, producing a **-0.40** correlation between Close and Volume, the strongest relationship in the matrix below. Pct_Change against Volume sits at **0.01**, confirming that daily returns and volume are unrelated.

![Correlation matrix](images/correlation_matrix.png)

---

### 5. Monthly patterns are weak, weekday patterns absent

July (+6.7%), November (+5.3%), May (+4.3%) and October (+3.9%) averaged strongest, September weakest (-3.3%). The bars appear decisive, but each rests on five observations, so a single unusual year can reverse a month's sign.

![Monthly seasonality](images/monthly_seasonality.png)

Weekdays show no effect. Average returns range from -0.09% (Thursday) to +0.16% (Wednesday), a spread of roughly a quarter of a percent against typical daily movement of 1.6–1.9%.

![Weekday effect](images/weekday_effect.png)

---

## 💡 Methodological Takeaways

Three measurement decisions changed the conclusions. Each applies well beyond stock data.

**1. Compare in percentages when the base changes.**

Volatility measured in dollars appeared to double. Measured in percentages, it showed no trend. The price had doubled, so the dollar figure grew on its own. Same data, opposite conclusions.

**2. Check group comparisons for hidden time effects.**

Splitting days by price level suggested that cheaper days attracted 4.8x more heavy trading. Prices rose and volume fell over the period, so that split was largely separating earlier days from recent ones. Comparing each day against its own year reduced the gap to 1.15x. Where two variables trend in opposite directions, a whole-period split measures the trend rather than the relationship.

**3. Move the baseline when the series trends.**

Daily volume fell 39% over five years. A threshold set on the five-year average would flag a normal 2026 day as unusually quiet. Baselines must move with the data.

A fourth point concerns reporting rather than measurement: a 16.3% annual return reads as steady, but contains a -28.6% year. Averages on a volatile series should be reported alongside their worst period.

---

## 📁 Project Structure

```
aapl-stock-analysis/
│── images/                 # Charts exported from the notebook
│── Apple_Inc.ipynb         # Full analysis: code, outputs, insights
│── DATA_DICTIONARY.md      # Field definitions, formulas, caveats
│── README.md
│── LICENSE
```

---

## ⚠️ Limitations

* **No benchmark.** Whether +112.8% beat the market is untested. No S&P 500 or NASDAQ comparison was run.
* **Five observations per month.** September's -3.3% rests on five Septembers. One unusual year would move it.
* **Same-day only.** Volume was compared on up-days against down-days. Whether it predicts the next day is untested.
* **Price returns only.** Dividends are excluded, so actual shareholder return exceeded 112.8%.

*Informational analysis, not investment advice.*

---

## 🧑‍💻 Author

**Thrinesh Vuribindi** — Data Analyst

[![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/YOUR_LINKEDIN)
[![GitHub](https://img.shields.io/badge/GitHub-12100E?style=for-the-badge&logo=github&logoColor=white)](https://github.com/thrinesh13)
