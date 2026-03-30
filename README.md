# Short-Term Trader — Claude Skill

An expert short-term trading skill for Claude covering stocks, options, derivatives, and crypto — with a built-in intraday macro dashboard, FinancialJuice news agent, and Magnificent Seven weekly options focus. Built for Cowork and Claude Code.

## What It Does

### Core Trading Analysis
- **Options analysis** — Greeks, IV rank, strategy selection, earnings plays, chain reading
- **Technical analysis** — Chart patterns, indicators (RSI, MACD, VWAP, BBands), market structure
- **Trade setup analysis** — Entry/exit, R:R, thesis validation, position sizing
- **Risk management** — 1% rule, Kelly criterion, stop placement, portfolio heat
- **Crypto trading** — Perpetual futures, funding rates, on-chain signals, DeFi
- **Trade journaling** — Log trades, P&L, R-multiple tracking, behavioral analysis
- **Strategy backtesting** — Rules → metrics → overfitting detection

### Macro Dashboard & Intraday Bias (NEW)
Daily put/call bias engine covering 8 indicators:
- **SOFR Rate** — overnight funding / liquidity signal
- **U.S. 2Y & 10Y Yields** — Fed policy sensitivity, equity discount rate
- **Fed Rate Cut Odds** (Polymarket) — market expectation of cuts
- **Cleveland Fed PCE Nowcast** — real-time inflation vs. consensus
- **RRP (Reverse Repo)** — excess liquidity in system
- **TGA Balance** — Treasury liquidity injection/drain
- **FinancialJuice News Flow** — live squawk tone (dovish/hawkish)

Produces a scored dashboard (+1 bull / 0 neutral / -1 bear per indicator) and states: **CALLS / PUTS / NO TRADE** with specific focus tickers.

### FinancialJuice News Agent (NEW)
- Fetches live news by timeframe (pre-market, market open, last hour, custom NY time)
- Stores news by date to `news-log/` directory
- Summarizes headlines into clean, categorized briefs
- Analyzes news impact on specific stocks
- Cross-references news with macro dashboard for trade decisions

---

## Skill Structure

```
short-term-trading-skill/
├── SKILL.md                          # Main skill: 7 workflows, all triggers
├── README.md                         # This file
├── evals/
│   └── evals.json                    # 6 test cases
└── references/
    ├── macro-dashboard.md            # 8-indicator scoring system, Mag 7 ref, VIX guide
    ├── financialjuice-agent.md       # News fetch, store, summarize, stock analysis
    ├── options.md                    # Options strategies, Greeks, IV, earnings
    ├── technicals.md                 # Chart patterns, indicators, market structure
    ├── risk.md                       # Position sizing, stops, drawdown management
    └── crypto.md                     # Crypto mechanics, DeFi, derivatives
```

The news log is stored separately:
```
news-log/
└── YYYY-MM-DD_HH-MM_financialjuice.md   # Date-stamped news archives
```

---

## Trigger Phrases

### Macro & Daily Bias
- "morning brief", "macro update", "put/call bias today"
- "QQQ setup", "what's the bias", "calls or puts"
- "SOFR", "RRP", "TGA", "10Y yield", "VIX level"

### FinancialJuice News
- "get me the news", "what's on the squawk", "FinancialJuice"
- "morning news", "pre-market news", "open news", "news since [time]"
- "any news on [ticker]", "what's moving [ticker]"
- "store the news", "summarize today's news", "what's the tape saying"

### Trade Analysis
- Trade ideas, setups, entries, exits, stops, targets
- Options: calls, puts, spreads, straddles, iron condors, Greeks, IV
- Technical: breakout, swing trade, day trade, chart patterns
- Crypto: BTC/ETH trades, funding rates, perpetuals
- Risk: position sizing, R:R, P&L

---

## Macro Sources

| Indicator | URL |
|-----------|-----|
| SOFR | https://fred.stlouisfed.org/series/SOFR |
| 2Y Yield | https://www.tradingview.com/symbols/TVC-US02Y/ |
| 10Y Yield | https://www.tradingview.com/symbols/TVC-US10Y/ |
| Fed Cut Odds | https://polymarket.com/event/fed-decision-in-december |
| PCE Nowcast | https://www.clevelandfed.org/indicators-and-data/inflation-nowcasting |
| RRP | https://www.newyorkfed.org/markets/desk-operations/reverse-repo |
| TGA | https://fiscaldata.treasury.gov/datasets/daily-treasury-statement/operating-cash-balance |
| News Flow | https://www.financialjuice.com/home |

---

## Design Philosophy

Built on best practices from the most successful open-source Claude trading skills, plus your custom macro framework:

1. **Macro first** — Check the 8-indicator dashboard before any directional trade
2. **News as signal** — FinancialJuice confirms or contradicts macro; cross-reference always
3. **Risk-first** — Position sizing before entry, never skip the stop
4. **IV-aware options** — Never buy expensive options; never ignore IV crush risk
5. **Crypto-specific rules** — Wider stops, conservative leverage, isolated margin always

---

## Disclaimer

This skill provides analysis for **educational purposes only**. It is not financial advice. Trading involves significant risk of loss. Always do your own research and manage your own risk.

---

## Author

Built for Justin — intraday options, Mag 7 weeklies, QQQ 0DTE, short-term equities & crypto.
