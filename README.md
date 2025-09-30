# 📈 Evaluating Classical and Deep Learning Time-Series Models on the S&P 500 Index

## 📝 Executive Summary
Financial markets are notoriously complex: they exhibit noise, non-stationarity, and abrupt structural changes. Analysts and data scientists often debate whether traditional statistical models are sufficient or whether deep learning architectures can capture hidden, nonlinear signals.  

This project set out not to “beat Wall Street,” but to **systematically evaluate** different model families on the weekly closing prices of the S&P 500 index (2004–2023). The guiding question was simple: *Can classical approaches like SARIMA still hold their ground against modern neural networks, and could a hybrid design combine the strengths of both?*  

Over the course of 11 executed notebooks (≈1,000+ cells), a pipeline was developed that progressed from baseline SARIMA models, through feature-augmented SARIMAX models, to deep recurrent neural networks (LSTM, GRU, Bi-LSTM), and finally to a hybrid SARIMAX–LSTM approach.  

**Key findings:**  
- **SARIMAX with exogenous features** (rolling means, volatility, lags) outperformed plain SARIMA by ~16%.  
- **Deep learning models (LSTM, GRU, Bi-LSTM)** performed comparably to SARIMA, offering no consistent improvement.  
- **A hybrid SARIMAX–LSTM** achieved the lowest errors overall (Test RMSE ≈ 0.0177, MAE ≈ 0.0134).  

Beyond the numbers, this project demonstrates the ability to **design experiments rigorously, compare models fairly, and communicate insights in a research-oriented narrative**.

---

## 1. 🎯 Introduction
The study was motivated by a central research question:  
**How do classical statistical models compare with modern neural networks when applied to financial time series, and can a hybrid approach combine the best of both worlds?**

To address this, the workflow was structured around comparative evaluation rather than raw forecasting. Each model family was tested systematically under controlled conditions, with results measured on hold-out test sets.

---

## 2. 📊 Data
- **Source:** Yahoo Finance (`^GSPC`, S&P 500 Index)  
- **Frequency:** Weekly closing prices  
- **Period:** 2004-01-01 → 2023-12-31  
- **Target:** Closing price (transformed using logs and first differences)  

**Feature Engineering (for SARIMAX & Hybrid):**  
- Rolling averages: 4-week, 12-week  
- Rolling volatility: 4-week standard deviation  
- Lagged returns: 1-week, 4-week  

**Experimental Design:**  
- Train/Test Splits: 85/15 and 90/10 (time-ordered, no leakage)  
- Scalers for neural & hybrid models: StandardScaler, MinMaxScaler, RobustScaler  
- Neural sequence lengths: 4, 12, 26, 52 weeks  

---

## 3. 🧠 Methodology
**Baseline (SARIMA):**  
- Established with stepwise auto-ARIMA and grid search.  
- ACF/PACF analysis guided parameter selection.  
- Residual diagnostics checked assumptions of white noise.  

**SARIMAX (with exogenous features):**  
- Extended SARIMA by adding rolling averages, volatility, and lags.  
- Compared performance vs. SARIMA under the same splits.  

**Deep Learning Models (LSTM, GRU, Bi-LSTM):**  
- Tested multiple sequence lengths and scalers.  
- Hyperparameter tuning via **Optuna**, **Keras Tuner**, and random search.  
- TimeSeriesSplit cross-validation with early stopping to prevent overfitting.  

**Hybrid (SARIMAX + LSTM):**  
- Stage 1: Fit SARIMAX to capture linear/seasonal structure.  
- Stage 2: Train LSTM on SARIMAX residuals.  
- Final output = SARIMAX predictions + LSTM residual forecasts.  

---

## 4. 📈 Results

**Comparative Performance (best per family):**

| Model Family | Best Test RMSE | Best Test MAE | Data Split | Notes |
|--------------|----------------|---------------|------------|-------|
| SARIMA       | 0.0226         | 0.0171        | 85/15, 52w | Random search |
| SARIMAX      | 0.0189         | 0.0148        | 85/15, 12w | Auto-ARIMA |
| LSTM         | 0.0224         | 0.0170        | 85/15, 4w  | Random search (2 layers) |
| GRU          | 0.0225         | 0.0172        | 85/15, 4w  | Random search (2 layers) |
| Bi-LSTM      | 0.0225         | 0.0173        | 85/15, 4w  | Random search (3 layers) |
| Hybrid       | **0.0177**     | **0.0134**    | 85/15, 12w | Robust scaler, Keras Tuner |

**Key Insights:**  
- Exogenous features consistently improved SARIMA → SARIMAX.  
- Deep networks matched SARIMA but did not surpass it in this dataset.  
- The hybrid SARIMAX–LSTM achieved the lowest overall errors, demonstrating complementarity between linear statistical structure and nonlinear residual learning.  
- Directional accuracy on raw levels was misleading (>95%), inflated by trend persistence. Directional accuracy on returns gave a more realistic signal.  

---

## 5. ⚖️ Limitations and Caveats
- **Directional accuracy inflation:** When measured on raw levels, DA exceeded 95%, but this was driven by the long-term upward trend of the index. Evaluating DA on returns provided a truer picture.  
- **Residual noise:** Even the hybrid left residuals that showed autocorrelation, indicating remaining unexplained structure.  
- **Neural instability:** RNN results varied significantly depending on sequence length and scaling, suggesting sensitivity to design choices and the limited size of weekly data (~1,000 samples).  
- **Data coverage:** The dataset was limited to 2004–2023 weekly closes. Daily or intraday data may reveal different dynamics, where deep learning could gain more traction.  
- **Comparability:** SARIMAX “seasonality” parameters and RNN sequence lengths are not strictly equivalent, though aligned for evaluation.  

---

## 6. 🏁 Conclusions
The evaluation showed that **classical models with exogenous features remain highly competitive**, while deep recurrent networks offered no clear benefit on weekly S&P 500 data. However, a **hybrid SARIMAX–LSTM** achieved the lowest errors, validating the idea that combining linear and nonlinear modelling can reduce residual variance.  

While not designed to produce deployable forecasts, the project highlights **experiment design, reproducibility, critical evaluation, and storytelling — essential skills for data science in business contexts**.  

---

## 7. 🔑 Outcomes
This repository demonstrates:  
- **Breadth:** Application of both statistical and neural methods.  
- **Depth:** Hyperparameter tuning with Optuna and Keras Tuner; feature engineering; residual diagnostics.  
- **Rigor:** Time-ordered splits, no leakage, cross-validation, reproducibility.  
- **Communication:** A clear research narrative suitable for portfolio presentation.  

Together, these skills translate directly to industry problems such as **forecasting, risk modelling, and demand planning**.
