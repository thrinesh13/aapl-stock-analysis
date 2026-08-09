# 📈 Apple Stock Analysis (AAPL)

Statistical analysis of Apple Inc. stock testing four common market assumptions and examining price behavior and return distribution across five years of daily trading data.

**August 2021 to August 2026. 1,254 trading days**

---

## 📌 Context

Apple is among the most heavily traded stocks, providing consistent daily volume and sufficient observations to detect real market patterns. Over the study period, AAPL moved from **$146 to $311 (+112.8% return, 16.3% CAGR)** across three distinct market regimes: **post-pandemic bull run (2021), rate-hike selloff (2022), and recovery (2023-2026)**. This variation eliminates confounding by a single market condition.

---

## 📊 Data

| Attribute | Value |
|---|---|
| **Source** | Yahoo Finance (`yfinance`) |
| **Period** | August 2021 to August 2026 |
| **Observations** | 1,254 trading days |
| **Fields** | Open, High, Low, Close, Adj Close, Volume |

Derived measures: daily returns, intraday range, 30-day rolling volatility (% returns), and 30-day moving average for trend analysis.

**Full documentation available in [Data Dictionary](DATA_DICTIONARY.md).**

---

## ⚡ Executive Summary

| Finding | Result | Key Insight |
|---|---|---|
| Higher price correlates with increased volatility | ❌ Rejected | Percentage volatility remains flat; dollar-denominated measures conflate price level with risk |
| Trading volume predicts price direction | ❌ Rejected | Volume on up-days and down-days is statistically indistinguishable (p = 0.267) |
| Calendar effects exist (monthly/weekly patterns) | ⚠️ Insufficient evidence | Observed patterns rest on 5 observations per month; requires extended observation to validate |
| Volume decline with price appreciation | ✓ Confirmed | 39% volume reduction despite 112.8% price increase; driven by time trends, not market mechanics |
| Price progression was uneven across years | ✓ Documented | 2022's -28.6% loss interrupts the narrative; full return only realized by holding through downturns |
| Returns follow normal distribution | ❌ Rejected | Fat tails present (kurtosis 6.64); extreme moves occur far more frequently than normal model predicts |

---

## 📈 Findings

Six findings emerged from the analysis: four hypothesis tests examining common market assumptions, and two observational findings on price progression and return distribution.

### 1. Does higher stock price correspond to higher risk?

**Hypothesis:** Stock risk increases proportionally as price appreciates.

**Method:** Calculated 30-day rolling volatility as percentage returns across five years.

**Result:** Rejected. Average daily volatility: **1.65%**. Peak year (2022): 2.20%; lowest years (2023-2024): 1.3-1.4%. During this period, price rose from $146 to $311 (+112.8%), yet volatility showed no upward trend. Current volatility aligns with five-year mean despite 112.8% price appreciation.

A 30-day spike in early 2025 reached 4.4% (the highest in the sample), but 2025's median daily movement remains below the five-year average at 1.61%. This spike represents an isolated shock, not a shift in baseline risk.

**Key Insight:** Dollar-denominated volatility rises with price level automatically (a $300 stock moving $5/day appears higher than a $150 stock moving $2.50/day). Correcting to percentage returns eliminates the apparent trend. Occasional spikes occur but do not alter the underlying stability. A stock that doubles in price need not double in risk.

![Volatility analysis](images/volatility_analysis.png)

---

### 2. Does trading volume predict price direction?

**Hypothesis:** Volume differentials between up-days and down-days signal directional strength.

**Method:** Two-sample t-test comparing mean volume on up-days vs down-days.

**Result:** **Rejected.** Up-day volume: **63.9M shares**; down-day volume: **65.7M shares** (p = 0.267). Correlation between returns and volume: **0.01**.

**Key Insight:** Volume fluctuates independently of price direction. Counterintuitively, down-days average slightly higher volume than up-days, contradicting the trader belief that heavy volume drives prices higher. High-volume days are equally likely to close up or down.

![Volume-Price Relationship](images/volume_price.png)

---

### 3. Do seasonal patterns in returns exist?

**Hypothesis:** Certain months or weekdays exhibit superior average returns (calendar anomalies).

**Method:** Aggregated returns by month and weekday across the five-year period.

**Result:** Patterns appear in the charts (July: **+6.7%**, September: **-3.3%**), but lack statistical power. Each month has only **5 observations**; a single unusual year reverses any month's sign. Weekday effects are negligible (**-0.09% to +0.16%**). While the visualizations show variation, the small sample size makes these patterns unreliable.

**Monthly patterns:** July and November show positive returns (+6.7% and +5.3%), while September is weakest (-3.3%). However, the 10% spread from worst to best month rests on five independent observations. One strong or weak September would shift the pattern materially. Additionally, July's +6.7% may reflect a single exceptional year, not a repeatable seasonal effect.

**Weekday patterns:** The range of -0.09% (Thursday) to +0.16% (Wednesday) is negligible, roughly one-sixth of Apple's typical 1.65% daily volatility. No weekday demonstrates consistent outperformance or underperformance.

**Key Insight:** While calendar variation exists, it falls within random fluctuation. These patterns are data descriptions of what happened, not predictions of what will happen.

![Monthly Seasonality](images/monthly_seasonality.png)

![Weekly Patterns](images/weekday_effect.png)

---

### 4. Why does volume decline while price appreciates?

**Observation:** Average daily volume fell **39%** (**85M → 51M** shares) while price rose **112.8%**.

**Apparent Finding:** Correlation between Close price and Volume is **-0.40**, suggesting inverse relationship.

**Actual Finding:** Correlation between daily returns and volume is **0.01** (unrelated). Looking at the correlation matrix, Close-to-Volume shows **-0.40** (bottom-left), but daily returns-to-Volume shows only **0.01**. The -0.40 reflects opposing time trends, not a true negative relationship between price level and trading activity.

