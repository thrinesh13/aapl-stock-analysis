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

## ✅ Results

Two of the three assumptions did not hold. In both cases the cause was a measurement choice rather than a market pattern.

| Question | Method | Result |
|---|---|---|
| Has AAPL become riskier as its price climbed? | Rolling volatility on % returns, by year | **No.** Volatility peaked in 2022 and sits near average today |
| Does volume signal price direction? | t-test on up-days vs down-days | **No.** Statistically indistinguishable (p = 0.267) |
| Do calendar patterns exist? | Monthly and weekday return averages | **Weakly.** A monthly pattern appears but rests on five observations per month; no weekday effect at all |

Each result is detailed below, along with a fourth that emerged from the volume analysis: daily volume fell 39% across the period.

---

## 📈 Key Findings

### 1. Apple more than doubled, but lost 28.6% in 2022

AAPL returned **+112.8%** over five years, a **16.3% CAGR** (compound annual growth rate), rising from $146 to $311. Growth was uneven. Every year was positive except 2022, and the full five-year return was available only to a holder who remained invested through that decline.

![Price trend and yearly returns](images/price_and_yearly_returns.png)

*Left: daily closing price with its 30-day trend line. Right: return in each calendar year. 2021 and 2026 are partial years, so their bars cover five and seven months respectively.*

---

### 2. The price rose, but risk did not

**The price climbed 112.8%. Volatility did not follow.** Measured on percentage returns there is **no upward trend**: 2022 was the most volatile year at 2.20% average daily movement, 2023 and 2024 were the calmest at approximately 1.3–1.4%, and 2026 sits near the five-year average of 1.65%.

The perception that Apple became riskier stems from measuring volatility in dollars. A $300 stock moves more dollars per day than a $150 stock at identical percentage volatility, so a dollar-based measure rises automatically with price. **It tracks the price, not the risk.** Correcting the measure reverses the conclusion.

![Volatility analysis](images/volatility_analysis.png)

*Left: 30-day rolling volatility against the five-year mean. Right: distribution by year.*

The chart's one dramatic feature does not change this. Early 2025 peaks at **4.4%**, the most volatile 30-day stretch in the sample and above the 3.12% high of 2022. Yet 2025's median day was 1.61%, below the five-year average, and the boxplot confines the episode to the upper outliers with the interquartile range untouched. A peak nearly three times the year's own typical day is **an isolated shock, not a new baseline**.

---

### 3. Large single-day moves occur far more often than expected

