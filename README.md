# 🚆 Estimating Price Elasticity of Demand for Rail Tickets

**Econometric analysis of 209,697 rail booking records to measure how ticket price affects demand, controlling for booking lead time, product, customer and seasonal effects.**

`Python` · `Econometrics` · `OLS Regression` · `Fixed Effects` · `Robust Inference` · `Pricing Analytics`

> NUS Master of Science in Business Analytics, DBA5101 Analytics in Managerial Economics (group project, 5 members)
> 📄 **[Read the full report (PDF)](Train%20Demand.pdf)**

---

## 📌 Business Question

How sensitive is rail ticket demand to price? And how much does the answer change once you account for *when* people book? Rail operators use dynamic, revenue-managed pricing, so a naive price–quantity comparison is biased. This project builds a defensible elasticity estimate from messy observational booking data.

## 🔑 Key Results

| Finding | Result |
|---|---|
| **Price elasticity (main model)** | **−0.253**: a 1% price increase is associated with ~0.25% fewer seats sold (p < 0.001) |
| Bias from ignoring booking timing | Omitting lead-time controls understates elasticity (−0.207 vs −0.253) |
| Customer segmentation | Category B is price-sensitive (**−0.296**); Category A shows a positive association (+0.165), consistent with dynamic pricing |
| Robustness | Negative elasticity holds across $10–$100 price bins (−0.289 to −0.326), clustered SEs, and outlier exclusion |
| Sample-design fix | Cut the max/min monthly mean-fare ratio from **4.18 → 1.36**, and SD of monthly mean lead time from **112.4 → 3.9 days** |

**Takeaway:** Demand is price-inelastic overall, but elasticity varies sharply by customer segment and booking stage. Pricing strategy should be segment- and timing-aware rather than uniform.

## 👩‍💻 My Contributions
- **Exploratory data analysis:** led the EDA and produced all of the report's visualisations (price/volume distributions, lead-time curves, and heatmaps by train, cabin class and customer category).
- **120-day analysis sample:** came up with the idea of a fixed booking window to remove the truncation bias in the raw data (see Approach, step 2).
- **Lead-time categories:** defined the 0–2 / 3–7 / 8–14 / 15–30 / >30-day booking stages from structural breaks in the data.
- **Baseline model:** built Model 0, the fixed-effects OLS benchmark without lead-time controls.
- **Report integration:** combined the team's analyses and outputs into the final report.

## 🛠️ Approach

### 1. Exploratory Data Analysis
- Profiled 209,697 sales records across 15 anonymised train services (purchases Jun 2018 – Jun 2019, departures to Jun 2020).
- Found heavily right-skewed price and volume, so applied a **log-log transformation** to make the price coefficient read directly as an elasticity.
- Spotted **structural breaks at 30, 14, 7 and 2 days** before departure in both price and volume, evidence of revenue-management fare rules. Checked these cutoffs against peer-reviewed transport-economics literature.
- Documented heterogeneity across trains, cabin class, customer category, departure month and day of week.

### 2. Fixing a Hidden Selection Bias
The purchase window is shorter than the departure window, so different departures are **truncated in opposite directions**. Early departures show only expensive late bookings, and late departures show only cheap early ones. Pooling them creates a spurious price–demand pattern.

- Designed a sample rule (departures 29 Sep 2018 – 30 Jun 2019, lead time ≤ 120 days) so **every departure is observed over the same booking window**.
- Picked the 120-day window with a data-driven trade-off analysis: records retained vs. share of the booking curve covered. This kept **123,991 records** while covering **82.7%** of the booking curve.

### 3. Econometric Modelling
Log-log OLS with fixed effects:

```
ln(Q) = α + β·ln(P) + γ'X + δ_LeadTime + ε
```

- **Controls (X):** train, cabin type, direction, return/one-way, customer category, departure day-of-week, departure year-month.
- **Lead-time fixed effects:** 0–2, 3–7, 8–14, 15–30, >30 days.
- **Inference:** a Breusch–Pagan test showed heteroskedasticity (p < 0.001), so the models use **HC3 robust standard errors**, plus a train-clustered SE check.

### 4. Robustness & Sensitivity Testing
- **Segment models** split by customer category, with and without lead-time controls.
- **High-leverage check:** dropped a single-record train. The estimate did not change.
- **Price-binning sensitivity** at $10 / $20 / $50 / $100 aggregation.
- **Limitations discussed openly:** price endogeneity and simultaneity from dynamic pricing mean the estimates are conditional associations, not causal effects.

## 💡 Skills Demonstrated
- **Causal-thinking & econometrics:** omitted-variable bias, selection/truncation bias, fixed effects, heteroskedasticity-robust inference, endogeneity
- **Data analysis in Python:** EDA, lead-time segmentation and data visualisation
- **Analytical judgement:** justifying modelling choices with data trade-offs and academic literature
- **Communication:** turning statistical output into pricing insights for business readers

## 📂 Repository Contents
| File | Description |
|---|---|
| [`Train Demand.pdf`](Train%20Demand.pdf) | Full project report: methodology, results, figures and regression output |

## 📚 References
1. Yuan, Y., Ran, W., & Zhang, S. (2026). *Analysis of advance purchase behavior of air–rail passengers with ticket booking data.* Applied Sciences, 16(13), 6300. https://doi.org/10.3390/app16136300
2. Hotle, S., Castillo, M., Garrow, L., & Higgins, M. (2015). *The impact of advance purchase deadlines on airline consumers' search and purchase behaviors.* Transportation Research Part A, 82, 1–16. https://doi.org/10.1016/j.tra.2015.09.001
