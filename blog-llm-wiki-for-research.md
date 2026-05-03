# Building a Personal Research Wiki with LLMs: From Stock Market Analysis to Any Domain

*How to turn AI coding assistants into knowledge compilers using the LLM Wiki pattern*

---

## The Problem with AI-Assisted Research

Modern AI assistants have memory features — Copilot remembers your preferences, Claude can recall past conversations. But these memories are shallow: snippets and preferences, not structured knowledge.

When you research a stock, the AI gives you an answer. But where does that analysis go? It sits in chat history, unstructured and hard to find. You can't cross-reference it with other research. You can't ask "how does this compare to what I learned about AMD last month?"

**What if your AI could build and maintain a structured, cross-referenced knowledge base?**

That's the idea behind the **LLM Wiki pattern**, popularized by Andrej Karpathy. Instead of treating AI as a stateless Q&A bot, you give it a schema file that teaches it to maintain a wiki. Every research session compounds into lasting knowledge.

---

## The Architecture: Three Simple Layers

```
┌─────────────────────────────────────────────────────────┐
│                    CLAUDE.md (Schema)                   │
│         "How to maintain the wiki"                      │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│                      raw/                               │
│         Source documents (AI reads, never writes)       │
│         • earnings reports                              │
│         • news articles                                 │
│         • research papers                               │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│                      wiki/                              │
│         Synthesized knowledge (AI owns this)            │
│         • sources/     → summaries of raw docs          │
│         • concepts/    → domain ideas                   │
│         • analyses/    → comparisons, insights          │
│         • glossary.md  → terminology                    │
│         • index.md     → catalog of all pages           │
└─────────────────────────────────────────────────────────┘
```

The magic is in **separation of concerns**:
- `raw/` is your source of truth — immutable research inputs
- `wiki/` is the AI's workspace — structured, cross-referenced outputs
- `CLAUDE.md` is the operating manual — tells the AI exactly how to maintain everything

---

## Real Example: Equity Market Research

I built an LLM Wiki to track 25 tech stocks across semiconductors, AI, cloud, and optical networking. Here's how it works in practice.

### The Research Command

I open VS Code with GitHub Copilot in Agent mode and type:

```
Research NVDA
```

The agent:
1. Scrapes Seeking Alpha for the latest earnings data
2. Pulls recent news headlines
3. **If new ticker**: Creates `raw/NVDA/profile.md` and wiki source page
4. **If existing**: Updates files with new data, preserves historical records
5. Creates/updates earnings files (TTM: 4 quarters) and dated news files (`news-2026-05-02.md`)
6. Updates `wiki/sources/nvda.md` — merges new data with existing synthesis
7. Updates `wiki/index.md` — refreshes market cap and summary
8. Appends to `wiki/log.md` — logs what changed

**One command. Multiple files. Compounding knowledge.**

### What the Wiki Actually Looks Like

Here's the real structure after researching 25 stocks:

```
wiki/
├── index.md                    # Catalog of all pages with market caps
├── glossary.md                 # P/E ratio, EPS, HBM, TAM, etc.
├── overview.md                 # Market themes, sector trends
├── log.md                      # Audit trail of all research
├── sources/
│   ├── nvda.md                 # NVIDIA — $4.82T, AI accelerators
│   ├── amd.md                  # AMD — $588B, CPU/GPU
│   ├── msft.md                 # Microsoft — $3.08T, Azure/Copilot
│   ├── cohr.md                 # Coherent — $64B, optical transceivers
│   ├── earnings-analysis-guide.md
│   └── ... (29 source pages)
├── concepts/
│   ├── semiconductors.md       # Chip industry dynamics
│   ├── quantum-computing.md    # Emerging compute paradigm
│   ├── healthcare.md           # Sector analysis
│   ├── fundamental-analysis.md # Valuation methods
│   ├── technical-analysis.md   # Chart patterns
│   └── ... (7 concept pages)
└── analyses/
    └── stock-watchlist-may-2026.md  # Cross-stock comparison
```

---

## The Secret Weapon: SKILL.md for Automation

The schema (`CLAUDE.md`) tells the AI *what* to do. A **SKILL.md** file tells it *how* to do domain-specific tasks.

Here's a snippet from our stock research skill:

```markdown
# Stock Research Ingestion Skill

## Workflow Steps

### 1. Scrape Earnings Data
**Source:** seekingalpha.com/symbol/{TICKER}/earnings

**Steps:**
1. Fetch the earnings page using `fetch_webpage` tool
2. Extract: Revenue, EPS, YoY growth, guidance
3. Create file: `raw/{TICKER}/earnings-q{Q}-fy{YY}.md`
4. Repeat for last 4 quarters (TTM baseline)

### 2. Scrape Market News
**Source:** seekingalpha.com/symbol/{TICKER}/news

**Steps:**
1. Fetch news headlines from last 10 days
2. Categorize by theme
3. Create file: `raw/{TICKER}/news-{YYYY-MM-DD}.md`
```

