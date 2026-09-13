# Portfolio Optimization & Risk Analysis (NSE, India)

A Modern Portfolio Theory (MPT) project that builds, constrains, and backtests optimized equity portfolios from a 24-stock NSE universe across 6 sectors, then honestly evaluates whether optimization actually beats naive diversification out-of-sample.

## Overview

This project applies Markowitz mean-variance optimization to Indian equities, then stress-tests the results against reality: a train/test split, transaction-cost-aware backtesting, and benchmarking against an equal-weight portfolio and the Nifty 50 index. The goal isn't just to produce an "optimal" portfolio, but to measure how well in-sample optimization actually holds up out-of-sample.

## Data

- **Universe:** 24 NSE-listed stocks across 6 sectors — IT, Banking, FMCG, Auto, Pharma, Energy
- **Period:** ~10 years of daily prices (2016–2026)
- **Source:** Yahoo Finance (`yfinance`), adjusted close prices (dividend- and split-adjusted)
- **Split:** ~70% train (2016–2023) / ~30% test (2023–2026), split chronologically to avoid look-ahead bias

## Methodology

1. **Data collection & cleaning** — download prices, check for missing data, drop stocks with excessive gaps
2. **Descriptive statistics** — annualized return, volatility, and Sharpe per stock; sector-level aggregation
3. **Train/test split** — all optimization inputs (expected returns, covariance matrix) are computed from the training period only
4. **Portfolio optimization** (SLSQP, long-only, fully invested):
   - Minimum Variance Portfolio (MVP)
   - Maximum Sharpe Portfolio (MSP), unconstrained
   - Maximum Sharpe Portfolio, constrained to a 15% max weight per stock (diversification cap)
5. **Risk analysis** — Value at Risk (VaR), Conditional VaR / Expected Shortfall, Sharpe, Sortino, Calmar ratios, max drawdown, skewness, and excess kurtosis, computed on **out-of-sample test data**
6. **Backtesting** — monthly-rebalanced portfolios with transaction costs (10 bps per unit of turnover), vs. buy-and-hold
7. **Benchmarking** — all strategies compared against an equal-weight (1/N) portfolio and the Nifty 50 index over the same test period
8. **Consolidated summary** — expected (in-sample) vs. historical vs. backtested performance, side by side

## Key Finding

The optimized portfolios (MVP, MSP) **underperformed the naive equal-weight benchmark and the Nifty 50 out-of-sample**, despite having much stronger *expected* Sharpe ratios in-sample. This reflects a well-documented issue in portfolio theory: mean-variance optimization is highly sensitive to estimation error in expected returns, and can overfit to noise in the training window rather than capturing a persistent signal. This project treats that result as a finding, not a failure — the honest comparison against naive diversification is the point.

## Repository Structure

```
├── portfolio_optimization_risk_analysis.ipynb   # Main notebook (all steps)
├── reports/
│   └── figures/                                 # Generated plots
├── portfolio_weights.csv                        # MVP / MSP / constrained MSP weights
├── risk_metrics.csv                              # Out-of-sample risk metrics per strategy
├── backtest_results.csv                          # Backtest performance with transaction costs
├── final_summary.csv                             # Expected vs. historical vs. backtested comparison
└── README.md
```

## Requirements

```
python >= 3.9
yfinance
pandas
numpy
matplotlib
seaborn
scipy
```

Install with:
```bash
pip install yfinance pandas numpy matplotlib seaborn scipy
```

## Usage

Run the notebook top to bottom. It will download fresh price data via `yfinance`, so results (especially the test-period date range) will shift slightly depending on when it's run.

## Limitations & Next Steps

- **Single train/test split** — results reflect one specific 3-year out-of-sample window; a walk-forward (rolling re-optimization) backtest would give a more robust read on consistency.
- **Sample-mean expected returns** — historical mean daily return is a noisy estimator; shrinkage approaches (e.g., Ledoit-Wolf covariance shrinkage, or Black-Litterman for expected returns) are a natural extension.
- **Static risk-free rate** — a flat 6.5% is used throughout; a time-varying rate (e.g., India 10Y G-Sec yield history) would be more precise.
- **No significance testing** — Sharpe ratio differences between strategies are not tested for statistical significance given the limited test-period sample size.

## License

MIT (or update as appropriate)
