# ETF effects: The role of primary versus secondary market activities

Paper review and replication of

> Comerton-Forde, C., Marta, T. (2025). *ETF effects: The role of primary versus secondary market activities.*
> Journal of Financial Markets, 75, 100983. https://doi.org/10.1016/j.finmar.2025.100983

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Mnntn/Finances/blob/main/ETF_effects_The_role_of_primary_versus_secondary_market_activities/etf_effects.ipynb)

## Contents

| File | What it is |
|---|---|
| [`etf_effects.ipynb`](etf_effects.ipynb) | Colab/Jupyter notebook: econometric toolkit, simulation labs, BoJ Monte Carlo, real-data modules |
| [`etf_effects_presentation.pptx`](etf_effects_presentation.pptx) | 20-slide presentation (16:9) with speaker notes |
| [`research_notes.md`](research_notes.md) | Paper summary, equations, results, critique, literature, data sources |
| `figures/` | Charts produced by the notebook (used in the slides) |
| `data/` | Optional inputs for the real-data BoJ module (see below) |

## Notebook at a glance

| § | Content | Paper counterpart | Runs offline |
|---|---|---|---|
| 0 | Two-way fixed-effects OLS with double-clustered SEs (checked against `linearmodels`) | all tables | ✅ |
| 1 | The paper in brief | §1 | ✅ |
| 2 | Simulation lab I: AP arbitrage with limits to arbitrage vs. HFT trading; size and power of the HFT test | Tables 2–4, Fig. 1 | ✅ |
| 3 | Simulation lab II: arbitrage and information-linkage channels in constituent stocks | Table 5 | ✅ |
| 4 | Bank of Japan experiment: identification, paper-sized simulation, Monte Carlo of Table 8 | Tables 7–8 | ✅ |
| 5 | Real data: BoJ experiment on Japanese stocks + dose–response across BoJ regimes (2021–24 placebo) | Table 8 + extension | Colab |
| 6 | Real data: ETF vs. basket liquidity (SPY, 1321.T); Japanese trend with tick-size controls | Tables 6, 9 | Colab |
| 7 | Takeaways and limitations | §6 | ✅ |

The authors' data (Euronext HFT flags, 5-minute LSEG quotes) are proprietary, so §2–4 rebuild the mechanisms and estimators on simulated data with known truth. §5–6 use open data:

- Yahoo Finance via `yfinance`;
- spreads from the EDGE estimator (`bidask`) and the Abdi–Ranaldo estimator;
- Parkinson volatility and Amihud illiquidity.

These sections skip themselves if a source is unreachable.

### Inputs for §5 (put them in `data/`)

| File | Columns | Source |
|---|---|---|
| `boj_etf_purchases.csv` | `date, amount_jpy_bn` | Bank of Japan → Statistics → Market Operations (daily ETF purchase results) |
| `nikkei225_paf.csv` | `code, paf` | Nikkei Indexes → Nikkei 225 constituents and price adjustment factors |
| `control_universe.csv` *(optional)* | `code` | Any list of non-Nikkei TOPIX stocks |

## Key results reproduced in simulation

- **Calibration matches the paper's Table 2:** primary-market activity on 31% of days (paper: 32%), mispricing s.d. 0.36% (0.37%), HFT share 70% (74%).
- **Table 3:** mispricing drives the magnitude, frequency and direction of creations and redemptions, with a threshold; HFT activity does not.
- **Table 4:** the "HFTs don't arbitrage" regression rejects in 7–10% of samples when HFTs ignore mispricing and in 100% when they arbitrage, so the paper's null is informative.
- **Table 5:** the paper's specification recovers both channels. Arbitrage raises volatility, spreads and inefficiency; HFT activity lowers spreads and inefficiency.
- **Table 8:** across 20 paper-sized markets, BoJ-day estimates centre on the true effect and placebo estimates on zero. Nikkei weights must come from the previous close, or a spurious return effect appears.
