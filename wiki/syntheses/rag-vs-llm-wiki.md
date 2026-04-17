---
title: RAG vs the LLM Wiki pattern
type: synthesis
created: 2026-04-17
updated: 2026-04-17
tags: [llm, rag, knowledge-management, comparison]
sources: [[wiki/sources/2026-04-17-llm-wiki-pattern]]
status: draft
---

# RAG vs the LLM Wiki pattern

**Thesis.** [[wiki/concepts/rag]] optimizes for query-time retrieval over a static pile of documents; the [[wiki/concepts/llm-wiki-pattern]] optimizes for _knowledge that compounds over time_. For any workflow where the user returns to the same material repeatedly, asks subtle cross-document questions, or accumulates sources over weeks or months, the LLM-Wiki pattern dominates — because structure is pre-computed once instead of re-synthesized every query.[[wiki/sources/2026-04-17-llm-wiki-pattern]]

## Context

Most "chat with your documents" products today (NotebookLM, ChatGPT file uploads, enterprise RAG chatbots) are RAG systems. A user evaluating whether to adopt the LLM-Wiki pattern needs a clear mental model of where each approach fits.[[wiki/sources/2026-04-17-llm-wiki-pattern]]

## Argument — side-by-side

| Dimension | RAG | LLM Wiki pattern |
| --- | --- | --- |
| **Primary artifact** | The raw corpus + an embedding index | A curated, LLM-maintained wiki of interlinked pages |
| **When knowledge is synthesized** | At query time, from scratch | At ingest time, into pages that persist |
| **Cross-references** | Implicit (via embedding neighbors) | Explicit wiki-links maintained by the LLM |
| **Contradictions** | Usually invisible — model averages or picks | Flagged on the relevant pages at ingest |
| **Cost per query** | Retrieval + generation | Usually just generation over a small set of pages |
| **Cost per source added** | Re-embed the chunk | ~10–15 page edits |
| **Human effort** | Upload and ask | Curate + ask + occasionally approve/lint |
| **Fits well when** | Large static corpus, lookup-style questions | Growing corpus, synthesis-heavy questions, long-running research |
| **Fits poorly when** | You want accumulated synthesis or evolving thesis | Corpus is enormous and lookup is the main use case |
| **Spiritual ancestor** | — | Vannevar Bush's [[wiki/concepts/memex]] |

## Supporting evidence

- RAG "rediscovers knowledge from scratch on every question" — nothing accumulates. [[wiki/sources/2026-04-17-llm-wiki-pattern]]
- In the LLM Wiki pattern, "the cross-references are already there. The contradictions have already been flagged." [[wiki/sources/2026-04-17-llm-wiki-pattern]]
- The _maintenance_ cost that kills human-maintained wikis (updating cross-references, keeping summaries current) is near zero when the LLM does it. [[wiki/sources/2026-04-17-llm-wiki-pattern]]
- A single ingest "might touch 10–15 wiki pages" — prohibitive for a human, trivial for an LLM. [[wiki/sources/2026-04-17-llm-wiki-pattern]]

## Counter-evidence / caveats

- **Scale matters.** The LLM Wiki's index-based navigation works well at "~100 sources, ~hundreds of pages" per the source. Past that, you likely want RAG-over-wiki-pages (e.g. [qmd](https://github.com/tobi/qmd)) as a complement rather than a replacement.[[wiki/sources/2026-04-17-llm-wiki-pattern]]
- **Discipline cost.** The LLM-Wiki pattern depends on a well-maintained schema and consistent ingests. A neglected wiki degrades; RAG doesn't care if you never look at it.
- **The two aren't exclusive.** A mature system can do both: the wiki as compiled knowledge, RAG over both raw and wiki pages as a retrieval layer. The source document hints at this via the qmd recommendation.[[wiki/sources/2026-04-17-llm-wiki-pattern]]
- **Lookup vs synthesis.** If 90% of a user's questions are "what page does X say?", plain RAG is cheaper and simpler — the wiki layer would mostly be dead weight.

## Conclusion

Pick RAG for **read-once, lookup-heavy** corpora where synthesis isn't the point. Pick the LLM Wiki pattern for **long-running, synthesis-heavy** workflows where knowledge should compound and the corpus is actively curated. At scale, combine them: LLM Wiki for compiled structure, RAG for retrieval across everything.

## Related pages

- [[wiki/concepts/llm-wiki-pattern]]
- [[wiki/concepts/rag]]
- [[wiki/concepts/memex]]
- [[wiki/entities/vannevar-bush]]
- [[wiki/sources/2026-04-17-llm-wiki-pattern]]
