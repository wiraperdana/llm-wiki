---
title: Wiki Log
type: log
created: 2026-04-17
---

# Wiki Log

Append-only chronological record of everything that happens in this wiki. Every entry starts with `## [YYYY-MM-DD] <op> | <title>` so it's greppable:

```bash
grep "^## \[" log.md | tail -10          # last 10 entries
grep "^## \[.*ingest" log.md | wc -l     # total ingests
grep "^## \[2026-04" log.md              # April 2026 activity
```

Operations: `ingest`, `query`, `synthesis`, `lint`, `schema-update`, `other`.

---

## [YYYY-MM-DD] other | Wiki initialized

_← Replace this with your own first entry. Example below:_

Bootstrapped the repo from the LLM Wiki pattern. Created `AGENTS.md` (schema), `CLAUDE.md` (pointer), `README.md`, `index.md`, `log.md`. Scaffolded directories: `raw/`, `raw/assets/`, `wiki/{entities,concepts,sources,syntheses,slides,templates}`. Added page templates for each type with YAML frontmatter. Seeded with the pattern document itself as the first source and created example pages: Vannevar Bush (entity), Memex, RAG, LLM Wiki pattern (concepts), and a RAG-vs-LLM-Wiki synthesis.

## [2026-04-17] schema-update | Multi-project support added

Extended `AGENTS.md` to explicitly support multi-project wikis. Rationale: this repo is intended as a template to be cloned into separate domain wikis (e.g. `llm-wiki-personal/`, `llm-wiki-company-a/`, `llm-wiki-academic/`), each of which will host many projects. Changes:

- New §2.5 "Project scoping" with the "local-first, promote when shared" rule + a what-goes-where table.
- Architecture diagram (§2) now shows `raw/projects/<slug>/` and `wiki/projects/<slug>/{entities,concepts,sources,syntheses,README.md}`.
- Frontmatter (§3.1) gains a `project:` field (required on project-scoped pages, omitted on top-level).
- Ingest checklist (§4.1) now asks the agent to determine scope first and file into the right location.
- Lint checklist (§4.3) gains promotion/demotion candidates and dead-project detection.
- Index layout (§5) restructured around a Projects section (grouped by status: active / paused / completed / archived) + cross-cutting sections.
- Principles (§9) add "local-first, promote when shared" and "scope discipline".
- Quick-reference (§10) adds commands: `new project <slug>`, `archive project <slug>`, `promote <page>`, `demote <page>`.

Scaffolding created: `wiki/projects/` + `wiki/projects/README.md`, `raw/projects/` + `raw/projects/README.md`, `wiki/templates/_project.md` for project READMEs. Top-level `index.md` restructured to the new layout — currently empty of real content, ready to accept projects.

No project folders created yet. Next step: either rename this repo to a domain wiki (e.g. `llm-wiki-personal/`) and start the first project, or keep this as a pristine template and clone it into the first real domain.

## [2026-04-17] other | Template cleanup: pruned over-extracted seeds + consolidated docs

Cleaned up the template to ship as a pristine starting point. Three changes:

1. **Removed over-extracted seed pages.** The original bootstrap created dedicated pages for Memex, RAG, and Vannevar Bush even though the raw source only mentions them in passing. Deleted: `wiki/entities/vannevar-bush.md`, `wiki/concepts/memex.md`, `wiki/concepts/rag.md`, `wiki/syntheses/rag-vs-llm-wiki.md`. Lesson filed under principles: don't fabricate pages from passing mentions — only what the source actually develops earns a page. Future sources that genuinely develop these topics can introduce them then.
2. **Cleaned dead links in remaining seed pages.** `wiki/sources/2026-04-17-llm-wiki-pattern.md` and `wiki/concepts/llm-wiki-pattern.md` both referenced the deleted pages. Converted those wiki-links to plain-text mentions and removed the "Related concepts" section from the concept page.
3. **Consolidated 5 sub-READMEs into the root README.** Previously every subfolder had its own README (`raw/README.md`, `raw/projects/README.md`, `wiki/projects/README.md`, `wiki/slides/README.md`, `wiki/templates/README.md`). Deleted all five. The root `README.md` is now the single user-facing doc and covers: setup, folder layout, immutability rules (only `raw/` is LLM-immutable), daily workflow, creating a new project (`new project <slug>`), assigning raw files to projects, queries, lint, conventions, templates, Marp, and cloning the template into domain wikis.

Net state: template is minimal (1 raw source, 1 source page, 1 concept page), schema is multi-project-ready, documentation is consolidated into a single root README. Ready to clone into domain wikis (`llm-wiki-personal`, `llm-wiki-company-a`, etc.).

## [2026-04-17] schema-update | Lint cadence rule added

