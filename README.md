# 📈 Apple Stock Analysis (AAPL)

Statistical analysis of Apple Inc. stock examining three common market assumptions across five years of daily trading data.

**August 2021 to August 2026. 1,254 trading days**

---

## ⚡ Executive Summary

| Hypothesis | Finding | Key Insight |
|---|---|---|
| Price appreciation correlates with increased volatility | ❌ Rejected | Percentage volatility remains flat; dollar-denominated measures conflate price level with risk |
| Trading volume predicts price direction | ❌ Rejected | Volume on up-days and down-days is statistically indistinguishable (p = 0.267) |
| Calendar effects exist (monthly/weekly patterns) | ⚠️ Insufficient evidence | Observed patterns rest on 5 observations per month; requires extended observation to validate |
| **Bonus finding:** Volume decline with price appreciation | ✓ Confirmed | 39% volume reduction despite 112.8% price increase; driven by time trends, not market mechanics |

---

## 📌 Context

Apple is among the most heavily traded stocks, providing consistent daily volume and sufficient observations to detect real market patterns. Over the study period, AAPL moved from $146 to $311 (+112.8% return, 16.3% CAGR) across three distinct market regimes: post-pandemic bull run (2021), rate-hike selloff (2022), and recovery (2023-2026). This variation eliminates confounding by a single market condition.

---

## 📊 Data

| Attribute | Value |
|---|---|
| **Source** | Yahoo Finance (`yfinance`) |
| **Period** | August 2021 to August 2026 |
| **Observations** | 1,254 trading days |
| **Fields** | Open, High, Low, Close, Adj Close, Volume |

Derived measures: daily returns, intraday range, 30-day rolling volatility (% returns), and 30-day moving average for trend analysis. Full documentation available in [Data Dictionary](DATA_DICTIONARY.md).

---

## 📈 Findings

### 1. Does higher stock price correspond to higher risk?

**Hypothesis:** Stock risk increases proportionally as price appreciates.

**Method:** Calculated 30-day rolling volatility as percentage returns across five years.

**Result:** Rejected. Average daily volatility: 1.65%. Peak year (2022): 2.20%; lowest years (2023-2024): 1.3-1.4%. Current volatility aligns with five-year mean despite 112.8% price appreciation. A 30-day spike in early 2025 reached 4.4% (the highest in the sample), but 2025's median daily movement remains below the five-year average at 1.61%. This spike represents an isolated shock, not a shift in baseline risk.

**Key Insight:** Dollar-denominated volatility rises with price level automatically (a $300 stock moving $5/day appears higher than a $150 stock moving $2.50/day). Correcting to percentage returns eliminates the apparent trend. Occasional spikes occur but do not alter the underlying stability. A stock that doubles in price need not double in risk.

![Volatility analysis](images/volatility_analysis.png)

---

### 2. Does trading volume predict price direction?

**Hypothesis:** Volume differentials between up-days and down-days signal directional strength.

**Method:** Two-sample t-test comparing mean volume on up-days vs down-days.

**Result:** Rejected. Up-day volume: 63.9M shares; down-day volume: 65.7M shares (p = 0.267). Correlation between returns and volume: 0.01.

**Key Insight:** Volume fluctuates independently of price direction. Down-days actually average slightly higher volume than up-days, contradicting the common trader belief that heavy volume drives prices higher. Any observed difference occurs within normal statistical variation. High-volume days are equally likely to close up or down.

![Volume-Price Relationship](images/volume_price.png)

---

### 3. Do seasonal patterns in returns exist?

**Hypothesis:** Certain months or weekdays exhibit superior average returns (calendar anomalies).

**Method:** Aggregated returns by month and weekday across the five-year period.

**Result:** Patterns appear (July: +6.7%, September: -3.3%) but lack statistical power. Each month has only 5 observations; a single unusual year reverses any month's sign. Weekday effects are negligible (-0.09% to +0.16%).

**Monthly patterns:** July and November show positive returns (+6.7% and +5.3%), while September is weakest (-3.3%). However, the 10% spread from worst to best month rests on five independent observations. One strong or weak September would shift the pattern materially. Additionally, July's +6.7% may reflect a single exceptional year, not a repeatable seasonal effect.

**Weekday patterns:** The range of -0.09% (Thursday) to +0.16% (Wednesday) is negligible—roughly one-sixth of Apple's typical 1.65% daily volatility. No weekday demonstrates consistent outperformance or underperformance.

**Key Insight:** While calendar variation exists, it falls within random fluctuation. These patterns are data descriptions of what happened, not predictions of what will happen.

![Monthly Seasonality](images/monthly_seasonality.png)

![Weekly Patterns](images/weekday_effect.png)

---

### 4. Why does volume decline while price appreciates?

**Observation:** Average daily volume fell 39% (85M → 51M shares) while price rose 112.8%.

**Apparent Finding:** Correlation between Close price and Volume is -0.40, suggesting inverse relationship.

**Actual Finding:** Correlation between daily returns and volume is 0.01 (unrelated). The -0.40 correlation reflects opposing time trends, not a true negative relationship between price level and trading activity.

**Key Insight:** When two variables trend in opposite directions over time, aggregate correlation measures the trend artifact, not the underlying relationship. Proper analysis requires time-detrending or within-period comparisons to isolate true associations.

![Correlation matrix](images/correlation_matrix.png)

---

## 💡 Conclusions

This analysis demonstrates that market insights depend critically on measurement methodology. Three widely-held assumptions fail empirical testing. The reason: measurement choices, not market behavior.

- **Volatility measurement:** Using dollars instead of percentages creates the false impression of rising risk. Correction reverses the conclusion.
- **Baseline selection:** Comparing across a multi-year period masks within-period relationships. Time trends dominate naive correlations.
- **Sample adequacy:** Calendar patterns observed in this five-year window may shift or stabilize with additional years of data. Current findings reflect this period only.
- **Unit consistency:** A stock's percentage-based risk remained stable despite doubling in price and halving in volume.

The project reinforces a critical principle: correct mathematics applied to incorrectly-specified questions produces wrong answers. Analytical rigor requires intentional measurement choices aligned with the economic question being asked.

---

## 📁 Project Structure

```
aapl-stock-analysis/
├── Apple_Inc.ipynb         # Full analysis notebook with code and visualizations
├── DATA_DICTIONARY.md      # Metric definitions, formulas, and caveats
├── images/                 # Exported charts
└── README.md
```

---

## 🧰 Tech Stack

- **Language:** Python
- **Libraries:** pandas, NumPy, SciPy, Matplotlib, Seaborn, yfinance
- **Environment:** Jupyter Notebook

---

## 🔄 Methodology

1. **Data Acquisition:** Retrieved 1,254 daily OHLCV records; verified 0 missing values and 0 duplicates. Unadjusted `Close` used for analysis of daily price behavior.

2. **Feature Engineering:** Derived daily returns, intraday range, 30-day moving average, and 30-day rolling volatility on percentage returns (ensuring comparability across the five-year period).

3. **Hypothesis Testing:** Applied D'Agostino normality test on returns; two-sample t-test for volume comparison; Pearson correlation for relationship analysis.

4. **Time-Based Analysis:** Computed yearly returns, monthly seasonality (within-year and aggregated), and average returns by weekday.

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
