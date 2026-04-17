---
title: LLM Wiki pattern
type: concept
created: 2026-04-17
updated: 2026-04-17
tags: [llm, knowledge-management, obsidian, meta]
aliases: [LLM wiki, LLM-maintained wiki]
sources: [[wiki/sources/2026-04-17-llm-wiki-pattern]]
status: draft
---

# LLM Wiki pattern

**Definition.** A workflow pattern in which an LLM agent incrementally builds and maintains a **persistent, interlinked markdown wiki** from a collection of immutable raw sources — so that knowledge compounds across sources and queries instead of being re-derived every time a question is asked.[[wiki/sources/2026-04-17-llm-wiki-pattern]]

This wiki is itself an instance of the pattern.

## Why it matters

It inverts the usual "LLM + documents" arrangement. Instead of the LLM acting as a query-time synthesizer over raw chunks (see [[wiki/concepts/rag]]), the LLM acts as a **maintainer**: every new source is absorbed into existing pages, every answer can be filed back, every cross-reference is kept current. The intellectual lineage runs back to [[wiki/entities/vannevar-bush]]'s [[wiki/concepts/memex]] — but the piece Bush couldn't solve, who maintains the structure, is handled by the LLM.[[wiki/sources/2026-04-17-llm-wiki-pattern]]

## Key properties

- **Three-layer architecture.** Immutable `raw/` sources, LLM-owned `wiki/` pages, a schema file (`AGENTS.md`/`CLAUDE.md`) that encodes conventions.[[wiki/sources/2026-04-17-llm-wiki-pattern]]
- **Three operations.** _Ingest_ integrates a new source (touches ~10–15 pages); _query_ reads index → pages → answers with citations, optionally filing the answer as a new synthesis; _lint_ is a periodic health-check for contradictions, orphans, stale claims.[[wiki/sources/2026-04-17-llm-wiki-pattern]]
- **Index + log.** `index.md` is a content catalog the LLM keeps current. `log.md` is append-only and greppable (`grep "^## \[" log.md | tail -5`).[[wiki/sources/2026-04-17-llm-wiki-pattern]]
- **Compounding artifact.** Cross-references, contradictions, synthesis — all pre-computed and kept current. Queries are cheap; answers are grounded in accumulated structure.[[wiki/sources/2026-04-17-llm-wiki-pattern]]
- **Human/LLM division.** Human curates sources, asks questions, directs emphasis. LLM does all writing, cross-referencing, bookkeeping — the tasks humans abandon wikis over.[[wiki/sources/2026-04-17-llm-wiki-pattern]]
- **Tooling-agnostic but Obsidian-friendly.** Plain markdown + YAML frontmatter. Obsidian Web Clipper, Dataview, Marp, graph view are natural complements.[[wiki/sources/2026-04-17-llm-wiki-pattern]]
- **Modular.** Image handling, slide decks, search CLIs — all optional. Start minimal, add pieces as the domain demands.[[wiki/sources/2026-04-17-llm-wiki-pattern]]

## Related concepts

- Ancestor: [[wiki/concepts/memex]] — same animating idea, pre-LLM.
- Contrast with: [[wiki/concepts/rag]] — stateless retrieval vs compounding artifact. See [[wiki/syntheses/rag-vs-llm-wiki]].

## Applications (from the source)

- Personal — goals, health, psychology, self-improvement tracking.
- Research — long deep-dives with an evolving thesis.
- Reading companion — per-chapter filing, building fan-wiki-style pages for characters, themes, plot threads.
- Business/team — LLM-maintained internal wiki fed by Slack, transcripts, project docs, customer calls.
- Competitive analysis, due diligence, trip planning, course notes, hobby deep-dives.

## Open questions

- Scaling ceiling: how far does "plain `index.md` + grep" take you before proper search (e.g. [qmd](https://github.com/tobi/qmd)) becomes necessary?
- Convention drift: how well does the pattern survive model upgrades or provider swaps?
- Non-text sources: audio/video beyond transcription — is there a better primitive than "transcribe first, then ingest"?
- Multi-user collaboration: how do two humans share one LLM-maintained wiki without stepping on each other?

## Sources

- [[wiki/sources/2026-04-17-llm-wiki-pattern]]
