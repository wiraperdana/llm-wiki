# AGENTS.md — Wiki Schema & Operating Manual

> This file is the **schema** for this wiki. Any LLM agent working in this repo MUST read it first, follow the conventions, and update it when conventions evolve. It is LLM-agnostic — Claude, Codex, Pi, OpenCode, or any future agent can use it. A mirror copy lives at `CLAUDE.md` for Claude-specific tooling.

---

## 1. Purpose

This is a **persistent, compounding personal knowledge base** maintained by an LLM agent in collaboration with the user. It is not a RAG-over-documents setup. The LLM **reads raw sources and progressively builds a wiki** — interlinked markdown pages, summaries, entity pages, concept pages, syntheses — that sits between the user and the source material.

The human curates sources and asks questions. The LLM does all writing, cross-referencing, and bookkeeping.

**Pattern origin:** [LLM Wiki idea](./raw/llm-wiki-pattern.md) (seed document stored in `raw/`).

---

## 2. Architecture — three layers

```
/
├── AGENTS.md            ← schema (this file) — agent-agnostic
├── CLAUDE.md            ← mirror of this file for Claude-specific runtimes
├── README.md            ← human-facing intro
├── index.md             ← content catalog (LLM keeps this current)
├── log.md               ← append-only chronological log
├── raw/                 ← IMMUTABLE source documents (LLM reads, never writes)
│   ├── assets/          ← downloaded images from articles
│   └── ...              ← articles, papers, PDFs, notes, transcripts
└── wiki/                ← LLM-owned markdown (LLM writes & maintains)
    ├── entities/        ← people, organizations, products, places
    ├── concepts/        ← frameworks, ideas, theories, methodologies
    ├── sources/         ← one summary page per ingested raw source
    ├── syntheses/       ← comparisons, analyses, explorations, theses
    ├── slides/          ← Marp decks generated from wiki content
    └── templates/       ← blank templates with YAML frontmatter
```

**Layer rules:**
- `raw/` is **immutable**. Never edit, rename, or delete anything in it unless the user explicitly asks.
- `wiki/` is **fully owned by the LLM**. Create, update, merge, split pages freely — but keep cross-references consistent.
- `index.md` and `log.md` live at the root and must be updated on every ingest, query that produces a new page, and lint pass.

---

## 3. Page conventions

### 3.1 YAML frontmatter (required on every wiki page)

Every page in `wiki/` starts with YAML frontmatter so Obsidian's Dataview plugin can query it:

```yaml
---
title: <Human-readable title>
type: entity | concept | source | synthesis | slide
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [tag1, tag2]
sources: [[sources/some-source]]        # list of source pages this page draws from
aliases: [alt name, another alias]      # optional — Obsidian wiki-links will match these
status: stub | draft | stable           # how mature this page is
---
```

Templates for each `type` live in `wiki/templates/`.

### 3.2 Linking

- Use Obsidian wiki-link syntax: `[[entities/some-person]]` or `[[concepts/some-idea|display text]]`.
- Always link the FIRST mention of any entity or concept on a page. Subsequent mentions are optional.
- When a new entity/concept is mentioned but has no page yet, **create a stub page** (frontmatter + one-line description + "_Stub — expand when more sources arrive._") rather than leaving a dead link.
- Back-linking is free via Obsidian's graph view — no need to manually track inbound links.

### 3.3 Citations

Every non-trivial claim in a wiki page cites its source using wiki-links:
```
Claim about X.[[sources/article-slug]]
```
If multiple sources support the claim, chain them: `[[sources/a]] [[sources/b]]`.
When sources contradict, flag it explicitly with **CONTRADICTION** and link both.

### 3.4 File naming

- Lowercase, hyphenated slug: `wiki/entities/vannevar-bush.md`, `wiki/concepts/memex.md`.
- For sources, prefix with date: `wiki/sources/2026-04-17-llm-wiki-pattern.md`.
- No spaces, no uppercase, no special chars beyond `-` and digits.

---

## 4. Operations

The LLM performs three kinds of operations. Each has a checklist.

### 4.1 Ingest — "process this source"

Trigger: the user drops a file in `raw/` (or points to one already there) and says *"ingest"* / *"process this"*.

Checklist:
1. Read the raw source fully.
2. Discuss key takeaways with the user (2–5 bullet points) and ask what to emphasize.
3. Create `wiki/sources/YYYY-MM-DD-<slug>.md` using the source template.
4. Scan the wiki for related entities, concepts, and syntheses. For each affected page:
   - Update claims, add citations, flag contradictions.
   - Create stub pages for entities/concepts the source introduces that don't yet exist.
5. Update `index.md` — add the new source under "Sources", add any new entities/concepts under their sections.
6. Append an entry to `log.md` (see §6).
7. Report back to the user: which pages were created, which were updated, any contradictions or open questions worth investigating.

One ingest typically touches **10–15 pages**. That's fine. Batch the edits.

### 4.2 Query — "answer this question"

Trigger: the user asks a question.

Checklist:
1. Read `index.md` first to locate relevant pages.
2. Read those pages (and follow links as needed).
3. Synthesize the answer with wiki-link citations.
4. **Decide: is this answer worth keeping?** If the question required real synthesis (comparing, analyzing, connecting), file the answer as a new page in `wiki/syntheses/` so the insight compounds.
5. Output format can vary: inline markdown, a comparison table, a Marp slide deck (in `wiki/slides/`), a chart, a canvas. Default to an inline markdown answer; offer richer formats when useful.
6. If a new synthesis page was created: update `index.md` and `log.md`.

