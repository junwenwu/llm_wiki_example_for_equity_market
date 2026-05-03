# Stock Research Ingestion

A skill for building comprehensive stock research knowledge bases by scraping earnings data and market news from financial websites.

---

## Overview

This skill automates the collection and organization of stock research data into a structured `raw/{TICKER}/` folder hierarchy. It scrapes:

- **Quarterly earnings reports** (most recent 4 quarters for TTM analysis)
- **Market news summaries** (last 10 days of headlines)
- **Company profiles** (fundamentals, metrics, sector info)

---

## Quick Start

### 1. Create a stock folder

```bash
mkdir -p raw/{TICKER}
```

### 2. Scrape earnings data

Ask the AI to fetch earnings from Seeking Alpha:

```
Scrape the last 4 quarters of earnings for INTC
```

The AI will:
1. Fetch earnings data from `seekingalpha.com/symbol/{TICKER}/earnings`
2. Extract key metrics (Revenue, EPS, YoY growth, guidance)
3. Create 4 earnings files with YAML frontmatter

### 3. Scrape market news

```
Scrape the last 10 days of news for INTC
```

The AI will:
1. Fetch news from `seekingalpha.com/symbol/{TICKER}/news`
2. Categorize headlines by theme (earnings, analyst activity, product news)
3. Create a news summary file

---

## Folder Structure

After running the skill, each stock folder contains:

```
raw/
  INTC/
    company-profile.md           # Fundamentals, sector, market data
    earnings-q1-fy26.md          # Most recent quarter
    earnings-q4-fy25.md          # Q-1
    earnings-q3-fy25.md          # Q-2
    earnings-q2-fy25.md          # Q-3 (oldest in TTM window)
    news-2026-05-02.md           # Latest news scrape
```

**Key requirement:** Always maintain **4 earnings files** per ticker for trailing twelve months (TTM) analysis.

---

## File Naming Conventions

### Earnings Files

Use fiscal year format based on the company's fiscal calendar:

| Company | Ticker | FY End | Q2 FY26 = |
|---------|--------|--------|-----------|
| Apple | AAPL | September | Jan-Mar 2026 |
| Intel | INTC | December | Apr-Jun 2026 |
| CrowdStrike | CRWD | January | May-Jul 2025 |
| Cloudflare | NET | December | Apr-Jun 2026 |

**Format:** `earnings-q{Q}-fy{YY}.md` (e.g., `earnings-q1-fy26.md`)

### News Files

**Format:** `news-{YYYY-MM-DD}.md` (date of scrape)

---

## Example: INTC Knowledge Base

### Step 1: Create folder
```bash
mkdir -p raw/INTC
```

### Step 2: Scrape earnings (4 quarters)
```
Use fetch_webpage to get Intel earnings from Seeking Alpha and create earnings files for the last 4 quarters
```

**Result:**
| File | Period | Revenue | EPS |
|------|--------|---------|-----|
| earnings-q2-fy25.md | Apr-Jun 2025 | $12.86B | -$0.67 |
| earnings-q3-fy25.md | Jul-Sep 2025 | $13.65B | $0.90 |
| earnings-q4-fy25.md | Oct-Dec 2025 | $13.67B | -$0.12 |
| earnings-q1-fy26.md | Jan-Mar 2026 | $13.58B | -$0.73 |

### Step 3: Scrape news
```
Fetch the last 10 days of Intel news from Seeking Alpha
```

**Result:** `news-2026-05-02.md` with categorized headlines:
- Q1 earnings blowout analysis
- Analyst upgrades (HSBC, BNP Paribas)
- Tesla/SpaceX 14A partnership announcement

---

## Data Sources

| Source | URL Pattern | Notes |
|--------|-------------|-------|
| **Seeking Alpha (Primary)** | `seekingalpha.com/symbol/{TICKER}/earnings` | Earnings data, estimates, surprises |
| **Seeking Alpha News** | `seekingalpha.com/symbol/{TICKER}/news` | Headlines, analyst activity |
| **MarketWatch** | `marketwatch.com/investing/stock/{ticker}/financials/income/quarter` | Quarterly financials |

**Note:** Yahoo Finance is blocked by CSP policy — use Seeking Alpha instead.

---

## Earnings File Template

```markdown
---
title: "{TICKER} Q{Q} FY{YY} Earnings"
type: source
created: {YYYY-MM-DD}
updated: {YYYY-MM-DD}
sources: [seekingalpha.com]
tags: [earnings, {ticker-lowercase}]
---

# {COMPANY} Q{Q} FY{YY} Earnings Summary

**Report Date:** {Month DD, YYYY}
**Stock Price:** ${price} ({+/-}% on report day)

---

## Key Metrics

| Metric | Result | Estimate | Beat/Miss |
|--------|--------|----------|-----------|
| Revenue | $X.XXB | $X.XXB | +$XXM |
| GAAP EPS | $X.XX | $X.XX | +$0.XX |
| Non-GAAP EPS | $X.XX | $X.XX | +$0.XX |

## Guidance

- Q{N+1} Revenue: $X.XX - $X.XXB
- Q{N+1} EPS: $X.XX - $X.XX

## Key Highlights

1. {Highlight 1}
2. {Highlight 2}
3. {Highlight 3}

---

## Related Pages

- [[{ticker}-overview]]
- [[earnings-analysis-guide]]
```

---

## News Summary Template

```markdown
# {COMPANY} ({TICKER}) Market News Summary

**Period:** {Start Date} - {End Date}
**Source:** Seeking Alpha
**Stock Price:** ${price} ({+/-}% on {date})

---

## Major Headlines (Last 10 Days)

### {Category} ({Date})

- **{Headline 1}**
- **{Headline 2}**

---

## Key Themes

1. **{Theme 1}** - {Brief explanation}
2. **{Theme 2}** - {Brief explanation}

---

## Analyst Activity

| Firm | Action | Rating | Notes |
|------|--------|--------|-------|
| {Firm} | Upgrade | Buy | {Notes} |

---

## Sentiment Summary

| Aspect | Sentiment |
|--------|-----------|
| Earnings | Bullish/Neutral/Bearish |
| Growth | Bullish/Neutral/Bearish |
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Yahoo Finance blocked | Use Seeking Alpha instead |
| Fiscal year confusion | Check company's FY end month (see table above) |
| Missing historical data | Try MarketWatch quarterly financials |
| Rate limiting | Add delay between requests |

---

## Quality Checklist

Before considering a stock folder complete:

- [ ] 4 earnings files covering TTM (trailing twelve months)
- [ ] Each earnings file has YAML frontmatter
- [ ] Key metrics include beat/miss vs estimates
- [ ] Guidance for next quarter included (if available)
- [ ] News file dated within last 10 days
- [ ] Sources cited in frontmatter
- [ ] Related pages linked

---

## Related Files

- [SKILL.md](.github/skills/stock-research-ingestion/SKILL.md) — Full skill specification
- [CLAUDE.md](CLAUDE.md) — Wiki schema and workflows
- [earnings-analysis-guide.md](raw/earnings-analysis-guide.md) — How to analyze earnings data
