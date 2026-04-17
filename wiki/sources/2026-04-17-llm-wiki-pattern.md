---
title: The LLM Wiki pattern
type: source
created: 2026-04-17
updated: 2026-04-17
tags: [meta, llm, knowledge-management, rag, obsidian]
raw: ../../raw/llm-wiki-pattern.md
author: (unattributed)
published: 2026
medium: note
url:
status: stable
---

# The LLM Wiki pattern

> Raw source: [`raw/llm-wiki-pattern.md`](../../raw/llm-wiki-pattern.md)

## 1-line summary

Use an LLM as a disciplined wiki maintainer: it incrementally builds and updates a persistent, interlinked markdown knowledge base from raw sources, so knowledge compounds instead of being re-derived on every query.

## Key takeaways

- **Vs RAG:** Vanilla RAG (NotebookLM, ChatGPT file uploads) retrieves raw chunks and re-synthesizes on every query — nothing accumulates. The LLM Wiki pattern compiles knowledge **once** into structured pages and keeps those pages current.
- **Three layers:** `raw/` (immutable sources) → `wiki/` (LLM-owned markdown) → schema file (`AGENTS.md` / `CLAUDE.md`) that tells the LLM the conventions.
- **Three operations:** _ingest_ (file a new source and update everything it touches), _query_ (read index → read pages → answer with citations; optionally file the answer as a new synthesis page), _lint_ (periodic health check for contradictions, orphans, stale claims, missing cross-refs).
- **Two special files:** `index.md` (content catalog, LLM keeps current) and `log.md` (append-only chronological record, greppable via `## [YYYY-MM-DD] <op> | <title>`).
- **Human/LLM division:** The human curates sources, directs attention, asks questions. The LLM does all the grunt work — summarizing, cross-referencing, filing, bookkeeping. The wiki gets maintained because maintenance cost drops to near zero with an LLM.
- **Tooling:** Obsidian as the IDE (graph view, Dataview, Marp, Web Clipper). Optional [qmd](https://github.com/tobi/qmd) for local hybrid search once the wiki outgrows plain index lookups.
- **Lineage:** Spiritually descended from Vannevar Bush's 1945 Memex concept — the piece Bush couldn't solve was "who does the maintenance." LLMs are the answer.

## Detailed summary

The document frames most contemporary LLM+document workflows as stateless RAG, where the model rediscovers knowledge from scratch on every query. The alternative it proposes is a compounding artifact — an LLM-maintained wiki of interlinked markdown pages that sits between the user and the raw source pile. Every time a new source arrives, the LLM integrates it: updating entity pages, revising topic summaries, flagging contradictions, strengthening or weakening the evolving synthesis. Cross-references are already resolved, contradictions already flagged, synthesis already written — so queries are cheap and answers are grounded in accumulated context.

Operationally, the pattern decomposes into three repeatable flows. **Ingest** reads a new raw file, produces a source-summary page, touches 10–15 related pages, updates `index.md`, and appends to `log.md` — with the user optionally staying in the loop to steer emphasis. **Query** uses the index to locate relevant pages, reads them, and synthesizes an answer with wiki-link citations. The key trick: answers worth keeping are filed back into the wiki (typically as syntheses) so explorations compound the same way ingested sources do. **Lint** is a periodic audit for contradictions, stale claims, orphan pages, missing cross-refs, data gaps — the kind of cleanup humans always mean to do but never get around to.

The document is deliberately abstract: it communicates the pattern and leaves the implementation — directory layout, frontmatter schema, tooling choices — to be worked out between the user and their chosen agent. Everything is modular (image handling, slide decks, search CLIs, Dataview frontmatter, etc.) — pick what fits the domain, ignore the rest.

The author's ergonomic setup is Obsidian open next to the LLM agent: the LLM edits files in real time; the human browses the graph, follows links, reads updated pages. **"Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase."**

## Concepts introduced / updated

- [[wiki/concepts/llm-wiki-pattern]] — the pattern itself (the source's main contribution).

_Note: the source also mentions Vannevar Bush's Memex (1945) and Retrieval-Augmented Generation (RAG) as background context, but does not develop either enough to warrant dedicated wiki pages at this stage. If future sources explore these in depth, create pages for them then._

## Contradictions or tensions

None — this is the seed document, so there's nothing to contradict yet. Future sources on RAG, agentic memory, or knowledge management will be checked against it.

## Quotes worth keeping

> "The wiki is a persistent, compounding artifact. The cross-references are already there. The contradictions have already been flagged."

> "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase."

> "LLMs don't get bored, don't forget to update a cross-reference, and can touch 15 files in one pass."

## Open questions

- Who else is publicly running this pattern and at what scale? (Simon Willison, Andy Matuschak, Linus Lee, ...?)
- At what wiki size does the plain `index.md` approach break down — is ~hundreds of pages really the ceiling, or can it go further?
- How does this pattern degrade gracefully when the LLM behind it changes (model upgrades, provider swaps)? Is there drift in conventions?
- Non-textual sources — how to handle audio/video beyond transcription? Worth formalizing in `AGENTS.md` once a real use case arrives.
