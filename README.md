# LLM Wiki for Equity Market Research

A working example of [Andrej Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/1dd0294ef9567971c1e4348a90d69285) applied to **stock market research**.

This repo demonstrates how an LLM can build and maintain a persistent, interlinked knowledge base for equity research — tracking 20 stocks across semiconductors, AI/cloud, healthcare, and quantum computing.

---

## What's Inside

| Folder | Contents |
|--------|----------|
| `raw/` | Source data: 120+ files with stock profiles, quarterly earnings, and news for 20 tickers |
| `wiki/` | AI-generated knowledge base: 40+ interlinked pages with summaries, concepts, and analysis |
| `CLAUDE.md` | The schema file that instructs the AI how to maintain the wiki |
| `.github/skills/` | Stock research ingestion skill for automated data fetching |

### Stocks Covered (20 tickers)

**Semiconductors**: NVDA, AMD, AVGO, MU, AXTI, MXL  
**Optical/Networking**: CIEN, COHR, GLW, VIAV  
**Big Tech**: MSFT, GOOG, META, TSLA  
**AI/Software**: PLTR, RDDT  
**Healthcare**: HIMS, TEM  
**Quantum Computing**: QBTS  
**Storage**: SNDK

---

## Original Repository

This is a customized example built on top of the original LLM Wiki template:

**Original repo**: https://github.com/balukosuri/llm-wiki-karpathy

Clone that repo if you want a clean starting point for any domain — not just equities.

---

## How It Works

Instead of RAG (re-searching documents on every query), the LLM:

1. **Reads sources once** and extracts key information
2. **Creates structured wiki pages** with proper metadata
3. **Cross-references everything** with `[[wikilinks]]`
4. **Maintains a glossary** of domain terms
5. **Logs all activity** for auditability

The wiki compounds over time — each ingest makes it richer.

---

## Using GitHub Copilot (Instead of Cursor)

The [original LLM Wiki repo](https://github.com/balukosuri/llm-wiki-karpathy) was designed for **Cursor**, which automatically reads the `CLAUDE.md` schema file.

This example uses **GitHub Copilot** in VS Code instead. Here's how to set it up:

### Setup

1. **Install VS Code** with the [GitHub Copilot](https://github.com/features/copilot) extension
2. **Enable Copilot Chat** — make sure you have access to the chat panel
3. **Open this repo** in VS Code

### How to Use Copilot Agent Mode

For the best experience, use **Copilot Agent mode** which can read files, run commands, and make edits autonomously:

1. Open **Copilot Chat** (click the Copilot icon or press `Cmd+Shift+I`)
2. Switch to **Agent mode** (look for the agent/edit toggle at the top of the chat panel)
3. The agent will automatically read `CLAUDE.md` and understand the wiki structure

### Key Differences from Cursor

| Feature | Cursor | GitHub Copilot |
|---------|--------|----------------|
| Schema file | Auto-reads `CLAUDE.md` | Agent mode reads it automatically |
| Multi-file edits | Built-in | Use Agent mode |
| Web scraping | Direct | Agent can use `fetch_webpage` tool |
| Context | Automatic | Use `@workspace` for full context |

### Tips for Copilot

- **Use Agent mode** for multi-step operations like "ingest" or "Research [TICKER]"
- **Say `@workspace`** if you want Copilot to search across files
- **The schema file `CLAUDE.md`** tells the agent exactly how to maintain the wiki — it reads this automatically

---

## Getting Started

```bash
git clone https://github.com/junwenwu/llm_wiki_example_for_equity_market.git
cd llm_wiki_example_for_equity_market
```

Open in VS Code, enable Copilot Agent mode, and start chatting.

**Optional**: Open the same folder in [Obsidian](https://obsidian.md/) to browse the wiki with graph view.

### Commands

| Command | What it does |
|---------|--------------|
| `ingest` | Process new source documents into wiki pages |
| `Research [TICKER]` | Fetch fresh data from Seeking Alpha and update wiki |
| `lint the wiki` | Check for contradictions, orphans, stale content |
| `query: [question]` | Ask anything — AI answers from wiki and can save the response |

---

## Example Session

**You**: Research NVDA

**AI**: 
1. Scrapes Seeking Alpha for latest data
2. Creates/updates `raw/NVDA/` files
3. Updates `wiki/sources/nvda.md` 
4. Logs activity in `wiki/log.md`

**You**: What are the highest margin stocks in my watchlist?

**AI**: (reads wiki, synthesizes answer with citations)
> Based on the watchlist analysis, the highest margin stocks are...
> Should I save this as a wiki analysis page?

---

## Repo Structure

```
llm_wiki_example_for_equity_market/
├── CLAUDE.md              # Schema — AI's operating manual
├── README.md              # This file
│
├── raw/                   # Source data (AI reads, you can add)
│   ├── NVDA/              # Stock folder with profile, earnings, news
│   ├── MSFT/
│   └── ... (20 tickers)
│
├── wiki/                  # AI-generated knowledge base
│   ├── index.md           # Master catalog
│   ├── overview.md        # Big-picture synthesis
│   ├── glossary.md        # Terms and definitions
│   ├── log.md             # Activity log
│   ├── sources/           # Stock profile pages (20)
│   ├── concepts/          # Sector pages (semiconductors, healthcare, quantum)
│   └── analyses/          # Comparison tables, watchlist analysis
│
└── .github/skills/        # Stock research automation skill
```

---

## Customization

Edit `CLAUDE.md` to change:
- Page types (add "ETF", "Sector Report", etc.)
- Ingest workflow (skip discussion, auto-save queries)
- Output formats (comparison tables, earnings summaries)

Add new stocks by saying:
> Research [NEW_TICKER]

---

## Credits

- **LLM Wiki Pattern**: [Andrej Karpathy](https://gist.github.com/karpathy/1dd0294ef9567971c1e4348a90d69285)
- **Original Template**: [balukosuri/llm-wiki-karpathy](https://github.com/balukosuri/llm-wiki-karpathy)
- **Data Source**: [Seeking Alpha](https://seekingalpha.com)

---

## License

MIT — use freely for your own research projects.
