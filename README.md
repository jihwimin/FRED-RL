# FRED Reinforcement Learning Portfolio Optimization

An AI-driven portfolio optimization system that uses **macroeconomic indicators from FRED** and **reinforcement learning** to dynamically allocate assets across market regimes.

This project investigates whether reinforcement learning agents can **adapt portfolio allocation in response to changing economic conditions** and outperform a traditional benchmark (S&P 500) in a low-frequency financial setting.

---

## Project Overview

Traditional portfolio strategies often rely on static assumptions and are slow to adapt to macroeconomic shifts. Human decision-making is also prone to emotional bias.

In this project, we build a **custom reinforcement learning environment** that:
- Ingests real-world macroeconomic data from FRED
- Learns optimal asset allocation strategies through trial and error
- Maximizes **risk-adjusted returns** using a Sharpe-ratio–based reward function

---

## Assets & Data

### Asset Universe (ETFs)
- **SPY** — U.S. large-cap equities  
- **QQQ** — NASDAQ (tech-heavy equities)  
- **TLT** — Long-term U.S. Treasury bonds  
- **GLD** — Gold  
- **VNQ** — Real estate (REITs)  

ETFs were selected to ensure diversification while minimizing idiosyncratic risk.

### Macroeconomic Indicators (FRED)
- Federal Funds Rate *(Leading)*
- VIX – Market Volatility Index *(Leading)*
- GDP Growth Rate *(Coincident)*
- Unemployment Rate *(Lagging)*
- Inflation Rate *(Lagging)*

Indicators are resampled monthly and normalized (z-score) to stabilize learning.

---

## Reinforcement Learning Setup

### State Space
- Vector of normalized macroeconomic indicators

### Action Space
- Continuous allocation weights for each asset
- Softmax constraint ensures weights sum to 1

### Reward Function
- Sharpe Ratio–based reward
- Benchmark-relative bonus for outperforming SPY

### Train / Test Split
- **Training:** 2010–2020  
- **Testing:** 2021–2024  

---

## Models Implemented

| Model | Strengths | Weaknesses |
|------|----------|-----------|
| A2C | Simple baseline | Least stable |
| PPO | Stable updates | Sample inefficient |
| SAC | Strong exploration | Sensitive to tuning |
| **TD3** | Sample efficient, robust | Higher complexity |

After extensive experimentation and hyperparameter tuning, **TD3 emerged as the best-performing model**.

---

## Key Experiments & Findings

### Benchmark Performance Challenge
Early models struggled to consistently outperform the **S&P 500 (SPY)**.

We explored:
- Reward engineering (Sharpe → hybrid → benchmark-relative)
- Adding additional macroeconomic indicators
- Extending historical data
- Testing alternative reinforcement learning algorithms

Reward design and expanded indicators had the greatest impact on performance.

---

### Data Extension Experiment (1971–Present)
We extended the dataset back to **1971**, the earliest point with reliable macroeconomic data.

However:
- GLD and VNQ did not exist before 2000
- Asset universe reduced from 5 to 3
- Reduced diversification limited defensive rotations
- Overall performance degraded

Final models reverted to **2000–present data** to preserve strategic flexibility.

---

## Results

- **TD3 outperformed the S&P 500 benchmark**
- Strong sample efficiency with limited monthly data (~200 observations)
- Learned dynamic allocation patterns aligned with macroeconomic regimes
- Performed well in volatile and regime-shifting markets

---

## Limitations

- Financial markets are **non-stationary**
- Reinforcement learning models are **black-box**
- Simulation cannot fully capture real-world trading dynamics
- Transaction costs and leverage are not modeled

---

## Tech Stack

- Python
- NumPy, Pandas
- Stable-Baselines3
- OpenAI Gym (custom environment)
- FRED API
- Yahoo Finance API
- Matplotlib / Seaborn

---

## Future Work

- Improve interpretability of learned policies
- Incorporate transaction costs and slippage
- Explore regime-aware or hierarchical RL approaches
- Extend to higher-frequency data
- Add explainability tools for financial decisions

---

## Contributors

- **Jihwi Min**
- **Minsuk Choi**

---

## References

- Federal Reserve Economic Data (FRED)
- Yahoo Finance
- Stable-Baselines3 documentation
