# Sustainability-Aware Asset Management
### Portfolio Allocation with a Carbon Objective

Group project — HEC Lausanne, MSc in Finance (Asset and Risk Management track), 2026.

## Overview

This project builds and evaluates carbon-aware portfolio strategies on an emerging-markets equity universe. Starting from a standard minimum-variance / value-weighted framework, we progressively introduce carbon constraints — a static 50% footprint reduction and a dynamic net-zero glide path — and measure their impact on both financial performance and carbon exposure.

The analysis is entirely out-of-sample: portfolios are re-optimized every year using only information available up to that point, and evaluated on realized returns going forward.

**Research question:** Can carbon emissions be meaningfully reduced in a portfolio without materially sacrificing risk-adjusted performance?

**Answer, in short:** Yes — carbon-constrained portfolios in this sample did not underperform their unconstrained counterparts; several constrained variants outperformed on a risk-adjusted basis.

## Data

- **Universe:** 702 firms in emerging markets, filtered by ISIN from a broader global dataset.
- **Financial data:** monthly/annual market capitalization and total return indices.
- **Carbon data:** Scope 1 and Scope 2 greenhouse gas emissions, annual.
- **Source:** proprietary dataset provided by Prof. Eric Jondeau (HEC Lausanne) for course use. Raw data files are **not included** in this repository due to licensing restrictions — see [Data Access](#data-access) below.
- **Sample period:** 2004–2025 (2004–2013 used as the initial estimation window; portfolios held and evaluated from 2014 to 2025).

## Methodology

1. **Data cleaning** — ISIN-based filtering, delisting handling, forward-fill of missing values, stale-price screening.
2. **Baseline portfolios** — long-only minimum-variance (MV) and value-weighted (VW) portfolios, rebalanced annually using a 10-year rolling estimation window (historical mean returns, MLE covariance matrix).
3. **Carbon metrics** — Weighted Average Carbon Intensity (WACI) and portfolio carbon footprint, computed from Scope 1+2 emissions and ownership shares.
4. **Carbon-constrained portfolios:**
   - Risk-minimizing MV portfolio under a 50% carbon footprint cap relative to the unconstrained MV portfolio.
   - Tracking-error-minimizing portfolio under a 50% carbon footprint cap relative to the VW benchmark.
5. **Net-zero portfolio** — tracking-error minimization under a carbon budget that declines by 10% per year from the 2013 VW baseline.
6. **Performance evaluation** — annualized return, volatility, Sharpe ratio, and min/max monthly returns for every strategy, compared against carbon-metric trajectories over time.

All optimizations are long-only, fully invested, and solved as constrained quadratic programs.

## Key results

| Metric | MV | MV −50% CF | VW | VW −50% CF | VW Net-Zero |
|---|---|---|---|---|---|
| Ann. return | 7.7% | 7.4% | 8.2% | 12.5% | 12.0% |
| Ann. volatility | 10.4% | 10.1% | 15.5% | 18.1% | 18.9% |
| Sharpe ratio | 0.57 | 0.56 | 0.42 | 0.59 | 0.54 |

- The unconstrained MV portfolio already runs a lower carbon footprint than the VW benchmark (~342 vs. ~605 tCO₂e/$M invested on average), a side effect of risk minimization rather than an explicit objective.
- Imposing a 50% carbon-footprint cut on the VW benchmark did **not** reduce risk-adjusted returns — the constrained portfolio's Sharpe ratio (0.59) exceeds the unconstrained benchmark's (0.42).
- The net-zero glide path achieves a more persistent, compounding reduction in carbon footprint than a one-off 50% cut, at a small cost in Sharpe ratio relative to the static-constraint version.

Full derivations, figures, and discussion are in `Report_Groupe_S.pdf`.

## Repository structure

```
├── Notebook_Groupe_S.ipynb      # Full analysis: data cleaning → portfolio construction → carbon metrics → results
├── Report_Groupe_S.pdf          # Written report (methodology, results, discussion, limitations)
├── Sales Pitch_Groupe_S.pdf     # Short investor-facing summary
├── Data/                        # Raw data files (excluded from repo — see Data Access)
└── Results/
    ├── Numerics/                 # CSV outputs: returns, statistics, carbon metrics per strategy
    └── Plots/                    # Figures used in the report
```

## Data access

The underlying financial and carbon datasets were provided by Prof. Eric Jondeau as part of HEC Lausanne's Asset Management course and are subject to academic licensing restrictions. They are not redistributed in this repository. The notebook expects the original files (`Static_2025.xlsx`, `DS_*.xlsx`, `Risk_Free_Rate_2025.xlsx`) under `Data/` to run end-to-end.

## My contribution

I was responsible for the full implementation of the notebook — data cleaning, portfolio construction, carbon-metric computation, and all constrained optimizations — as well as parts of the written report (methodology and results sections).

## Tools

Python (pandas, NumPy, `scipy.optimize.minimize` for the constrained optimizations, Matplotlib).

## Authors

Lukas Tonkovic, Marko Zivaljevic, Philippe Borghini Villiger, Stefan Stevanovic — MSc in Finance, HEC Lausanne (2026).

## Disclosure

Large language model tools (see `Report_Groupe_S.pdf`, Section 7) were used in a supervised capacity for code debugging, writing assistance, and literature search, in accordance with HEC Lausanne's academic integrity guidelines. All analysis, code, and conclusions are the authors' own work.
