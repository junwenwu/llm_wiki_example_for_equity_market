# Stock Research Ingestion Skill

Build raw source files for stock research by scraping earnings data and market news from financial websites.

---

## Overview

This skill documents the workflow for collecting and organizing stock research data into the `raw/{TICKER}/` folder structure. It covers earnings reports, news summaries, and file naming conventions.

---

## Directory Structure

```
raw/
  {TICKER}/
    profile.md                         # Company profile and business description
    earnings-q{Q}-fy{YY}.md            # Most recent 4 quarterly earnings (TTM)
    earnings-q{Q-1}-fy{YY}.md          # e.g., earnings-q1-fy26.md
    earnings-q{Q-2}-fy{YY}.md          # e.g., earnings-q4-fy25.md
    earnings-q{Q-3}-fy{YY}.md          # e.g., earnings-q3-fy25.md
    news-{YYYY-MM-DD}.md               # Market news summaries
```

**Note:** Always maintain 4 earnings files per ticker for TTM analysis.

---

## Workflow Steps

### 1. Create Stock Folder

```bash
mkdir -p raw/{TICKER}
```

### 2. Scrape Earnings Data

**Primary Source:** Seeking Alpha (`seekingalpha.com/symbol/{TICKER}/earnings`)

**Requirement:** Scrape the **most recent 4 quarterly earnings reports** to establish a trailing twelve months (TTM) baseline and enable trend analysis.

**Steps:**
1. Fetch the earnings page using `fetch_webpage` tool
2. Extract key metrics: Revenue, EPS (GAAP/Non-GAAP), YoY growth
3. Note guidance for next quarter if provided
4. Create earnings file with YAML frontmatter
5. Repeat for each of the last 4 quarters (Q-1, Q-2, Q-3, Q-4)

**File Naming Convention:**
- Fiscal year format: `earnings-q{Q}-fy{YY}.md` (e.g., `earnings-q2-fy26.md`)
- Calendar year format: `earnings-q{Q}-{YYYY}.md` (e.g., `earnings-q3-2025.md`)

**Fiscal Year Mappings (Important):**
| Company | Ticker | FY End | Example: Q2 FY26 = |
|---------|--------|--------|---------------------|
| Apple | AAPL | September | Jan-Mar 2026 |
| CrowdStrike | CRWD | January | May-Jul 2025 |
| Cloudflare | NET | December | Apr-Jun 2026 |
| AppLovin | APP | December | Apr-Jun 2026 |

### 3. Scrape Market News

**Primary Source:** Seeking Alpha (`seekingalpha.com/symbol/{TICKER}/news`)

**Fallback Note:** Yahoo Finance is blocked by CSP policy - use Seeking Alpha instead.

**Steps:**
1. Fetch news page using `fetch_webpage` tool with query for "recent news headlines"
2. Extract headlines from the last 10 days
3. Categorize by theme (earnings, analyst activity, product news, regulatory)
4. Create news summary file

**File Naming:** `news-{YYYY-MM-DD}.md` (date of scrape)

### 4. Scrape Company Profile

**Primary Source:** Seeking Alpha (`seekingalpha.com/symbol/{TICKER}`)

**Alternative Sources:**
- Yahoo Finance (`finance.yahoo.com/quote/{TICKER}/profile`)
- MarketWatch (`marketwatch.com/investing/stock/{TICKER}/company-profile`)

**Steps:**
1. Fetch the company profile page using `fetch_webpage` tool
2. Extract the business description (what the company does)
3. Note key details: sector, industry, headquarters, employees, founded
4. Capture key products/services and market position
5. Create profile file with YAML frontmatter

**File Naming:** `profile.md` (one per ticker, updated as needed)

### 5. Create Company Profile File

**Template:**

```markdown
---
title: "{COMPANY} ({TICKER}) Company Profile"
type: source
created: {YYYY-MM-DD}
updated: {YYYY-MM-DD}
sources: [seekingalpha.com]
tags: [profile, {ticker-lowercase}, {sector-lowercase}]
---

# {COMPANY} ({TICKER}) Company Profile

**Sector:** {Sector}
**Industry:** {Industry}
**Headquarters:** {City, State/Country}
**Founded:** {Year}
**Employees:** {Number}
**Website:** {URL}

---

## Business Description

{Raw description from source - what the company does, its products/services, and market position}

---

## Key Products & Services

1. **{Product/Service 1}** - {Brief description}
2. **{Product/Service 2}** - {Brief description}
3. **{Product/Service 3}** - {Brief description}

---

## Market Position

- **Primary Markets:** {Markets served}
- **Key Customers:** {Customer types or named customers}
- **Competitive Advantages:** {Moats, differentiators}

---

## Related Pages

- [[{ticker}-earnings-latest]]
- [[{ticker}-news-latest]]
```

