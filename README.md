# Portfolio VaR Engine

## Overview

An end-to-end **Portfolio Value-at-Risk (VaR) engine** for a 5-asset U.S. equity portfolio. The project estimates one-day portfolio risk using three VaR methodologies and evaluates their performance through statistical backtesting.

## Portfolio

| Asset | Ticker | Weight |
|---|---|---:|
| Apple | AAPL | 30% |
| Microsoft | MSFT | 20% |
| Amazon | AMZN | 20% |
| JPMorgan Chase | JPM | 15% |
| Exxon Mobil | XOM | 15% |
| **Total** | | **100%** |

The portfolio value is assumed to be **$1,000,000**.

## Methodology

### 1. Data & Portfolio Returns

Historical daily prices are used to calculate daily asset returns. Portfolio returns are calculated using the fixed weights:

```python
weights = np.array([0.30, 0.20, 0.20, 0.15, 0.15])
```

Portfolio P&L is then calculated as:

\[
P\&L_t = R_{p,t} 	imes Portfolio\ Value
\]

### 2. VaR Models

Three one-day VaR approaches are implemented:

- **Historical VaR:** Uses the empirical distribution of historical portfolio returns.
- **Variance-Covariance VaR:** Uses portfolio mean, volatility, and the covariance matrix:
  \[
  \sigma_p = \sqrt{w^T\Sigma w}
  \]
- **Monte Carlo VaR:** Simulates correlated asset returns using the estimated mean vector and covariance matrix and derives VaR from the simulated P&L distribution.

VaR is calculated at **95% and 99% confidence levels** using a **250-trading-day rolling window**.

## Backtesting

Each day's VaR forecast is compared with the **realized next-day portfolio P&L**. A VaR exception occurs when the realized loss exceeds the predicted VaR.

The models are evaluated using:

- **Kupiec Proportion of Failures (POF) Test** — checks whether the observed exception frequency is consistent with the expected rate.
- **Christoffersen Independence Test** — checks whether VaR exceptions occur independently or cluster over time.

## Sensitivity Analysis

The project tests the stability of VaR estimates by varying:

- Lookback windows (e.g., 125, 250, and 500 trading days)
- Portfolio weights
- Confidence levels (95% and 99%)

## Key Outputs

- Daily Historical, Variance-Covariance, and Monte Carlo VaR
- Realized portfolio P&L
- VaR exception indicators
- Kupiec and Christoffersen backtesting results
- Covariance and correlation matrices
- Sensitivity analysis
- VaR vs. realized-loss visualizations

## Project Structure

```text
portfolio-var-engine/
├── data/
│   ├── portfolio_prices.csv
│   └── portfolio_returns.csv
├── src/
│   ├── data.py
│   ├── portfolio.py
│   ├── var_models.py
│   ├── backtesting.py
│   └── sensitivity.py
├── output/
│   ├── var_backtest_results.csv
│   ├── backtest_summary.csv
│   └── sensitivity_analysis.csv
├── notebooks/
│   └── portfolio_var_analysis.ipynb
├── README.md
└── requirements.txt
```

## Technologies

**Python · NumPy · Pandas · SciPy · Matplotlib · Seaborn · yfinance**

## Key Assumptions

- One-day holding period
- $1M portfolio
- 250-day rolling estimation window
- Fixed portfolio weights
- Daily returns
- No transaction costs, leverage, or short positions
