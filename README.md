
# Comparing Classical and Deep Learning Models on the S&P 500

Most people in finance want to know one thing: *can we predict the market?*  
This project didn’t try to beat Wall Street — instead, it set out to **evaluate** how different time-series models behave when applied to nearly 20 years of S&P 500 weekly data.

The journey began with a simple curiosity:  
- How far can classical models like **SARIMA** take us?  
- Do modern deep learning methods (**LSTM, GRU, Bi-LSTM**) actually add value here?  
- And if neither shines alone, could a **hybrid approach** do better?  

Over the course of 11 notebooks and 1,000+ code cells, the project moved step by step:  
from building baselines, to engineering exogenous features, to training tuned neural networks, and finally combining both worlds into a **hybrid SARIMAX–LSTM model**.

---

## What was done
- Collected weekly S&P 500 index data (2004–2023) using Yahoo Finance.  
- Built classical baselines (**SARIMA, SARIMAX**) with rolling averages, volatility, and lags.  
- Trained recurrent networks (**LSTM, GRU, Bi-LSTM**) with multiple sequence lengths and scalers.  
- Used tuning frameworks (**Optuna, Keras Tuner, Random Search**) and time-series cross-validation.  
- Evaluated each model fairly with RMSE, MAE, and directional accuracy.  
- Combined SARIMAX and LSTM into a **residual-based hybrid**, asking: *can the hybrid capture what the others miss?*

---

## What was discovered
- **SARIMAX outperformed SARIMA**, thanks to exogenous features (≈16% error reduction).  
- **Deep learning didn’t clearly beat the classical models** — their errors hovered around the same level as SARIMA.  
- **The hybrid model achieved the lowest errors overall**, showing that statistical structure + neural flexibility can complement each other.  
- **Directional accuracy was misleading** (inflated by trends). Measuring accuracy on returns was more realistic.  

---

## Why it matters
This project wasn’t about producing a “magic forecast.”  
It was about **learning how different modelling families perform, how to tune them responsibly, and how to communicate results clearly**.  

For a data scientist, this demonstrates:  
- Comfort with both **classical statistics** and **deep learning**.  
- Ability to design experiments, **tune hyperparameters**, and avoid leakage.  
- Storytelling with results — including a Tableau dashboard to let others explore the findings interactively.  
