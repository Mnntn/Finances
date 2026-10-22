# Real-data results (notebook §5–6)

Notebook executed on 2026-10-22 with prices from Yahoo Finance (`yfinance` 1.7.0) and the inputs in [`data/`](data/) (sources in [`data/SOURCES.md`](data/SOURCES.md)). All numbers below are copied from the executed notebook. SE in §6 is computed as coef / t from the printed values.

## Headline

- **§5, BoJ experiment (paper Table 8):** not reproduced with daily open data. On BoJ purchase days in Sep 2010 – Sep 2016, Nikkei-leg purchase intensity has no significant effect on returns, volatility or spreads. None of the Z-tests separates purchase days from the placebo days.
- **§6.1, ETF vs. basket (paper Table 6):** reproduced in direction. SPY and 1321.T are more liquid than their baskets, and basket spreads co-move with ETF spreads.
- **§6.2, Japanese trend (paper Table 9):** no significant improvement in large-stock spreads over 2010–2016, with or without tick-size dummies.

## §5 BoJ experiment on Japanese stocks

**Sample.**
- 493 tickers: the 225 current Nikkei 225 constituents and 268 controls (TOPIX 500 minus Nikkei 225). Yahoo has no 2010–2024 history for 6 of them (285A, 5016, 8729, 9023 listed after March 2024; 7240 and 9508 have Yahoo history only from July 2026).
- Paper period, Sep 2010 – Sep 2016:
  - 1,490 trading days, 355 of them BoJ purchase days;
  - 469 stocks with data, 217 of them Nikkei;
  - 734,570 stock-days, 175,015 on BoJ days.
- `PMStockActivity` (bps of market value) on BoJ days has a mean of 0.336 across all stocks, close to the paper's 0.33. The mean is 0.737 across Nikkei stocks only.

**Units.**
- Return: close-to-close, %.
- Volatility: Parkinson, annualised %.
- QuotedSpread: Abdi–Ranaldo, %.
- Illiquidity: Amihud, |return| per ¥1trn of turnover. It replaces the paper's Depth, so its expected sign is the opposite of Depth's.

### Table 8 replication, Sep 2010 – Sep 2016 (stock and day fixed effects, SEs double-clustered by stock and day)

| Outcome | A: coef (BoJ days) | A: SE | A: t | B: coef (placebo) | B: SE | B: t | C: Z (A vs B) | N (A) | N (B) |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Return | −0.027 | 0.034 | −0.79 | −0.033 | 0.018 | −1.86 | 0.16 | 161,598 | 508,875 |
| Volatility | −0.031 | 0.221 | −0.14 | 0.243 | 0.180 | 1.35 | −0.96 | 161,926 | 511,012 |
| QuotedSpread | −0.010 | 0.019 | −0.54 | 0.008 | 0.010 | 0.78 | −0.83 | 161,456 | 510,407 |
| Illiquidity | −3.693 | 1.799 | −2.05 | −3.075 | 1.347 | −2.28 | −0.28 | 161,598 | 508,875 |

For comparison, the paper's Panel A reports:

| Outcome | coef (SE) | Z |
|---|---|---:|
| Return | 0.239 (0.054) | 2.21 |
| Volatility | 1.901 (0.505) | 1.70 |
| QuotedSpread | 0.011 (0.005) | 3.97 |
| Depth | −0.943 (0.214) | −3.16 |

Here:
- No Panel A coefficient has the paper's sign and significance.
- The only |t| > 2 result is Illiquidity. Its sign means *more* liquidity, opposite to the paper's depth decline, and it is just as large on non-purchase days (Z = −0.28). It is not a purchase-day effect.

### Extension: dose–response by BoJ regime (BoJ days only; slope on Nikkei-leg intensity)