### 6. Create Earnings File

**Template:**

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

## Management Commentary

> "{Notable quote from earnings call}"

---

## Related Pages

- [[{ticker}-overview]]
- [[earnings-analysis-guide]]
```

### 7. Create News Summary File

**Template:**

```markdown
# {COMPANY} ({TICKER}) Market News Summary

**Period:** {Start Date} - {End Date}
**Source:** Seeking Alpha
**Stock Price:** ${price} ({+/-}% on {date})

---

## Major Headlines (Last 10 Days)

### {Category 1} ({Date})

- **{Headline 1}**
- **{Headline 2}**
- {Supporting detail}

### {Category 2}

- **{Headline}**

---

## Key Themes

1. **{Theme 1}** - {Brief explanation}
2. **{Theme 2}** - {Brief explanation}

---

## Analyst Activity

| Firm | Action | Rating | Notes |
|------|--------|--------|-------|
| {Firm} | {Upgrade/Downgrade/Initiate} | {Rating} | {Notes} |

---

## Sentiment Summary

| Aspect | Sentiment |
|--------|-----------|
| Earnings | {Bullish/Neutral/Bearish} |
| Growth | {Bullish/Neutral/Bearish} |
| Valuation | {Bullish/Neutral/Bearish} |
```

---

## Tool Usage

### Fetching Web Pages

```
fetch_webpage:
  urls: ["https://seekingalpha.com/symbol/{TICKER}/earnings"]
  query: "earnings results revenue EPS guidance"
```

```
fetch_webpage:
  urls: ["https://seekingalpha.com/symbol/{TICKER}/news"]
  query: "recent news headlines last 10 days"
```

```
fetch_webpage:
  urls: ["https://seekingalpha.com/symbol/{TICKER}"]
  query: "company description business profile sector industry"
```

### Creating Files

```
create_file:
  filePath: raw/{TICKER}/earnings-q{Q}-fy{YY}.md
  content: {formatted markdown}
```

### Renaming Files (if needed)

```bash
mv raw/{TICKER}/old-name.md raw/{TICKER}/new-name.md
```

---

## Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| Yahoo Finance blocked by CSP | Use Seeking Alpha instead |
| Fiscal year confusion | Check company's FY end month (see mapping table) |
| Missing earnings data | Try Motley Fool or company IR page |
| Missing company profile | Try MarketWatch or company About page |
| Rate limiting | Add delay between requests |

---

## Quality Checklist

- [ ] File follows naming convention
- [ ] YAML frontmatter is complete
- [ ] Company profile includes business description
- [ ] Key metrics include beat/miss vs estimates
- [ ] Guidance for next quarter included (if available)
- [ ] Sources cited
- [ ] Related pages linked
- [ ] Date of scrape recorded

---

## Example: AAPL Q2 FY26 Workflow

1. **Fetch earnings:**
   ```
   fetch_webpage(urls=["https://seekingalpha.com/symbol/AAPL/earnings"], query="Q2 FY26 earnings revenue EPS")
   ```

2. **Extract data:**
   - Revenue: $111.18B (beat by $1.6B)
   - GAAP EPS: $2.01 (beat by $0.07)
   - iPhone revenue: +22% YoY
   - Dividend: raised 3.8% to $0.27
   - Buyback: $100B authorized

3. **Create file:**
   ```
   create_file(filePath="raw/AAPL/earnings-q2-fy26.md", content=...)
   ```

4. **Fetch news:**
   ```
   fetch_webpage(urls=["https://seekingalpha.com/symbol/AAPL/news"], query="recent headlines")
   ```

5. **Create news summary:**
   ```
   create_file(filePath="raw/AAPL/news-2026-05-02.md", content=...)
   ```

---

## Related Skills

- [[wiki-ingest]] - Processing raw files into wiki pages
- [[earnings-analysis]] - Analyzing earnings trends
