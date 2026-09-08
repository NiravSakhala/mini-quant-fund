# Mini Quant Fund

## Overview

This project develops and backtests a systematic multi-factor equity strategy using 30 liquid large-cap Indian stocks.

The model combines five factors:

- Momentum
- Low Risk
- Quality
- Value
- Size

Each stock is ranked monthly using percentile-based factor scores. The five factors are equally weighted, and the Top 10 stocks are selected each month.

Three portfolio-construction methods are compared:

- Equal Weight
- Factor-Score Weight
- Sharpe-Optimized Weight

The strategy is evaluated against the NIFTY 50 benchmark using historical data from 2020 to 2024.

---

## Research Question

**Can a systematic multi-factor strategy produce stronger risk-adjusted performance than the NIFTY 50, and does that performance remain robust across different portfolio-construction methods and market periods?**

---

## Methodology

### Stock Universe

The model uses a fixed universe of 30 liquid large-cap Indian equities across sectors such as banking, information technology, energy, pharmaceuticals, consumer goods and industrials.

### Factors

**Momentum**  
Trailing 12-month price performance. Higher is better.

**Low Risk**  
Trailing 12-month volatility of monthly returns. Lower is better.

**Quality**  
Measured using Return on Equity (ROE). Higher is better.

**Value**  
Measured using Price-to-Earnings ratio. Lower is better.

**Size**  
Measured using market capitalisation. Smaller companies within the large-cap universe receive higher scores.

Each factor is converted into a monthly percentile score between 0 and 1.

The final composite score is:

**Factor Score = 20% Momentum + 20% Low Risk + 20% Quality + 20% Value + 20% Size**

---

## Portfolio Construction

At each monthly rebalance date:

1. All eligible stocks are ranked by Factor Score.
2. The Top 10 stocks are selected.
3. Portfolio weights are assigned using one of three methods:

### Equal Weight

Each selected stock receives an equal allocation.

### Factor-Score Weight

Stocks with higher composite Factor Scores receive larger portfolio weights.

### Sharpe-Optimized Weight

Portfolio weights are optimized using trailing historical returns to maximize historical excess return relative to volatility.

The optimizer is long-only and uses only information available at each rebalance date.

---

## Backtesting Framework

- Historical period: 2020–2024
- Training period: 2020–2022
- Validation period: 2023
- Out-of-sample period: 2024
- Monthly rebalancing
- Top 10 holdings
- Benchmark: NIFTY 50
- Transaction-cost assumption: 0.25% per unit of turnover
- Time-varying Indian short-term risk-free-rate proxy
- 90-day reporting lag applied to annual fundamental data

Adjusted prices are used for return calculations.

Raw month-end prices are used for valuation-related calculations such as P/E and Market Capitalisation.

---

## Main Results

| Portfolio | CAGR | Volatility | Sharpe Ratio | Max Drawdown | Avg. Monthly Turnover |
|---|---:|---:|---:|---:|---:|
| Equal Weight | 24.17% | 16.82% | 1.08 | -15.42% | 16.23% |
| **Factor Score** | **25.03%** | 16.55% | **1.14** | **-14.67%** | **15.29%** |
| Sharpe Optimized | 18.32% | **16.43%** | 0.80 | -15.07% | 34.83% |
| NIFTY 50 | 14.87% | 18.91% | 0.57 | -23.25% | — |

The Factor-Score portfolio produced the strongest overall balance of return, risk-adjusted performance, drawdown and turnover.

---

## Out-of-Sample Performance

During the 2024 out-of-sample period:

- Factor-Score CAGR: approximately **15.49%**
- NIFTY 50 CAGR: approximately **8.8%**

The strategy continued to outperform the benchmark outside the earlier training and validation periods.

This does not guarantee future outperformance, but it provides a stronger test than evaluating the model only on the full historical sample.

---

## Robustness Testing

The strategy was tested across several alternative assumptions:

- Top 8, Top 10 and Top 12 holdings
- Different transaction-cost assumptions
- Alternative factor-weight combinations
- Year-by-year performance

Performance remained reasonably strong across these tests.

Some alternative factor-weight combinations produced stronger historical results, but the original equal 20% weighting was retained as the primary specification to avoid post-hoc overfitting.

---

## Technical Audit

The final model was audited for:

- Portfolio weights summing to 100%
- No negative weights
- Exactly 10 stocks selected each month
- No duplicate stock-date observations
- No missing factor values in scored rows
- No missing selected forward returns
- No future return data entering the optimizer
- Factor-score formula consistency
- Point-in-time fundamental-data usage
- Consistency between the primary strategy and robustness baseline

The final point-in-time fundamentals audit found **0 violations**.

---

## Key Findings

The project produced three main findings:

1. A systematic multi-factor ranking model produced stronger historical risk-adjusted performance than the NIFTY 50 over the tested period.

2. Factor-Score weighting performed slightly better than Equal Weight while maintaining moderate turnover.

3. More complex portfolio optimization did not automatically improve results. The Sharpe-Optimized portfolio produced higher turnover and weaker overall performance than the simpler methods.

---

## Limitations

The model has several important limitations:

- The universe is fixed at 30 present-day large-cap stocks, creating survivorship bias.
- A 90-day reporting lag is used as an approximation instead of exact company filing dates.
- Annual fundamentals are used rather than quarterly fundamentals.
- Transaction costs are modeled using a fixed assumption.
- Detailed bid-ask spread, slippage and market impact are not modeled.
- Historical relationships may not persist in the future.
- Historical NIFTY membership is not reconstructed.

---

## Data Sources

Historical stock and benchmark prices:

- Yahoo Finance
- yfinance Python library

Fundamental data:

- Screener.in company exports

Risk-free-rate proxy:

- India 3-month / 90-day short-term interest-rate series from FRED

Benchmark information:

- NIFTY 50 / NSE Indices

Transaction-cost reference:

- National Stock Exchange of India

---

## Repository Files

- `Mini_Quant_Fund_V1.ipynb` — full quantitative research notebook
- `Mini_Quant_Fund_Report.pdf` — detailed research report
- `Executive_Summary.pdf` — condensed project summary

---

## Reproducibility

The notebook is designed to run in Google Colab.

Historical fundamental-data files are not included directly in this repository. The notebook expects the required Screener Excel exports to be stored separately and referenced through Google Drive.

This avoids redistributing third-party data while keeping the methodology reproducible.

---

## Future Work

Planned extensions include:

- Extending the frozen strategy into 2025–2026
- Building a genuine forward-performance record
- Creating a current monthly model-portfolio signal
- Adding quarterly fundamental data
- Testing additional factors
- Reconstructing a historical investable universe
- Improving transaction-cost and execution modeling
- Building an interactive website/dashboard

---

## Disclaimer

This project is an educational quantitative research exercise.

It is not investment advice, a recommendation to buy or sell securities, or a claim that historical performance will continue in the future.
