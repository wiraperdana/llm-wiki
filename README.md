# LLM Wiki — Personal Knowledge Base Template

A persistent, compounding knowledge base maintained by an LLM agent. Sources go in `raw/`; the LLM builds and maintains interlinked wiki pages in `wiki/`.

**This is not RAG.** The LLM reads sources once, distills them into structured pages, and keeps those pages current as new sources arrive. The wiki itself is the artifact — not the raw pile.

---

## How to use this

1. **Open the folder in Obsidian.** Install these plugins for the full experience:
   - **Dataview** — query the YAML frontmatter on wiki pages.
   - **Marp** — preview slide decks in `wiki/slides/`.
   - **Obsidian Web Clipper** (browser extension) — clips articles into `raw/` as markdown.

2. **Open your LLM agent** (Claude Cowork, Claude Code, Codex, etc.) in this folder. The agent will read [`AGENTS.md`](./AGENTS.md) — which contains all conventions — automatically.

3. **Add a source** — drop any file (article, PDF, transcript, note) into `raw/` and tell the agent *"ingest this"*. The agent will summarize, file, and cross-reference it.

4. **Ask questions** — *"what does the wiki say about X?"*, *"compare X and Y"*, *"what do I still not know about Z?"* Answers worth keeping are filed as new pages under `wiki/syntheses/`.

5. **Lint periodically** — every few weeks, tell the agent *"lint the wiki"*. It will flag contradictions, stale claims, orphans, and suggestions to investigate.

---

## Directory map

```
AGENTS.md        ← schema + operating manual for the LLM (read this first)
CLAUDE.md        ← pointer for Claude-specific runtimes → AGENTS.md
README.md        ← you are here
index.md         ← catalog of every page in the wiki (LLM-maintained)
log.md           ← chronological log of all ingests, queries, lints

raw/             ← immutable sources (you add, LLM reads)
  └── assets/    ← downloaded images from clipped articles

wiki/            ← LLM-owned markdown
  ├── entities/     — people, orgs, products, places
  ├── concepts/     — ideas, frameworks, theories
  ├── sources/      — one page per ingested source (summary + takeaways)
  ├── syntheses/    — analyses, comparisons, theses, lint reports
  ├── slides/       — Marp decks built from wiki content
  └── templates/    — blank templates for each page type
```

---

## Division of labor

| You do | The LLM does |
| --- | --- |
| Curate sources (what goes in `raw/`) | Read and summarize sources |
| Ask questions & direct attention | Create, update, cross-link pages |
| Review updates in Obsidian | Keep `index.md` & `log.md` current |
| Decide when something is "true" | Flag contradictions between sources |
| Update `AGENTS.md` with new conventions | Maintain consistency across pages |

The tedious part of a wiki is the bookkeeping. LLMs don't get bored. You focus on sourcing and thinking.

---

## Getting started

A seed source is already in place as an example:

- `raw/llm-wiki-pattern.md` — the pattern this wiki is built on.
- `wiki/sources/2026-04-17-llm-wiki-pattern.md` — example source page.
- `wiki/entities/vannevar-bush.md` — example entity page.
- `wiki/concepts/memex.md` + `wiki/concepts/llm-wiki-pattern.md` — example concept pages.
- `wiki/syntheses/rag-vs-llm-wiki.md` — example synthesis page.

Open the graph view in Obsidian to see how they link. Then **replace these example files with your own sources** and say *"ingest"* to your agent to start building your own wiki.

---

_Inspired by the [LLM Wiki pattern](./raw/llm-wiki-pattern.md). Maintained by LLMs, curated by humans._