Added a new principle to `AGENTS.md` §9 (principle #10) formalizing lint cadence: every 20 ingests or every 30 days since last lint, whichever comes first. Rationale: at scale (hundreds-to-thousands of raw sources), staleness and hallucination risk are driven by the ingest-to-lint ratio, not by raw corpus size. Making cadence explicit keeps the wiki layer from drifting away from the raw layer.

Changes:
- §4.3 trigger updated: lint can now fire on cadence, not only on user command. Agent proactively suggests lint at the end of the current operation (does not silently start one).
- §9 principle #10 added with rationale + grep recipes to check cadence from `log.md`.
- §10 quick-reference table gains a "cadence trigger" row.
- `CLAUDE.md` mirrors the rule in its Claude-specific quick notes.

No wiki content changed — schema-only.

## [2026-04-17] schema-update | v2 additions: confidence, quality, supersession, typed relationships, scrub, entity extraction, auto-crystallize, feature flags

Adopted six data-schema additions + one principle + one feature-flag section from the "LLM Wiki v2" playbook (ref: `raw/` upload `llm-wiki-advance.md`). Rationale: data-schema changes are **cheap at page creation** but **expensive (sometimes impossible) to backfill** later — e.g., supersession trail cannot be reconstructed once a page has been overwritten without it. Adopting early means every page born from now on carries the new fields, avoiding a retrofit at ~500 pages. Behavioral changes (auto-resolve contradictions, self-healing lint, retention decay, etc.) are **kept behind feature flags** in a new §11 so they can be switched on without touching data.

Changes:

**Schema (AGENTS.md §3):**
- §3.1 frontmatter gains: `confidence` (0.0–1.0), `quality` (0.0–1.0), `supersedes`, `superseded_by`, optional `relationships` block. `status` extended to include `archived`.
- §3.1a — `status` documented as consolidation tier with reader-action table.
- §3.1b — confidence calibration guide.
- §3.1c — quality as self-assessed craft score.
- §3.1d — supersession protocol: mark both sides, preserve old body under an "Archived" header, update `index.md`, log.
- §3.2a — typed relationships (inline `|type` on wiki-links + frontmatter `relationships:` block), standard relationship types table (`uses`, `depends_on`, `owns`, `caused`, `fixed`, `contradicts`, `supersedes`, `part_of`, `references`).
- §3.3 — inline confidence marker syntax `{c=0.0-1.0}` after citations for claim-level confidence.

**Operations (AGENTS.md §4):**
- §4.1 ingest: added step 2 (scrub sensitive data — API keys, PII, NDA-covered names; record in source page's `scrubbed:` frontmatter), step 5 (entity-extraction pass emitting structured `{ENTITIES, CONCEPTS, CLAIMS, RELATIONS}` list before writing), and per-page confidence/quality recomputation in step 7.
- §4.2 query: step 4 changed from discretionary to threshold-driven auto-crystallize (≥500 words OR ≥3 concepts connected OR surfaces a contradiction OR user asks).

**Principles (AGENTS.md §9):**
- Principle #11 added: "Make uncertainty visible." Prefer confidence markers, supersession trails, and contradiction flags over silent overwrites or confident guesses.

**Feature flags (AGENTS.md §11 — new section):**
- Five behaviors declared but off-by-default: `auto_resolve_contradictions`, `self_healing_lint`, `auto_quality_score_on_write`, `retention_decay`, `hybrid_search`. Each with its "turn on when" criterion. Toggled via `enable <flag>` / `disable <flag>` (added to §10 quick-ref).

**Templates (`wiki/templates/*`):**
- All six templates (`_concept`, `_entity`, `_source`, `_synthesis`, `_slide`, `_project`) extended with `confidence`, `quality`, `supersedes`, `superseded_by`. Relationship-bearing templates (`_concept`, `_entity`, `_synthesis`, `_project`) gain commented `relationships:` scaffolding. `_source` additionally gains `scrubbed:` field. Defaults per type: stubs start at `confidence: 0.3, quality: 0.2`; fresh source summaries at `0.8 / 0.6`; project READMEs at `0.8 / 0.6`.

**CLAUDE.md:**
- Six new bullets under "Claude-specific quick notes" covering uncertainty hygiene, scrub, entity extraction, auto-crystallize, and feature-flag defaults.

**What was deliberately NOT adopted:**
- Full four-tier consolidation (working/episodic/semantic/procedural) — folded into extended `status` enum instead (`stub | draft | stable | archived`).
- Ebbinghaus retention decay as a live mechanism — deferred as feature flag `retention_decay`.
- Hybrid search (BM25 + vectors + graph) — deferred as feature flag `hybrid_search`; index.md + Obsidian search stays primary.
- Mesh sync / multi-agent coordination — single-user wiki, not applicable.
- Event-driven automation hooks — outside current tooling scope.

Existing wiki content (1 source page, 1 concept page) is NOT backfilled — the new fields are optional. They will be populated on the next touch of those pages during ingest or lint.