**Why this matters:** The skill encodes your research workflow. The AI follows it consistently every time, even across sessions.

---

## Using GitHub Copilot as Your Research Agent

The original LLM Wiki pattern was built for Cursor, but **GitHub Copilot works just as well** with Agent mode.

### Setup

1. Install VS Code + GitHub Copilot extension
2. Open the wiki repo
3. Switch to **Agent mode** in Copilot Chat

### Key Commands

| Command | What Happens |
|---------|--------------|
| `Research {TICKER}` | Scrapes data, creates raw files, updates wiki |
| `ingest raw/{TICKER}/earnings.md` | Synthesizes a source into wiki pages |
| `Compare NVDA vs AMD` | Creates an analysis page |
| `lint the wiki` | Finds contradictions, stale data, orphan pages |
| `What's the bull case for MSFT?` | Queries the wiki, cites pages |

### The Compounding Effect

After researching 25 stocks, the wiki has real data like:

- **NVDA**: $4.82T market cap, Q4 FY26 revenue $68.13B (beat by $1.90B), 80%+ AI training share
- **MSFT**: $3.08T, Azure +40% YoY, Q4 FY26 guidance $86.7-87.8B
- **COHR**: $64B, #1 optical transceiver provider for AI data centers

Now I can ask:

> "Which stocks have the strongest earnings momentum this quarter?"

The agent doesn't re-scrape the internet. It reads `wiki/index.md`, finds relevant source pages, and synthesizes an answer **from knowledge it already compiled**.

---

## Adapting the Pattern to Other Domains

The LLM Wiki pattern isn't just for stocks. The architecture works for any research domain:

### Legal Research
```
raw/
  cases/                # Court opinions, statutes
  depositions/          # Witness transcripts
wiki/
  sources/              # Case summaries
  concepts/             # Legal doctrines
  analyses/             # Argument synthesis
```

**SKILL.md:** How to cite cases, extract holdings, identify precedents.

### Medical Literature
```
raw/
  papers/               # PubMed articles
  guidelines/           # Clinical protocols
wiki/
  sources/              # Paper summaries
  concepts/             # Disease mechanisms
  analyses/             # Treatment comparisons
```

**SKILL.md:** How to extract study design, sample size, outcomes, limitations.

### Competitive Intelligence
```
raw/
  companies/            # 10-K filings, press releases
  market-reports/       # Industry analyses
wiki/
  sources/              # Company profiles
  concepts/             # Market dynamics
  analyses/             # Competitive positioning
```

**SKILL.md:** How to extract revenue segments, growth rates, strategic initiatives.

### Technical Documentation
```
raw/
  specs/                # API docs, RFCs
  code/                 # Source files
wiki/
  sources/              # Spec summaries
  concepts/             # Architecture patterns
  features/             # Feature documentation
```

**SKILL.md:** How to document APIs, extract code examples, link to implementations.

---

## Key Principles for Your Own LLM Wiki

### 1. Separation of Source and Synthesis
Never let the AI modify `raw/`. It's your source of truth. The wiki is the AI's workspace.

### 2. Schema as Operating Manual
`CLAUDE.md` isn't a prompt — it's an instruction manual the AI reads at the start of every session. Be specific about workflows, file formats, and conventions.

### 3. Skills Encode Domain Expertise
SKILL.md files capture *how* you research. Scraping patterns, data extraction rules, categorization schemes. The AI follows these consistently.

### 4. Cross-Reference Everything
The power is in connections. When you research NVDA, the wiki should link to AMD, AVGO, and the "AI Infrastructure" concept page.

### 5. Log Everything
`wiki/log.md` is your audit trail. You can see exactly what changed and when.

### 6. Lint Regularly
Ask the AI to check for contradictions, stale data, and missing links. Knowledge bases decay without maintenance.

---

## Getting Started

1. **Clone the template:**
   ```bash
   git clone https://github.com/junwenwu/llm_wiki_example_for_equity_market.git
   ```

2. **Open in VS Code** with GitHub Copilot

3. **Switch to Agent mode** in Copilot Chat

4. **Try a command:**
   ```
   Research AAPL
   ```

5. **Browse the wiki** in Obsidian (optional but recommended for graph view)

---

## Conclusion

The LLM Wiki pattern transforms AI from a stateless assistant into a **knowledge compiler**. Every research session builds on the last. Your wiki compounds over time.

Whether you're tracking stocks, researching legal cases, reviewing medical literature, or documenting a codebase — the pattern is the same:

- **Raw sources** you collect
- **A wiki** the AI maintains
- **A schema** that teaches it how
- **Skills** that encode your workflows

Start with the equity market example, then adapt it to your domain.

---

*This wiki was built using [GitHub Copilot](https://github.com/features/copilot) in VS Code, following the LLM Wiki pattern from [Andrej Karpathy](https://x.com/karpathy) and adapted from [balukosuri/llm-wiki-karpathy](https://github.com/balukosuri/llm-wiki-karpathy).*
