---
title: Retrieval-Augmented Generation (RAG)
type: concept
created: 2026-04-17
updated: 2026-04-17
tags: [llm, retrieval, knowledge-management]
aliases: [RAG, retrieval augmented generation]
sources: [[wiki/sources/2026-04-17-llm-wiki-pattern]]
status: stub
---

# Retrieval-Augmented Generation (RAG)

**Definition.** A pattern where, at query time, a system retrieves relevant chunks from a document collection (typically via embedding similarity) and feeds them into an LLM's context to generate a grounded answer. Commercial examples include NotebookLM, ChatGPT file uploads, and most enterprise "chat-with-your-docs" products.[[wiki/sources/2026-04-17-llm-wiki-pattern]]

## Why it matters

It's the dominant pattern for "use an LLM with my documents," and therefore the relevant foil for the [[wiki/concepts/llm-wiki-pattern]]. Understanding where RAG falls short is how the LLM-Wiki idea justifies itself.

## Key properties

- **Stateless per query.** Each question triggers its own retrieval; nothing accumulates between queries.[[wiki/sources/2026-04-17-llm-wiki-pattern]]
- **Re-synthesis every time.** The model re-derives the same conclusions repeatedly if you ask related questions.[[wiki/sources/2026-04-17-llm-wiki-pattern]]
- **No persistent structure.** There are no cross-references, no flagged contradictions, no evolving synthesis — only raw chunks and on-demand generation.[[wiki/sources/2026-04-17-llm-wiki-pattern]]
- **Strong for lookup, weak for synthesis.** Great at "where does the source say X?"; weaker at "how have my five most recent sources changed my view of X?"[[wiki/sources/2026-04-17-llm-wiki-pattern]]

## Related concepts

- Contrast with [[wiki/concepts/llm-wiki-pattern]] — see [[wiki/syntheses/rag-vs-llm-wiki]] for the full comparison.
- Could complement the LLM-Wiki pattern at scale — RAG over wiki pages is still valuable once the wiki outgrows plain index lookups (see the note on [qmd](https://github.com/tobi/qmd) in the [[wiki/sources/2026-04-17-llm-wiki-pattern|source]]).

## Open questions

- At what corpus size does RAG-only become strictly better than index-lookup for a well-maintained wiki?
- Hybrid architectures: is the "right" setup wiki-as-compiled-knowledge + RAG-over-wiki-pages for scale?

## Sources

- [[wiki/sources/2026-04-17-llm-wiki-pattern]]
