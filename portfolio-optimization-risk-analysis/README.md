# Portfolio Optimization & Risk Analysis — Indian Equities

Constructs and backtests optimized investment portfolios using 10 years of daily price
data across 24 NSE-listed stocks spanning 6 sectors (IT, Banking, FMCG, Auto, Pharma, Energy),
applying Modern Portfolio Theory, tail-risk analysis, and realistic transaction-cost-aware
backtesting.

## Objective

Evaluate risk-return tradeoffs across optimized portfolio strategies — Minimum Variance
Portfolio (MVP) and Maximum Sharpe Portfolio (MSP) — and validate their robustness against
downside risk, tail behavior, and realistic trading frictions.

## Methodology

1. **Data**: 10 years of daily adjusted close prices for 24 NSE stocks (`yfinance`), cleaned
   and forward-filled for missing sessions.
2. **Optimization**: Mean-variance optimization (`scipy.optimize`, SLSQP) for both an
   unconstrained long-only MSP and a diversification-constrained MSP (max 15% per stock).
3. **Risk Analysis**: Maximum Drawdown, historical Value-at-Risk (VaR) and Conditional VaR
   (CVaR) at 95%/99%, skewness, and excess kurtosis to characterize tail behavior.
4. **Backtesting**: Monthly rebalancing with a 0.1% transaction-cost drag per unit of
   turnover, compared against a static buy-and-hold benchmark.

## Key Results

| Strategy | Expected Return | Historical Sharpe | Max Drawdown | Backtested Return (net of costs) | Backtested Sharpe |
|---|---|---|---|---|---|
| MVP | 13.27% | 0.52 | −26.09% | 13.06% | 0.50 |
| MSP (Unconstrained) | 20.13% | 0.87 | −34.78% | 19.94% | 0.86 |
| MSP (Constrained, 15% cap) | 19.91% | 0.86 | −35.37% | 19.68% | 0.85 |

Risk-free rate assumption: 6.5% (approx. India 10Y G-Sec yield).

**Rebalancing vs. buy-and-hold**: the monthly-rebalanced constrained MSP grew ₹100 to ₹611
over 10 years vs. ₹509 for a static buy-and-hold allocation — rebalancing added meaningful
value even after transaction costs.

**Tail risk**: excess kurtosis of 17–20 across all portfolios reflects fat tails driven
largely by the March 2020 COVID crash, consistent with known equity market stress periods.

## Repository Structure