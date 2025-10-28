# ⛽ **Forecasting the Real Price of Gasoline in the United States**

---

### **Introduction**

This notebook develops a complete econometric and statistical forecasting exercise aimed at understanding and predicting the **real (inflation-adjusted) price of gasoline in the United States**.  

Using a dataset that includes **monthly gasoline prices** and the **Consumer Price Index (CPI)**, the project proceeds through every step of a rigorous time-series forecasting workflow — from data preparation and transformation to model estimation, evaluation, and diagnostic testing.

The analysis blends **economic intuition** with **statistical modeling**, illustrating how seemingly sophisticated models often fail to outperform simple benchmarks when applied to highly volatile, market-driven variables like fuel prices.

---

### **Economic Background**

Nominal gasoline prices fluctuate heavily due to factors such as crude oil supply, geopolitical events, refining capacity, and seasonal demand.  
However, to assess true changes in purchasing power and real cost, these nominal prices must be **adjusted for inflation** using the CPI, yielding the **real price of gasoline**.

The central question is whether historical patterns in real gasoline prices contain enough structure to make accurate forecasts, or if price changes are largely random, reflecting efficient market behavior.

---

### **Analytical Framework**

The study unfolds through several major steps:

1. **Data Preparation and Inflation Adjustment**  
   - Imported the nominal gasoline price and CPI series.  
   - Converted nominal prices to **real terms**:  
     $
     \text{Real Price}_t = \frac{\text{Nominal Price}_t}{\text{CPI}_t} \times 100
     $
   - Ensured proper data types, handled decimal separators, and created time-based indexing.

2. **Exploratory Visualization**  
   - Compared nominal vs. real gasoline prices over time.  
   - Observed how inflation smoothing reveals true price dynamics, with real prices showing long-term mean reversion and shorter cyclical spikes.

3. **Logarithmic Transformation and Stationarity Analysis**  
   - Transformed real prices into logs: $ y_t = \log(P_t^{real}) $, to stabilize variance.  
   - Examined the **autocorrelation functions (ACFs)** of $ y_t $ and its first difference $ \Delta y_t $.  
   - Found strong persistence in levels and weak correlation in differences — evidence that the series is **integrated of order one (I(1))**.

4. **AR(1) Model Estimation**  
   - Fitted **AR(1)** models for both $ y_t $ and $ \Delta y_t $.  
   - The coefficient $ \phi \approx 0.98 $ for $ y_t $ confirmed near-unit-root behavior, while $ \phi \approx 0.45 $ for $ \Delta y_t $ showed short-term stationarity.  
   - Residual diagnostics confirmed well-behaved, mean-zero errors.

5. **Forecasting Models**  
   - Generated **one-step-ahead forecasts** using four competing models:  
     - Random Walk (no drift)  
     - ARIMA(1,1,0)  
     - ARIMA(0,1,1)  
     - ARIMA(1,1,1)
   - Used a **recursive (expanding window)** scheme to mimic real-time forecasting.  
   - Forecasts were produced in log scale and then exponentiated back to real prices.

6. **Forecast Evaluation**  
   - Computed **Mean Squared Forecast Error (MSFE)** to measure predictive accuracy.  
   - Visualized forecasts and residuals for each model.  
   - Found that **ARIMA(0,1,1)** yielded the lowest MSFE, but improvements over the Random Walk were minimal.

7. **Statistical Tests of Predictive Superiority**  
   - Applied **Theil’s U** statistic and the **Diebold–Mariano test** to formally compare models.  
   - All ARIMA models achieved Theil’s U < 1 (slightly better than Random Walk) but with **no statistically significant improvement** , DM p-values ≈ 0.5–0.6.  
   - Concluded that the Random Walk benchmark remains unbeatable in practice.

8. **Multi-Step Forecasting (1–12 months)**  
   - Extended analysis to 1-, 3-, 6-, and 12-month horizons.  
   - Found that ARIMA(0,1,1) performs slightly better for short horizons but quickly converges to Random Walk performance as the horizon increases.  
   - By 12 months, the Random Walk model actually outperforms all others — confirming the limits of predictability.

---

### **Key Findings**

- **Persistence and Randomness:**  
  The log of real gasoline prices follows a near-unit-root process. Shocks have persistent effects, implying that prices do not revert quickly to a mean level.

- **Stationarity in Changes, Not in Levels:**  
  The first difference of the log series (roughly percentage changes) is stationary, meaning that while the level is unpredictable, monthly percentage changes behave like white noise.

- **Model Performance:**  
  ARIMA models slightly reduce forecast error variance in the short run, but their improvement is economically and statistically negligible.

- **Forecast Horizon Dynamics:**  
  As the horizon extends, forecast errors grow, and all models converge toward the Random Walk benchmark.

- **Statistical Significance:**  
  Diebold–Mariano tests confirm no significant difference in predictive accuracy between ARIMA models and the Random Walk.

---

### **Conclusion**

This empirical investigation leads to a consistent conclusion:  
the **real price of gasoline behaves like a random walk**, with changes driven by new, unforeseeable information, geopolitical events, supply shocks, policy shifts, and global demand fluctuations.

Complex ARIMA structures offer minor refinements but no robust forecasting advantage.  
In an economic sense, the gasoline market exhibits **informational efficiency**, meaning that all available historical information is already embedded in current prices.

Thus, the best forecast for next month’s real gasoline price remains elegantly simple:

\[
\boxed{\hat{P}_{t+1}^{real} = P_t^{real}}
\]

In other words, today’s real price is the best guess for tomorrow’s — a reminder that, in the world of energy forecasting, the **humble Random Walk** continues to outshine its more elaborate statistical competitors.