| Regime | Regressor | Outcome | coef | SE | t | N |
|---|---|---|---:|---:|---:|---:|
| Sep 2010 – Sep 2016 (paper), 355 BoJ days | PMStockActivity | Return | −0.027 | 0.034 | −0.79 | 161,598 |
| | | Volatility | −0.031 | 0.221 | −0.14 | 161,926 |
| | | QuotedSpread | −0.010 | 0.019 | −0.54 | 161,456 |
| | | Illiquidity | −3.693 | 1.799 | −2.05 | 161,598 |
| Oct 2016 – Mar 2021, 323 BoJ days | PMStockActivity | Return | −0.153 | 0.118 | −1.29 | 153,197 |
| | | Volatility | −0.854 | 0.457 | −1.87 | 153,200 |
| | | QuotedSpread | 0.050 | 0.073 | 0.69 | 153,197 |
| | | Illiquidity | −2.023 | 0.677 | −2.99 | 153,197 |
| Apr 2021 – Mar 2024 (placebo: TOPIX-only purchases), 16 BoJ days | Intensity (hypothetical) | Return | 1.004 | 0.612 | 1.64 | 7,703 |
| | | Volatility | −1.048 | 2.543 | −0.41 | 7,704 |
| | | QuotedSpread | 0.116 | 0.191 | 0.61 | 7,704 |
| | | Illiquidity | −3.704 | 1.610 | −2.30 | 7,703 |

There is no dose–response pattern. The effect is not larger when the Nikkei share of purchases was larger (before Oct 2016). The negative Illiquidity slope is as strong in the 2021–24 placebo, when the BoJ bought no Nikkei ETFs. So it reflects which stocks carry high Nikkei weight, not the purchases.

## §6.1 ETF vs. basket liquidity (paper Table 6)

**Setup.** Monthly EDGE spreads (bps) from daily OHLC, Jan 2021 – Oct 2026 (70 months; October 2026 is partial). The basket is the 10 largest holdings, cap-weighted with current market caps.

| Pair | Series | Mean | s.d. | Median | N (months) |
|---|---|---:|---:|---:|---:|
| SPY | ETF spread | 32.84 | 22.31 | 30.47 | 70 |
| | Basket spread | 71.37 | 28.91 | 66.57 | 70 |
| | Basket − ETF | 38.53 | 20.18 | 36.91 | 70 |
| 1321.T | ETF spread | 42.55 | 26.43 | 35.62 | 70 |
| | Basket spread | 84.29 | 32.22 | 77.88 | 70 |
| | Basket − ETF | 41.75 | 25.64 | 37.36 | 70 |

**Basket spread on ETF spread (Newey–West, 3 lags):**

| Pair | β (ETF spread) | SE | t | β (lag) | SE | t (lag) | R² | N (months) |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| SPY | 0.951 | 0.122 | 7.80 | −0.086 | 0.087 | −0.99 | 0.530 | 69 |
| 1321.T | 0.758 | 0.190 | 3.98 | 0.085 | 0.179 | 0.48 | 0.406 | 69 |

Both ETFs are more liquid than their baskets, which matches the paper (2.6 vs. 4.7 bps for the CAC 40 ETF). Basket spreads move with the ETF's spread in the same month (β = 0.95 for SPY, 0.76 for 1321.T), which is consistent with the paper's information-linkage channel.

## §6.2 Japanese large-stock trend, Sep 2010 – Sep 2016 (paper Table 9)

**Setup.**
- 25 large TOPIX100/Nikkei stocks, cap-weighted monthly averages, N = 73 months.
- Newey–West SEs with 6 lags.
- Tick-regime dummies for 14 Jan 2014, 22 Jul 2014 and 24 Sep 2015.

| Outcome | Model | Slope per year | SE | t | R² |
|---|---|---:|---:|---:|---:|
| QuotedSpread (EDGE, bps) | trend only | 1.057 | 1.512 | 0.70 | 0.008 |
| | trend + tick regimes | 2.645 | 3.156 | 0.84 | 0.093 |
| Volatility (Parkinson, %) | trend only | 0.554 | 0.428 | 1.29 | 0.036 |
| | trend + tick regimes | 1.622 | 1.030 | 1.58 | 0.219 |

No trend is significant. The point estimates suggest that spreads and volatility slightly *worsened* over 2010–2016, so the improvement the paper reads as an ETF benefit does not appear in these daily-data proxies. Adding tick-size dummies does not change that.

