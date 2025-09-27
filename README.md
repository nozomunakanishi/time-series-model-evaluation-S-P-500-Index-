# Evaluating Classical and Deep Learning Time-Series Models on the S&P 500 Index

## Executive Summary
Financial time-series are noisy and hard to model. This project explored whether classical approaches like SARIMA can hold their ground against deep learning models such as LSTMs and GRUs, and whether a hybrid of both could achieve lower errors.

This project did not aim to beat Wall Street. Instead, it set out to **systematically evaluate** different time-series model families on the S&P 500 weekly closing prices (2004–2023). The goal was to understand *how classical models perform, how deep learning compares, and whether a hybrid approach could lower errors*.  

The findings:  
- **SARIMAX (with exogenous features) outperformed plain SARIMA** by ~16%.  
- **Deep learning models (LSTM, GRU, Bi-LSTM)** performed similarly to SARIMA — no clear gain.  
- **A hybrid SARIMAX–LSTM delivered the lowest errors overall** (Test RMSE ≈ 0.0177, MAE ≈ 0.0134).  

This project demonstrates the ability to design experiments, evaluate models fairly, and communicate insights in a research-style narrative.  

---

## 1. Introduction
The study was motivated by a simple question:  
*How do classical statistical models compare with modern neural networks when applied to financial time series, and can a hybrid approach combine the best of both worlds?*  

To answer this, a workflow of 11 notebooks (≈1,000+ executed cells) was developed, progressing from baseline models to deep learning and finally to a hybrid SARIMAX–LSTM design.  

---

## 2. Data
- **Source:** Yahoo Finance (`^GSPC`, S&P 500 Index).  
- **Frequency:** Weekly closes.  
- **Period:** 2004-01-01 → 2023-12-31.  
- **Target:** Closing price (transformed via logs/differences).  

**Feature Engineering:**
- Rolling averages: 4-week, 12-week.  
- Rolling volatility: 4-week.  
- Lags: 1-week, 4-week.  

**Train/Test Splits:**
- 85/15 and 90/10 (time-ordered, no leakage).  

**Scaling (for neural & hybrid models):**
- StandardScaler, MinMaxScaler, RobustScaler.  

**Neural Input Sequence Lengths:** 4, 12, 26, 52 weeks.  

---

## 3. Methodology
1. **Baseline (SARIMA):**  
   Established with `auto_arima` and grid search. ACF/PACF plots and residual diagnostics confirmed adequacy.  

2. **SARIMAX (with exogenous features):**  
   Extended SARIMA by incorporating rolling averages, volatility, and lags.  

3. **Deep Learning Models (LSTM, GRU, Bi-LSTM):**  
   - Multiple sequence lengths and scalers tested.  
   - Hyperparameter tuning with **Optuna**, **Keras Tuner**, and random search.  
   - Cross-validation with **TimeSeriesSplit**; early stopping applied.  

4. **Hybrid (SARIMAX + LSTM):**  
   - Stage 1: Fit SARIMAX to capture structure.  
   - Stage 2: Train LSTM on residuals.  
   - Final output = SARIMAX forecast + LSTM residual prediction.  

---

## 4. Results

### Comparative Performance (best from each family)

| Model Family | Best Test RMSE | Best Test MAE | Data Split | Notes |
|--------------|----------------|---------------|------------|-------|
| SARIMA       | 0.0226         | 0.0171        | 85/15, 52w | Random search |
| SARIMAX      | 0.0189         | 0.0148        | 85/15, 12w | Auto-ARIMA |
| LSTM         | 0.0224         | 0.0170        | 85/15, 4w  | Random search (2 layers) |
| GRU          | 0.0225         | 0.0172        | 85/15, 4w  | Random search (2 layers) |
| Bi-LSTM      | 0.0225         | 0.0173        | 85/15, 4w  | Random search (3 layers) |
| Hybrid       | **0.0177**     | **0.0134**    | 85/15, 12w | Robust scaler, Keras Tuner |

### Key Insights
- **Exogenous signals help:** SARIMAX consistently outperformed SARIMA.  
- **Deep networks did not clearly beat classical models** in this dataset.  
- **Hybrid SARIMAX–LSTM produced the lowest errors**, showing complementarity.  
- **Directional accuracy on levels was misleading** (>95%, inflated by trends). Accuracy on returns is more realistic.  

---