### 4.3 Lint — "health-check the wiki"

Trigger: the user says *"lint"* / *"audit"* / *"health-check"*.

Checklist — look for and report (don't auto-fix without confirmation):
- **Contradictions** between pages.
- **Stale claims** superseded by newer sources.
- **Orphan pages** with no inbound links (candidates to merge or delete).
- **Hub pages** that have grown too long and should be split.
- **Missing cross-references** (entity mentioned on many pages but not linked).
- **Implicit concepts** referenced repeatedly but lacking their own page.
- **Data gaps** that a web search could fill.
- **Stub pages** that have accumulated enough supporting sources to be upgraded to `draft` or `stable`.

Produce a report in `wiki/syntheses/lint-YYYY-MM-DD.md` with suggestions + counts. Only apply fixes the user approves.

---

## 5. `index.md` — the content catalog

`index.md` is content-oriented. The LLM rewrites it on every ingest / new-page creation so it stays current.

Structure:

```markdown
# Wiki Index
_Last updated: YYYY-MM-DD_

## Sources (N)
- [[sources/2026-04-17-llm-wiki-pattern]] — The LLM Wiki pattern (seed document)
- ...

## Entities (N)
- [[entities/vannevar-bush]] — Engineer who proposed the Memex in 1945
- ...

## Concepts (N)
- [[concepts/memex]] — Bush's hypothetical personal knowledge device
- [[concepts/rag]] — Retrieval-Augmented Generation
- ...

## Syntheses (N)
- [[syntheses/rag-vs-llm-wiki]] — Comparison of paradigms
- ...

## Slides (N)
- [[slides/...]] — ...
```

Keep one-line summaries tight (~10–15 words). Group alphabetically within each section.

---

## 6. `log.md` — chronological record

Append-only. Every ingest, every new synthesis, every lint pass gets an entry. Entries start with a consistent prefix so `grep` works:

```markdown
## [YYYY-MM-DD] <op> | <title>
<1-3 sentence summary of what happened, pages touched, open questions>
```

`<op>` is one of: `ingest`, `query`, `synthesis`, `lint`, `schema-update`, `other`.

Quick queries the LLM can run:
```bash
grep "^## \[" log.md | tail -10          # last 10 entries
grep "^## \[.*ingest"   log.md | wc -l   # total ingests
grep "^## \[2026-04"    log.md           # everything in April 2026
```

---

## 7. Image handling

Some raw sources are articles with embedded images.

**Convention:**
- Downloaded images live in `raw/assets/` (set Obsidian's attachment folder there).
- In markdown, reference them relatively: `![alt](../raw/assets/foo.png)`.
- An LLM reading a markdown source can usually only process text natively. **Workflow**: read the source text first, then if images matter (diagrams, charts, screenshots), open specific image files separately with a vision-capable tool.
- When creating a source summary, always note which referenced images are important and which can be skipped.

**Obsidian Web Clipper + hotkey tip** (configure once):
1. Install Obsidian Web Clipper browser extension — clips web articles to markdown in `raw/`.
2. Settings → Files and links → Default location for new attachments → `raw/assets/`.
3. Settings → Hotkeys → search "Download attachments for current file" → bind to `Ctrl+Shift+D`.
4. After clipping, hit the hotkey; images download locally.

---

## 8. Slide decks (Marp)

Marp is markdown-based slides. Decks live in `wiki/slides/` with frontmatter like:

```yaml
---
marp: true
theme: default
paginate: true
title: <title>
type: slide
created: YYYY-MM-DD
sources: [[syntheses/...]]
---
```

Slides should **draw content from wiki pages**, not restate raw-source material. Cite the pages they're built from. Install the Marp plugin in Obsidian for in-editor preview.

---

## 9. Principles — keep in mind every session

1. **Wiki compounds; chat doesn't.** If an insight is worth remembering, it belongs in the wiki, not buried in a chat log.
2. **Human curates, LLM maintains.** Don't ask the user to do bookkeeping the LLM can do.
3. **Stub early, expand later.** It's better to create a one-line stub page than leave a dead link.
4. **Cite everything.** No unsourced claims in wiki pages.
5. **Flag, don't silently reconcile, contradictions.** Let the user decide.
6. **Update the schema when you learn something about the domain.** The schema is a living document — if you discover a useful convention, propose adding it here.
7. **Prefer small, frequent edits over giant rewrites.** Git-friendly.

---

## 10. Quick-reference commands for the agent

When the user says… | the agent does…
--- | ---
"ingest X" / "process X" / "file this" | Run the §4.1 checklist.
"what does the wiki say about X?" | Run §4.2, read index + pages, answer with citations.
"compare X and Y" | Run §4.2 and file the answer in `wiki/syntheses/`.
"lint" / "audit" / "clean up" | Run §4.3, produce report, wait for approval before fixing.
"update the schema" | Edit this file and `CLAUDE.md` together. Log the change.
"show me the graph" / "what's connected" | Suggest the user use Obsidian's graph view; summarize orphans / hubs.

---

_This schema co-evolves with the wiki. When you discover a better convention, update this file and note the change in `log.md` under `schema-update`._