**Key Insight:** When two variables trend in opposite directions over time, aggregate correlation measures the trend artifact, not the underlying relationship. Proper analysis requires time-detrending or within-period comparisons to isolate true associations between variables.

![Correlation matrix](images/correlation_matrix.png)

---

Beyond these hypothesis tests, two additional market characteristics emerged from the analysis.

### 5. How did Apple's stock price and returns progress over the period?

**Observation:** Apple appreciated **112.8%** (**$146 to $311**) with a **16.3% CAGR**.

**Yearly progression breakdown:**

| Year | Return | Market Regime | Notes |
|---|---|---|---|
| 2021 | **+20.7%** | Post-pandemic bull | Partial year (5 months) |
| 2022 | **-28.6%** | Rate-hike selloff | Highest loss in sample |
| 2023 | **+52.9%** | Recovery begins | Strongest year; breaks negative streak |
| 2024 | **+35.8%** | Momentum continues | Steady gains |
| 2025 | **+10.7%** | Normalizing | Growth moderates |
| 2026 | **+15.3%** | Steady growth | Partial year (7 months) |

**Key Insight:** The 112.8% headline return masks significant annual volatility. An investor holding through the 2022 decline captured the full five-year gain; one exiting after 2022 would have missed the 2023-2024 recovery (+52.9% and +35.8%). Performance appears smooth only in aggregate. Year-to-year progression is lumpy and regime-dependent. The contrast between 2022's -28.6% and 2023's +52.9% shows why timing matters, yet commitment through downturns is rewarded.

![Price and Yearly Returns](images/price_and_yearly_returns.png)

---

### 6. Do returns follow the normal distribution assumed in risk models?

**Hypothesis:** Daily returns follow a normal (bell curve) distribution.

**Method:** Applied D'Agostino normality test with Q-Q plot analysis.

**Result:** **Rejected (p < 0.001).** Returns exhibit **kurtosis of 6.64**, meaning extreme daily moves occur far more frequently than a normal distribution predicts.

The Q-Q plot (top-right of the visualization) shows sharp divergence at both tails, providing clear evidence that days a normal model would classify as near-impossible happened regularly.

**Key Insight:** Standard risk models that assume normal distribution systematically underestimate extreme move frequency. With 1.65% average daily volatility, Apple experiences days outside **±3 standard deviations multiple times per year**. These are events that normal models classify as "impossible," yet they occur regularly. This has critical implications for value-at-risk calculations, option pricing, and portfolio hedging strategies built on normality assumptions.

![Return Distribution](images/return_distribution.png)

---

## 💡 Conclusions

Market insights depend critically on measurement methodology, not market behavior. Three widely-held assumptions failed empirical testing—revealing that measurement choices drive conclusions.

- **Volatility measurement:** Using dollars instead of percentages creates the false impression of rising risk. Correction reverses the conclusion.
- **Baseline selection:** Comparing across a multi-year period masks within-period relationships. Time trends dominate naive correlations.
- **Sample adequacy:** Calendar patterns observed in this five-year window may shift or stabilize with additional years of data. Current findings reflect this period only.
- **Unit consistency:** A stock's percentage-based risk remained stable despite doubling in price and halving in volume.

The project reinforces a critical principle: **correct mathematics applied to incorrectly-specified questions produces wrong answers.** Analytical rigor requires intentional measurement choices aligned with the economic question being asked.

---

## 📁 Project Structure

```
aapl-stock-analysis/
├── images/                 # Exported charts (8 visualizations)
├── Apple_Inc.ipynb         # Full analysis notebook with code and outputs
├── DATA_DICTIONARY.md      # Metric definitions, formulas, and caveats
└── README.md               # Project overview and findings
```

---

## 🧰 Tech Stack

- **Language:** Python
- **Libraries:** pandas, NumPy, SciPy, Matplotlib, Seaborn, yfinance
- **Environment:** Jupyter Notebook

---

## 🔄 Methodology

**1. Data Acquisition**
- Retrieved 1,254 daily OHLCV records; verified 0 missing values and 0 duplicates
- Used unadjusted `Close` for daily price behavior analysis

**2. Feature Engineering**
- Daily returns, intraday range, 30-day moving average, 30-day rolling volatility (% returns)
- Percentage-based metrics ensure comparability across the five-year period

**3. Hypothesis Testing**
- D'Agostino normality test on returns
- Two-sample t-test for volume (up-days vs down-days)
- Pearson correlation for relationship analysis

**4. Time-Based Analysis**
- Yearly returns, monthly seasonality (within-year and aggregated), weekday averages

---

## ⚠️ Limitations

- **No benchmark:** Returns are measured in absolute terms. No comparison to S&P 500 or NASDAQ is provided to contextualize performance relative to the broader market.
- **Dividends excluded:** Actual shareholder return exceeded 112.8% when accounting for dividend payments. Analysis focuses on price behavior only.
- **Partial years:** 2021 covers 5 months; 2026 covers 7 months. Annual returns for these years are not directly comparable to full-year periods.
- **Period-specific insights:** Analysis is based on historical data (Aug 2021 to Aug 2026). As market structures and conditions evolve, findings may not hold in future periods. Results describe observed behavior during this specific window, not necessarily predictive patterns.

*This analysis is informational only and should not be construed as investment advice.*

---

## 🧑‍💻 Author

**Thrinesh Vuribindi** — Data Analyst

[![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/thrineshvuribindi)
[![GitHub](https://img.shields.io/badge/GitHub-12100E?style=for-the-badge&logo=github&logoColor=white)](https://github.com/thrinesh13)
