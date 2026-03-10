# Market Entry Scanner

A cross-asset geopolitical market entry scanner built on RSI, moving average divergence, and return momentum signals. Covers five equity buckets with distinct strategies — mean reversion for quality tech and banks, momentum for defence, and selective mean reversion for luxury.

Live at: `https://codas0.github.io/market-entry-scanner/`

---

## How to use

1. Open the dashboard
2. Enter your [Financial Modeling Prep](https://financialmodelingprep.com) API key
3. Click **Run Scan** — data loads progressively per bucket
4. Adjust assumptions in the **⚙ Parameters** drawer if needed

Each ticker shows: signal label, composite score, RSI, 1W/3M returns, distance from 50/200-DMA, P/E, beta, and market cap.

---

## Signal labels

| Score | Mean Reversion | Momentum (Defense) |
|-------|---------------|-------------------|
| ≥ 70 | STRONG BUY | STRONG MO |
| ≥ 58 | MR ENTRY | MOMENTUM |
| 45–57 | NEUTRAL | CONSOLIDATING |
| 32–44 | CAUTION | WEAK |
| < 32 | AVOID | — |

Composite score = RSI (35%) + Return momentum (35%) + MA distance (30%).

---

## Buckets

| Bucket | Strategy | Coverage |
|--------|----------|----------|
| AI / Mega-Cap Tech | Mean reversion | NVDA, MSFT, GOOGL, META, AMZN, AAPL, AVGO, TSLA |
| Broad Tech Ex-AI | Mean reversion + FCF screen | 26 names — US software, cloud, cybersecurity, semis, ASML, SAP |
| Defense | Momentum | 20 names — US + European (BAE, Rheinmetall, Safran, Saab, MTU, HENSOLDT, Airbus, Thales, Leonardo) |
| Luxury / Discretionary | Selective MR (capped at 70) | 19 names — LVMH, Hermès, Richemont, Kering, Moncler, Ferrari + US experiential |
| Banks | Mean reversion | JPM, BAC, GS, MS, WFC, C, USB, TFC |

---

## Scoring methodology

**RSI (Relative Strength Index)**
Computed from raw OHLC history (default 14-day period). Sub-30 = maximum positive contribution. Above 60 = penalised. Logic inverted for the Defense momentum bucket.

**MA Distance**
Percentage deviation of price from the 50-day and 200-day moving averages. Computed from raw history. Price >10% below 50-DMA = strong signal; above 50-DMA = no entry.

**Return Momentum**
1-week and 3-month price returns. MR entry threshold: −5% (1W), −15% (3M). Inverted for Defense.

**Valuation context (not scored)**
P/E ratio (TTM) and beta are displayed alongside each ticker as context. They inform conviction but do not feed into the composite signal score.

---

## Data sources

| Source | Used for |
|--------|----------|
| [Financial Modeling Prep](https://financialmodelingprep.com) `/stable` endpoints | Price history, P/E, beta, market cap |
| Computed client-side | RSI, MA divergence, 1W/3M returns |

All API calls go directly from your browser to FMP using your own key. No data is stored or transmitted anywhere else.

---

## Parameters (all tunable in the UI)

| Parameter | Default | Description |
|-----------|---------|-------------|
| RSI Period | 14 | Lookback for RSI calculation |
| Slow MA | 50 | Days for short moving average |
| Long MA | 200 | Days for long moving average |
| RSI Deep Oversold | 30 | Max score threshold |
| RSI Oversold | 40 | Moderate score threshold |
| RSI Overbought | 60 | Penalty begins |
| RSI Deep OB | 70 | Max penalty |
| RSI Weight | 35 | % of composite score |
| 1W Return (strong) | −5% | Full return sub-score |
| 1W Return (mod) | −2% | Moderate return sub-score |
| 3M Return (strong) | −15% | Full return sub-score |
| 3M Return (mod) | −8% | Moderate return sub-score |
| Return Weight | 35 | % of composite score |
| MA Div (strong) | −10% | Full MA sub-score |
| MA Div (mod) | −5% | Moderate MA sub-score |
| MA Weight | 30 | % of composite score |
| Strong Buy threshold | 70 | Min score for top signal |
| MR Entry threshold | 58 | Min score for entry signal |
| Luxury cap | 70 | Max score for luxury bucket |

---

## Requirements

- A Financial Modeling Prep API key 
- A web server to serve the file (GitHub Pages works; opening as `file://` will fail due to CORS)

---

## Local development

```bash
# Clone
git clone https://github.com/yourusername/market-entry-scanner.git
cd market-entry-scanner

# Serve locally
python3 -m http.server 8080

# Open
open http://localhost:8080
```

---

## Disclaimer

This tool is for informational purposes only and does not constitute financial advice. All signals are generated mechanically from price data and do not account for fundamental developments, earnings surprises, or macro shifts or trends not reflected in price.
