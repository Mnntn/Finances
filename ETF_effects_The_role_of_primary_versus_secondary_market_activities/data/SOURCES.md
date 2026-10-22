# Data sources for notebook §5

All files were downloaded on **2026-10-22**. Prices and volumes are not stored here: the notebook downloads them from Yahoo Finance (`yfinance`) when it runs.

## `boj_etf_purchases.csv`: Bank of Japan regular ETF purchases

- **Source:** Bank of Japan, "Purchases of ETFs and J-REITs", past results archive
  https://www.boj.or.jp/en/statistics/boj/fm/ope/etf/pastetfreit.zip
  (linked from the daily market-operations menu https://www3.boj.or.jp/market/en/menu_o.htm and from https://www.boj.or.jp/en/statistics/boj/fm/ope/index.htm).
- **Contents of the archive:** one spreadsheet per calendar year, 2010–2024 (2016 is split into "until March" and "from April"). Each lists purchases by trade date in units of ¥100 million.
- **Processing:**
  - Kept the column "ETF" (2010 – Mar 2016) or "(1) Purchases of ETFs other than those mentioned in (2)" (Apr 2016 onwards), i.e. the regular purchases.
  - Converted ¥100 million to ¥ billion (divided by 10) and kept days with a positive amount.
  - **Left out** the separate "(2) Purchases of ETFs to support investment in physical and human capital": 1,219 small daily purchases (mostly ¥1.2bn), Apr 2016 – Mar 2021, ¥1,462.8bn in total.
  - J-REIT purchases are not included.
- **Result:** 694 purchase days, 2010-12-15 to 2023-10-04, ¥35,692.1bn in total.
  - Sep 2010 – Sep 2016 (the paper's sample): 355 purchase days, ¥9,921.7bn. Typical size rises from ¥14–35bn (2010–2016 H1) to ¥70bn+ after the July 2016 expansion.
  - The BoJ made no regular ETF purchase after 2023-10-04. The 2024 sheet is empty up to the end of new purchases on 2024-03-19.

## `nikkei225_paf.csv`: Nikkei 225 constituents and price adjustment factors

- **Source:** Nikkei Indexes, "Price Adjustment Factor (CSV)", linked from the Nikkei 225 profile page https://indexes.nikkei.co.jp/en/nkave/index/profile?idx=nk225
  https://indexes.nikkei.co.jp/nkave/archives/file/nikkei_225_price_adjustment_factor_en.csv
- **Date of data:** 2026/10/22. The file's note says that, for a capped constituent, the factor shown is the capped price adjustment factor.
- **Processing:** kept `Code` → `code` and `Price Adjustment Factor` → `paf`; dropped the name/sector columns and the copyright footer.
- **Checks:** 225 rows, unique codes. The codes match the current constituent list at https://indexes.nikkei.co.jp/en/nkave/index/component?idx=nk225 (updated 2026-10-22) exactly.
- **Terms:** the file carries Nikkei's notice that it is a copyrightable work of Nikkei, and that copying or circulating it without permission is prohibited. Only the code and factor columns are kept here, for research use.

## `control_universe.csv`: control stocks (TOPIX 500 minus Nikkei 225)

- **Source:** Japan Exchange Group, "List of TOPIX constituents and weights", linked from https://www.jpx.co.jp/english/markets/indices/topix/index.html
  https://www.jpx.co.jp/automation/english/markets/indices/topix/files/topixweight_e.csv
- **Date of data:** 2026-08-31.
- **Processing:**
  - TOPIX 500 = stocks whose "New Index Series Code" is TOPIX Core30, TOPIX Large70 or TOPIX Mid400. That is 492 stocks on this date.
  - Removed the 224 of them that are current Nikkei 225 constituents. The one Nikkei stock outside TOPIX 500 is 6103 Okuma (TOPIX Small 1).
- **Result:** 268 codes.

## Caveats

- **Survivorship and look-ahead.** Both lists are *current* memberships, applied to 2010–2024. Stocks that left the Nikkei 225 or TOPIX 500 before 2026 are missing, and some current members were not members (or not listed) in earlier years.
- **Current factors.** Using today's factors with split-adjusted Yahoo prices reproduces historical Nikkei weights only for stocks whose factor changed solely because of splits.
