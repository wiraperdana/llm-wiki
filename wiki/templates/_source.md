---
title: <Source title>
type: source
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: []
raw: ../../raw/<file-or-folder>
author: <author>
published: YYYY-MM-DD
medium: article|paper|book|podcast|video|transcript|note
url: <original URL if any>
status: stable        # stub | draft | stable | archived
confidence: 0.8       # how sure the summary faithfully reflects the raw source — typically high for fresh ingests
quality: 0.6          # self-assessed craft of the summary page itself (not the source)
scrubbed:             # list of things redacted from the summary (see AGENTS.md §4.1 step 2)
  # - API keys
  # - Client name (see raw)
supersedes:           # optional — [[wiki/sources/older-version]]
superseded_by:        # optional — [[wiki/sources/newer-version]]
---

# <Source title>

> Link to raw file: [`raw/<filename>`](../../raw/<filename>)

## 1-line summary

One sentence capturing the source's main claim or contribution.

## Key takeaways

- Takeaway 1.
- Takeaway 2.
- Takeaway 3.

## Detailed summary

2–5 paragraphs of prose covering the source's argument / findings / story. Don't just paraphrase — extract what's *useful for the wiki*.

## Entities introduced / updated

- [[wiki/entities/...]]

## Concepts introduced / updated

- [[wiki/concepts/...]]

## Contradictions or tensions

Does anything in this source contradict existing wiki pages? Flag with **CONTRADICTION** and link both. If this source materially replaces an older one, apply the supersession protocol (AGENTS.md §3.1d) and record the pair in frontmatter.

## Quotes worth keeping

> "A direct quote…"  — _(source, page/timestamp)_

## Open questions

What's unresolved? What to look for in future sources?