## Simulation headline check (§2–4, fixed seeds)

Every number the slides quote is unchanged by the re-run:

| Statistic | Value |
|---|---|
| PM days | 30.6% (31%) |
| Mispricing s.d. | 0.358% (0.36%) |
| HFT share | 69.7% (70%) |
| PM / secondary volume | 0.104 (10%) |
| HFT-test rejection rate under the null | 7% (\|ETFMispricing\|) and 10% (\|PMActivity\|) |
| HFT-test power | 100% |

## Fixes made to the live cells

Each fix is in `download_ohlcv` or the share-count helpers, in the second code cell of §5:

1. **Batching.** `download_ohlcv` downloads in batches of 50 tickers with a 2-second pause, then retries the tickers Yahoo returned empty. During testing, Yahoo returned 1321.T and 4544.T empty on a first request; the retry recovers such tickers.
2. **Zero-volume placeholder rows.** Yahoo fills Japan Airlines (9201.T) before its Sep 2012 relisting with Close = 1, volume 0. It also pads 22 Tokyo holidays (mostly 2017–18) with stale zero-volume rows for every stock. These rows are now treated as missing, and dates on which nothing traded are dropped.
   - **Impact:** before this fix, JAL's tiny "market value" gave intensities up to 3,047 bps. Panel A then showed spurious t-stats of 11–14 on all four outcomes.
3. **Stale share counts after recent splits.** Yahoo adjusts prices for a split at once but can keep the pre-split share count. This hit Tokyo Electron (5:1, 29 Sep 2026), Tokio Marine (15:1), Kioxia (3:1) and Toyoda Gosei (5:1), whose market values were too small by the split ratio.
   - **Fix:** `split_lag` detects a count still at the pre-split level in Yahoo's share history and rescales it. A check of all 493 tickers found 55 splits in the past year: 4 stale counts, 51 already updated and none ambiguous.
   - **Coverage:** applied to `shares_outstanding` (§5) and `caps` (§6).
4. **Isolated bad prints.** 1321.T on 6 Oct 2026 is quoted at 1/100 of the surrounding prices, apparently adjusted twice for its 100:1 split. It produced a 670 bps "spread" for October 2026.
   - **Fix:** closes more than 3× away from both neighbours are now masked. This was the only such print in all downloaded data.
5. **Warning noise.** DeprecationWarnings from `yfinance` under pandas 3 are silenced, which removed about 790 KB of warning output and local paths from the saved notebook.

## Caveats

- **Survivorship and look-ahead.** Nikkei 225 and TOPIX 500 memberships are as of 2026 and applied to 2010–2024. The same applies to the §6 baskets and the §6.2 stock list.
- **Current price adjustment factors.** Today's factors (e.g. Fast Retailing 2.4, SoftBank Group 24, Tokyo Electron 15) are used with split-adjusted prices. They reproduce historical Nikkei weights only where factors changed through splits alone; any other change (capping, rule changes) distorts them. For example, the implied Fast Retailing weight in Sep 2016 is 5.7%.
- **Market value.** Market value is the current share count × split-adjusted price, so it ignores issuance and buybacks.
- **Nikkei share of BoJ purchases.** The share is assumed, not observed: 50% before Oct 2016, 25% to Mar 2021 and 0% afterwards. The 2021–24 placebo has only 16 purchase days, and its intensity is hypothetical.
- **Daily proxies.** The outcomes are daily proxies for the paper's 5-minute measures: Abdi–Ranaldo spread, Parkinson volatility, and Amihud instead of depth. They are much noisier, which lowers power.
- **EDGE levels are biased upward.** `bidask.edge` with `sign=False` returns √|s²|, so noise-driven negative estimates count as positive spreads. For very liquid instruments the monthly levels are therefore far above true quoted spreads: SPY's true spread is well under 1 bp. The ETF-vs-basket comparison and the co-movement regression are more informative than the levels.
- **No BoJ purchase data after 2023.** The BoJ made no regular ETF purchases after 2023-10-04. Purchases of ETFs supporting physical and human capital investment are excluded (see `data/SOURCES.md`).
