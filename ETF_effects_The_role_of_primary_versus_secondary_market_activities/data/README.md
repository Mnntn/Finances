# Inputs for notebook §5 (BoJ experiment on real data)

Place these files here before running §5 in Colab:

| File | Columns | Notes |
|---|---|---|
| `boj_etf_purchases.csv` | `date` (YYYY-MM-DD), `amount_jpy_bn` | Daily totals of the Bank of Japan's regular ETF purchases (BoJ → Statistics → Market Operations) |
| `nikkei225_paf.csv` | `code` (4-digit TSE code), `paf` | Nikkei 225 constituents and their current price adjustment factors (Nikkei Indexes) |
| `control_universe.csv` *(optional)* | `code` | Non-Nikkei TOPIX stocks used as controls |