**Any risk model that assumes a normal distribution will understate how often Apple moves sharply.** Returns fail a normality test (**p < 0.001**) with **kurtosis of 6.64**, meaning extreme daily moves occur far more frequently than a bell curve predicts. Terms in this section are defined in the [Glossary](DATA_DICTIONARY.md#glossary).

The Q-Q plot (top right) shows this directly. Under a normal distribution the points would follow the straight red line; instead they diverge sharply at both ends. Each divergent point is a day the normal model treats as near-impossible and that happened anyway.

![Return distribution](images/return_distribution.png)

---

### 4. Volume shows no relationship with price direction

Volume on up-days (63.9M shares) is statistically indistinguishable from down-days (65.7M): **p = 0.267**, correlation of 0.01. A gap that small appears by chance often enough that it is not evidence of a pattern.

---

### 5. Trading volume fell 39% as the price doubled

**Average daily volume declined from approximately 85M shares to 51M** while the price rose 112.8%. This produces a **-0.40** correlation between Close and Volume, the strongest relationship in the matrix below.

![Correlation matrix](images/correlation_matrix.png)

The correlation does not mean high prices suppress trading. Both measures trend across the period in opposite directions, and correlation registers that. `Pct_Change` against Volume sits at **0.01**, confirming that daily returns and volume are unrelated.

The same time trend distorted a related comparison. Measured against five-year averages, days with a below-average opening price and above-average volume outnumbered high-price heavy-volume days **4.8x**, suggesting that low prices attract heavy trading. But below-average price and above-average volume both describe the early years, so that split was separating 2021 from 2026 rather than cheap days from expensive ones. Comparing each day against its own year's averages gives 241 against 210, a ratio of **1.15x**.

![Volume and price relationship](images/volume_price.png)

*Left: heavy-volume days by price level, compared within each year rather than across the full period.*

---

### 6. Calendar patterns are too thin to rely on

**The monthly bars look decisive, but each rests on five observations.** July (+6.7%), November (+5.3%), May (+4.3%) and October (+3.9%) averaged strongest and September weakest (-3.3%), yet one unusual year would reverse a month's sign. The chart describes five specific years rather than a repeatable pattern.

![Monthly seasonality](images/monthly_seasonality.png)

Weekdays show nothing at all. Average returns range from -0.09% (Thursday) to +0.16% (Wednesday), a spread of roughly a quarter of a percent against typical daily movement of 1.6–1.9%.

![Weekday effect](images/weekday_effect.png)

---

## 💡 Methodological Takeaways

Four points where a reasonable measurement choice would have produced the wrong answer.

| Decision | Measured the obvious way | After correction |
|---|---|---|
| **Volatility unit** — dollars or percentage returns | Risk appears to roughly double | No trend across five years |
| **Comparison baseline** — five-year average or within-year | Low-price days draw 4.8x more heavy trading | 1.15x |
| **Reporting an average** — headline rate alone or with its worst period | 16.3% CAGR reads as steady | Contains a -28.6% year |
| **Sample size** — how many observations sit behind an average | July's +6.7% reads as a seasonal edge | Five observations; one year flips the sign |

Where two variables trend in opposite directions, a whole-period comparison measures the trend rather than the relationship. That single problem produced two of the four errors above.

---

## 📁 Project Structure

```
aapl-stock-analysis/
│── images/                 # Charts exported from the notebook
│── Apple_Inc.ipynb         # Full analysis: code, outputs, insights
│── DATA_DICTIONARY.md      # Field definitions, formulas, caveats
│── README.md
```

---

## 🧰 Tech Stack

* **Language:** Python
* **Libraries:** pandas, NumPy, SciPy, Matplotlib, Seaborn, yfinance
* **Environment:** Jupyter Notebook

---

## 🔄 Workflow

**1. Data acquisition.** Retrieved 1,254 daily OHLCV records via `yfinance` and confirmed 0 missing values and 0 duplicate rows. Unadjusted `Close` was used throughout, since the analysis studies daily price behaviour rather than total shareholder return.

**2. Feature engineering.** Derived daily returns, intraday range, a 30-day moving average for trend, and 30-day rolling volatility. Volatility was calculated on percentage returns rather than dollar prices so that values remain comparable across a period in which the price doubled.

**3. Descriptive and distribution analysis.** Summarised price, volume, and volatility overall and by year. Tested return normality using the D'Agostino test, with skewness and kurtosis to characterise the distribution.

**4. Correlation and hypothesis testing.** Built a correlation matrix across price, volume, return, volatility, and range. Applied a two-sample t-test comparing volume on up-days against down-days. The opening-price versus volume comparison was first run against five-year averages, then rerun within each year once the rising-price and falling-volume trends were found to be driving the result.

**5. Time-based analysis.** Calculated yearly returns, monthly seasonality (each year-month's return first, then averaged by month), and average returns by weekday.

---

## ⚠️ Limitations

* **No benchmark.** Whether +112.8% beat the market is untested. No S&P 500 or NASDAQ comparison was run.
* **Five observations per month.** September's -3.3% rests on five Septembers. One unusual year would move it.
* **Partial first and last years.** The window runs August to August, so 2021 covers five months and 2026 seven. Yearly returns and volatility for those two are not comparable to the full years.
* **Same-day only.** Volume was compared on up-days against down-days. Whether it predicts the next day is untested.
* **Price returns only.** Dividends are excluded, so actual shareholder return exceeded 112.8%.

*Informational analysis, not investment advice.*

---

## 🧑‍💻 Author

**Thrinesh Vuribindi** — Data Analyst

[![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/thrineshvuribindi)
[![GitHub](https://img.shields.io/badge/GitHub-12100E?style=for-the-badge&logo=github&logoColor=white)](https://github.com/thrinesh13)
