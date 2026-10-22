# Research notes: "ETF effects: The role of primary versus secondary market activities"

> Comerton-Forde, C., Marta, T. (2025). *ETF effects: The role of primary versus secondary market activities.*
> Journal of Financial Markets, 75, 100983. DOI: [10.1016/j.finmar.2025.100983](https://doi.org/10.1016/j.finmar.2025.100983).
> Open access (CC BY-NC-ND 4.0). Earlier version: CEPR Discussion Paper DP18234 (2023).
> **The authors do not share their data.**

---

## 1. The paper in one paragraph

ETFs live on two markets. On the **primary market**, Authorized Participants (APs) create and redeem ETF units against the basket of underlying stocks. This arbitrage enforces the law of one price (ETF price ≈ NAV). On the **secondary market**, ETF units trade on the exchange, and high-frequency traders (HFTs) dominate that trading. The two activities affect the ETF's constituent stocks differently. AP arbitrage is infrequent, but it is associated with **higher volatility and lower liquidity** in overweighted constituents. HFT activity is associated with **narrower spreads**: liquidity "spills over" from the ETF to its stocks. Causality comes from a quasi-natural experiment, the Bank of Japan (BoJ) ETF purchases. Bottom line: primary-market harm is local and transitory, and the liquidity benefits of secondary-market trading dominate.

## 2. Key concepts

| Term | Meaning |
|---|---|
| **NAV** | Net asset value per ETF unit (value of the basket) |
| **Mispricing (premium/discount)** | $(Price - NAV)/NAV$ |
| **AP (Authorized Participant)** | Dealer allowed to create or redeem ETF units directly with the issuer |
| **Creation / redemption** | Primary market: AP delivers the basket and receives units (creation), or the reverse (redemption) |
| **HFT** | High-frequency traders: secondary market only, small inventories, short horizons |
| **Law of one price** | Identical cash flows should have identical prices; AP arbitrage closes the ETF–basket gap |
| **Limits to arbitrage** | Costs and risks make it unprofitable to trade away small gaps (Shleifer & Vishny, 1997) |

## 3. Two competing hypotheses

1. **Arbitrage channel** (Malamud 2015; Ben-David et al. 2018; Foucault et al. 2017). APs trade the basket to close the mispricing. This transmits non-fundamental demand shocks, adverse selection and volatility to the stocks. Prediction: **volatility ↑, liquidity ↓**.
2. **Information linkage channel** (Grossman 1992; Madhavan 1996; Fremault 1991). Secondary-market ETF trading, mostly by HFTs, diffuses information through quotes, so stock prices adjust without trading the stocks directly. Prediction: **spreads ↓, price efficiency ↑**.

## 4. Research questions and design

| # | Question | Data | Method |
|---|---|---|---|
| Q1 | What drives primary and secondary ETF activity, and which matters more? | Euronext Paris (AMF/BEDOFIH), 2015; 34 large ETFs (93% of ETF volume); trade-level HFT flag | ETF×day panel, ETF and day FE, double-clustered SEs |
| Q2 | How are the two activities related to the market quality of constituent stocks? | Lyxor CAC 40 ETF + 38 CAC 40 stocks; 5-min LSEG DataScope data | Stock×day panel, stock and day FE |
| Q3 | Is the primary-market effect **causal**? | Japan, Sep 2010 – Sep 2016; 2,173 TOPIX and Nikkei 225 stocks; BoJ ETF purchase days | Quasi-natural experiment; counterfactual (placebo) test with Z-test |
| (+) | Which effect dominates? | Japan 2010–2016 | Time-trend regressions of market quality |

## 5. Variables and equations

**Primary market activity.** $N_{e,t}$ is the number of ETF units outstanding at the end of the day:

$$PMActivity_{e,t} = \frac{N_{e,t} - N_{e,t-1}}{N_{e,t-1}}, \qquad \mathbb{1}_{PM} = \mathbb{1}\{|PMActivity_{e,t}| > 0\}$$

**HFT activity.** $V^{HFT}_{e,t}$ is the number of units traded where at least one side is an HFT:

$$HFTActivity_{e,t} = \log(1 + V^{HFT}_{e,t})$$

**Mispricing**, demeaned by ETF to neutralise NAV conventions:

$$ETFMispricing_{e,t} = \frac{Price_{e,t} - NAV_{e,t}}{NAV_{e,t}}$$

**Determinants of primary-market activity (5) and of HFT activity (6):**

$$PrimaryActivity_{e,t} = \alpha_e + \beta_1 ETFMispricing_{e,t} + \beta_2 HFTActivity_{e,t} + \varphi' X_t + \eta_t + \varepsilon_{e,t}$$

$$HFTActivity_{e,t} = \alpha_e + \beta_1 PrimaryActivity_{e,t} + \beta_2 |ETFMispricing_{e,t}| + \varphi' X_t + \eta_t + \varepsilon_{e,t}$$

**Constituent market quality (10)**, with $ETFOwn_{i,t} = ETFholdings_{i,t} / MarketValue_{i,t}$:

$$Y_{i,t} = \alpha_i + \beta_1 (ETFOwn_{i,t} \times |PMActivity_t|) + \beta_2 (ETFOwn_{i,t} \times HFTActivity_t) + \eta_t + \varphi' X_{i,t} + \varepsilon_{i,t}$$

$Y$ is one of: volatility of 5-minute returns, quoted spread, depth, resiliency (Kempf et al. 2009), and inefficiency, defined as $|AC_1|$ of 5-minute returns.

**Japanese experiment (11)–(12).** $PMStockActivity$ is the AP purchase of stock $i$ implied by the BoJ's ETF purchases, as a percentage of the stock's market value:

$$PMStockActivity_{i,t} = \frac{\sum_{ETF} BoJPurchases_{ETF,t} \times StockWeightInETF_{ETF,i,t}}{MarketValue_{i,t-22}}$$

$$Y_{i,t} = \alpha_i + \lambda_t + \beta_1 PMStockActivity_{i,t} + \beta' X_{i,t} + \varepsilon_{i,t}$$

**Counterfactual test (13).** On non-purchase days, a placebo $LastPMStockActivity$ replaces $PMStockActivity$, and the two slopes are compared:

$$Z = \frac{\hat\beta_{1,A} - \hat\beta_{1,B}}{\sqrt{\sigma^2(\hat\beta_{1,A}) + \sigma^2(\hat\beta_{1,B})}}$$

**Why the Japanese design identifies something.** The Nikkei 225 is **price-weighted**. The Nikkei-ETF purchase of stock $i$, as a share of its market value, is therefore proportional to

$$\frac{P_i \cdot PAF_i}{P_i \cdot Shares_i} = \frac{PAF_i}{Shares_i},$$

where $PAF$ is the price adjustment factor. High-price, low-share-count stocks (Fast Retailing, Fanuc, SoftBank, …) receive disproportionate BoJ-driven demand. For the TOPIX, which is cap-weighted, the same ratio is nearly identical across stocks and is absorbed by the day fixed effect. *Implication for our implementation: the identifying variation comes from the Nikkei 225.*

## 6. Main results (with numbers)

**Descriptives, France (Table 2)**
- Creations or redemptions occur on only **32%** of ETF-days. Primary-market volume is ≈ **13%** of secondary-market volume.
- HFTs account for ≈ **74%** (73.8%) of ETF volume, versus ≈ **50%** in CAC 40 stocks.
- Mispricing: mean 0.005%, s.d. 0.37%, min −1.55%.

**Determinants of primary-market activity (Table 3)**
- A 1 pp increase in $|ETFMispricing|$ raises $|PMActivity|$ by **0.079** pp (\*\*) and the probability of creation or redemption by **2.5** pp.
- Above-median mispricing: +0.143 pp (\*\*\*). Top tercile: +0.121 pp (\*\*\*). The threshold pattern fits limits to arbitrage.
- Signed version: APs **create** at a premium and **redeem** at a discount (slope 0.21–0.28). For the Lyxor CAC 40 (€3.8bn), a 1 pp premium implies €8–10.7m of creations.
- HFT activity is **unrelated** to primary-market activity in every specification.

**HFT (Table 4).** HFTActivity is unrelated to mispricing and to primary-market activity. HFTs do not do basis arbitrage at the daily frequency.

**Market quality of CAC 40 stocks (Table 5)**

| | Volatility | QuotedSpread | Depth | Resiliency | Inefficiency |
|---|---|---|---|---|---|
| ETFOwn × \|PMActivity\| | **0.029\*\*** | **0.004\*\*** | 0.017 | **−0.098\*\*** | **0.038\*\*** |
| ETFOwn × HFTActivity | 0.013 | **−0.006\*\*\*** | 0.004 | −0.068 | **−0.027\*** |

**The ETF is more liquid than its basket (Table 6).** The basket spread is 4.74 bps and the ETF spread 2.58 bps, a 2.17 bps gap. The gap widens with HFT activity (0.306\*\*\*). The ETF spread transmits to stock spreads (0.511\*\*\*).

**Japan, causal effect (Table 8; BoJ purchase days only, 349 of 1,481 days)**

| | Return | Volatility | QuotedSpread | Depth | Resiliency | Inefficiency |
|---|---|---|---|---|---|---|
| PMStockActivity | 0.239\*\*\* | 1.901\*\*\* | 0.011\*\* | −0.943\*\*\* | −0.018\*\*\* | −0.003\* |
| Z-test (A vs B) | 2.21\*\*\* | 1.70\*\* | 3.97\*\*\* | −3.16\*\*\* | −3.53\*\*\* | 1.38\* |

- The BoJ spends ≈ ¥70bn per purchase day, against ≈ ¥25bn of average daily trading in those ETFs.
- Economic size (authors' back-of-the-envelope): return +8%, volatility +2%, spread +2%, depth −6%. The conservative counterfactual version gives +4%, +1%, +3% and −5%.

**Which effect dominates (Table 9, Fig. 2).** Over 2010–2016, ETF ownership of Japanese stocks rose steeply while volatility and spreads **fell** and resiliency rose. The authors conclude that arbitrage harm does not dominate.

## 7. Critical assessment

1. **Narrow French sample.** One year (2015) and 34 ETFs. The stock-level analysis uses **one** ETF (Lyxor CAC 40), and Table 6 has only 255 observations.
2. **Primary-market proxy.** Changes in units outstanding are reported with lags; in the US, settlement can be delayed by up to 5 days (Evans et al. 2022, operational shorting). Intraday creations and redemptions are invisible.
3. **HFT proxy.** Investment banks (MIX flag) are excluded from the HFT flag although some of their desks run HFT strategies, so the HFT share is probably understated.
4. **Simultaneity in France.** Mispricing, volatility and activity are jointly determined. The authors acknowledge this, so the French results are associations.
5. **Asymmetric Japanese experiment.** The BoJ only bought, so **redemption** effects are untested. The BoJ bought on down days, so purchase days are endogenous, and the sample is restricted to them.
6. **External validity.** BoJ purchases (≈ ¥70bn per day) are an extreme demand shock, unlike routine arbitrage.
7. **Section 5 is a trend correlation.** In 2014–2015 the TSE **cut tick sizes** for TOPIX100 stocks (Jan 14 and Jul 22, 2014; Sep 24, 2015). That mechanically narrows quoted spreads and is confounded with the "ETF effect". Daily trend regressions also have autocorrelated errors, so Newey–West SEs are preferable to plain robust SEs.
8. **Minor text/table inconsistencies.**
   - The text reports a "1.14 pp annual rise" in ETFOwn, but the Table 9 slope is 0.19. The text says volatility falls by "1.2 pp per year", but the table shows −1.48.
   - The reported $PMStockActivity$ mean (0.33% of market cap per purchase day) is hard to reconcile with aggregate sizes: ≈ ¥70bn per day against a ≈ ¥300–600tn market implies ≈ 0.01–0.02% on average, so the units deserve clarification.
   - The sign of Inefficiency differs between France (+) and Japan (−).
9. **Timing of index weights (an implementation detail we found).** Eq. (11) writes the weight with a day-$t$ subscript. For the price-weighted Nikkei, a weight measured at the day-$t$ close contains the day-$t$ price, so it correlates mechanically with day-$t$ returns. On random prices our real-data pipeline produced a spurious return effect (t ≈ 5) until we switched to previous-close weights. The paper does not say which convention it used.

## 8. Related literature

- **Ben-David, Franzoni, Moussawi (2018, JF):** "Do ETFs increase volatility?"; ETF ownership raises stock volatility (arbitrage channel).
- **Malamud (2015):** a model of shock propagation through ETF arbitrage.
- **Bhattacharya & O'Hara (2018):** ETF information linkages and market fragility.
- **Box, Davis, Evans, Lynch (2021, JFE):** intraday arbitrage between ETFs and their portfolios.
- **Brogaard, Heath, Huang (2024):** ETF sampling and index arbitrage.
- **Pan & Zeng (2019):** bond-ETF arbitrage under liquidity mismatch.
- **Charoenwong, Morck, Wiwattanakantang (2021, RoF):** BoJ equity purchases; origin of the $PMStockActivity$ construction.
- **Barbon & Gianinazzi (2019, RAPS):** QE and equity prices; the BoJ ETF programme.
- **Harada & Okimoto (2021, IRFA):** BoJ purchases and Nikkei 225 stocks; afternoon-session effect.
- **Shleifer & Vishny (1997); Gromb & Vayanos (2010):** limits to arbitrage.
- **Kumar & Seppi (1994); Fremault (1991):** index arbitrage and information.
- *Used in our implementation:*
  - **Ardia, Guidotti, Kroencke (2024, JFE):** EDGE spread estimator from OHLC prices (`bidask` package).
  - **Corwin & Schultz (2012); Abdi & Ranaldo (2017):** spread estimators from daily data.
  - **Parkinson (1980):** range-based volatility.
  - **Amihud (2002):** illiquidity.
  - **Newey & West (1987):** HAC standard errors.

## 9. Current context (October 2026)

- Global ETF assets hit a record **$24.03trn** at end-August 2026 (ETFGI), up from the $15trn (Dec 2024) cited in the paper.
- The **Bank of Japan** stopped new ETF purchases in March 2024. On 19 Sep 2025 it decided to **sell** its ETFs at ≈ ¥330bn a year at book value (≈ ¥620bn at market value, about 0.05% of market turnover). Sales **started in January 2026**. The BoJ holds roughly 7% of the Japanese equity market through ETFs.
- This mirrors the paper: the redemption side it could not study is now under way, but so slowly that, by the paper's own logic, the effect on stocks should be negligible.

## 10. Data availability: what can be reproduced

| Paper (proprietary) | Open substitute | Caveat |
|---|---|---|
| Euronext BEDOFIH trade data with HFT flag | **SEC MIDAS** per-security daily metrics (cancel-to-trade, odd-lot ratio, trade-to-order volume): standard algorithmic/HFT proxies | Proxy, not a flag; US only |
| Units outstanding and NAV (Bloomberg/Datastream) | Issuer files (e.g., iShares NAV and shares-outstanding history) + `yfinance` closes | Manual download or scraping; formats change |
| 5-min LSEG data (spread, depth, resiliency) | Daily OHLCV from `yfinance` → **EDGE** / Corwin–Schultz / Abdi–Ranaldo spreads, Parkinson volatility, Amihud illiquidity | No resiliency, no intraday inefficiency |
| BoJ purchase dates and amounts | BoJ market-operations statistics (boj.or.jp) | Must be downloaded and saved as CSV (schema in the notebook) |
| Nikkei 225 / TOPIX weights | Nikkei Indexes (constituents, PAF), prices and shares from `yfinance` | Pre-2016 composition is incomplete; survivorship bias |
| — | Simulation of the AP + HFT + mispricing ecosystem | Illustrates mechanisms and validates estimators; not evidence |

---

### Sources

- Paper: [Journal of Financial Markets, DOI 10.1016/j.finmar.2025.100983](https://doi.org/10.1016/j.finmar.2025.100983); [Minerva Access (University of Melbourne)](https://minerva-access.unimelb.edu.au/server/api/core/bitstreams/f33e76b0-27ca-49db-b991-0f0c5c64e329/content); [CEPR DP18234 on IDEAS/RePEc](https://ideas.repec.org/p/cpr/ceprdp/18234.html)
- ETF assets: [ETFGI via Markets Media: global ETF assets top $24 trillion](https://www.marketsmedia.com/global-etf-assets-top-24-trillion-for-first-time/)
- BoJ ETF sales: [BoJ statement, 19 Sep 2025](https://www.boj.or.jp/en/mopo/mpmdeci/state_2025/k250919a.htm); [Oxford Economics](https://www.oxfordeconomics.com/resource/boj-announces-cautious-plan-to-sell-etf-and-j-reit-holdings/); [Northern Trust](https://www.northerntrust.com/united-states/insights-research/2025/weekly-economic-commentary/the-bank-of-japan-prunes-its-portfolio); [Adnkronos/Jiji: BoJ begins unloading ETFs in January](https://english.adnkronos.com/2026/02/03/boj-begins-unloading-etfs-in-jan/)
- BoJ operations data: [Market Operations by the Bank of Japan](https://www.boj.or.jp/en/statistics/boj/fm/ope/index.htm)
- BoJ purchase literature: [Charoenwong, Morck, Wiwattanakantang, NBER w25525](https://www.nber.org/papers/w25525); [Harada & Okimoto, RIETI DP 19-E-014](https://www.rieti.go.jp/en/publications/summary/19030006.html)
- Ben-David, Franzoni, Moussawi: [NBER w20071](https://www.nber.org/papers/w20071)
- SEC MIDAS: [Metrics by Individual Security](https://www.sec.gov/data-research/sec-markets-data/marketstructuredata-security); [WRDS: SEC MIDAS](https://wrds-www.wharton.upenn.edu/pages/data-announcements/sec-midas-wrds/)
- EDGE: [`bidask` on PyPI](https://pypi.org/project/bidask/); [GitHub eguidotti/bidask](https://github.com/eguidotti/bidask)
- Nikkei 225 PAF: [Nikkei FAQ](https://indexes.nikkei.co.jp/nkave/archives/faq/faq_nikkei_stock_average_en.pdf); [2021 methodology change](https://indexes.nikkei.co.jp/nkave/archives/news/20210705E_1.pdf)
- TSE tick sizes: [JPX tick-size pilot](https://www.jpx.co.jp/english/news/1030/b5b4pj000004baqo-att/b5b4pj000004bboh.pdf); [arXiv 1602.00839](https://arxiv.org/abs/1602.00839v7)
