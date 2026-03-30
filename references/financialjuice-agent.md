# FinancialJuice News Agent

## Overview

FinancialJuice (https://www.financialjuice.com/home) is a professional real-time financial news squawk service. Justin has an active membership. This agent handles:

1. **Fetching news** for specific timeframes (NY time)
2. **Storing news by date** in a structured local log
3. **Summarizing** news into a clean brief
4. **Analyzing** news using trading skill context
5. **Making trade decisions** on specific stocks based on headlines

---

## How to Access FinancialJuice

FinancialJuice requires login. Use the Claude-in-Chrome MCP to access the live feed:

1. Navigate to https://www.financialjuice.com/home
2. If not logged in, wait for Justin to log in (do not handle credentials)
3. Once on the feed page, read the live squawk headlines using `read_page` or `get_page_text`
4. Filter by time using the page's built-in time filter or by parsing timestamps in the feed

**Important**: Always read the page fresh — don't rely on cached content. Financial news is time-sensitive.

---

## Timeframe Fetch Commands

When Justin asks for news, these are the standard timeframe requests:

| Command | Meaning | NY Time Window |
|---------|---------|----------------|
| "morning news" / "pre-market" | Pre-market headlines | 4:00 AM – 9:30 AM ET |
| "open news" / "market open" | First hour of trading | 9:30 AM – 10:30 AM ET |
| "today so far" | Full day to current time | Market open to now |
| "last hour" | Most recent 60 minutes | Current time - 1 hour |
| "afternoon news" | Afternoon session | 1:00 PM – 4:00 PM ET |
| "after hours" | Post-market | 4:00 PM – 8:00 PM ET |
| Custom time (e.g., "since 2pm") | User-specified | As requested |

---

## News Storage Format

Store fetched news to: `/sessions/modest-relaxed-davinci/mnt/outputs/news-log/`

File naming convention: `YYYY-MM-DD_HH-MM_financialjuice.md`

Example: `2026-03-29_09-30_financialjuice.md`

### Storage Template

```markdown
# FinancialJuice News Log
**Date**: YYYY-MM-DD
**Fetched at**: HH:MM ET
**Timeframe**: [e.g., Pre-market 4:00–9:30 AM ET]

---

## Headlines (newest first)

| Time (ET) | Headline | Category |
|-----------|----------|----------|
| 09:28 | [headline text] | [Fed / Macro / Earnings / Geopolitical / Sector] |
| 09:15 | [headline text] | [category] |
...

---

## Raw Count: [N] headlines in timeframe
```

---

## News Summary Format

When summarizing news, produce this format:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEWS BRIEF — [TIMEFRAME] | [DATE] [TIME] ET
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

MACRO / FED
• [Key macro headline]
• [Fed commentary if any]

EARNINGS / CORPORATE
• [Any earnings reports or guidance]

GEOPOLITICAL / SECTOR
• [Any relevant geo or sector news]

TONE: [Dovish / Hawkish / Mixed / Risk-On / Risk-Off]
MARKET IMPACT: [Brief 1-sentence assessment]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## News Analysis Framework

After fetching and summarizing news, apply this analysis:

### Step 1: Classify Each Headline

| Headline Type | Typical Market Impact |
|---|---|
| Hot CPI / PPI / PCE | Bearish (hawkish repricing) |
| Cool CPI / PPI / PCE | Bullish (rate cut repricing) |
| Strong NFP / Jobs | Bearish (Fed keeps rates high) |
| Weak NFP / Jobs | Bullish (Fed has room to cut) |
| Fed hawkish speak | Bearish |
| Fed dovish speak / cut signal | Bullish |
| Earnings beat + guidance raise | Bullish for stock + sector |
| Earnings miss / guidance cut | Bearish for stock + sector |
| Geopolitical escalation | Bearish (risk-off, VIX up) |
| Geopolitical de-escalation | Bullish (risk-on) |
| China stimulus / risk | Mixed (AAPL, TSLA sensitive) |
| AI capex announcements | Bullish (NVDA, MSFT, GOOGL) |

### Step 2: Cross-Reference with Macro Dashboard

After classifying headlines, check against the macro dashboard scores:
- Does news **confirm** the macro bias? → Higher conviction trade
- Does news **contradict** the macro bias? → Reduce size, wait for clarity
- Is news **company-specific** with no macro overlap? → Isolated stock trade only

### Step 3: Stock-Specific Impact Assessment

When Justin asks about a specific stock, produce this:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEWS IMPACT ANALYSIS — [TICKER]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

RELEVANT HEADLINES:
• [Time] [Headline 1]
• [Time] [Headline 2]

DIRECT IMPACT: [High / Medium / Low / None]
DIRECTION: [Bullish / Bearish / Neutral]
WHY: [1–2 sentences connecting the news to the stock's fundamentals]

MACRO ALIGNMENT: [Aligned / Contradicts / Independent]

TRADE IMPLICATION:
  Bias: [Calls / Puts / No trade]
  Timeframe: [0DTE / weekly / skip]
  Key level: [price level to watch given this news]
  Risk: [what would invalidate this news-driven thesis]

CONFIDENCE: [High / Medium / Low]
Note: This is analysis only, not financial advice.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Trigger Phrases

This agent activates when Justin says:
- "get me the news", "what's on the squawk", "FinancialJuice"
- "morning brief", "pre-market news", "open news"
- "what happened at [time]", "news since [time]"
- "any news on [ticker]", "what's moving [ticker]"
- "summarize today's news", "store the news"
- "what's the tape saying", "news flow"

---

## Important Notes

- **Never trade solely on news without checking macro dashboard first** — a single headline can be noise; the macro context determines whether it's signal.
- **Speed matters**: In intraday trading, news > 15 minutes old is stale for 0DTE decisions. Always note the headline timestamp.
- **Fade vs. follow**: Strong initial news-driven moves (first 5 min) often reverse. Wait for confirmation before entering in the direction of news.
- **Quiet tape = stay small**: If FinancialJuice is showing no major headlines, the market is trading on technicals alone → reduce directional conviction.
