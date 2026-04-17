---
title: Wiki Index
type: index
created: 2026-04-17
updated: 2026-04-17
---

# Wiki Index
_Last updated: 2026-04-17_

A content-oriented catalog of every page in this wiki. The LLM agent keeps this current on every ingest and new-page creation. For chronology, see [`log.md`](./log.md). For scoping rules (project vs top-level), see [`AGENTS.md`](./AGENTS.md) §2.5.

---

## Projects (0)

### 🟢 Active
_None yet. Start a project with_ *"new project `<slug>`"*.

### ⏸️ Paused
_None._

### ✅ Completed
_None._

### 🗄️ Archived
_None._

---

## Cross-cutting Sources (1)

- [[wiki/sources/2026-04-17-llm-wiki-pattern]] — The LLM Wiki pattern (meta-document describing this repo's design)

## Cross-cutting Entities (0)

_People / orgs / products that span multiple projects go here. Default is project-scoped._

## Cross-cutting Concepts (1)

- [[wiki/concepts/llm-wiki-pattern]] — Pattern for LLM-maintained personal knowledge bases

## Cross-cutting Syntheses (0)

_Portfolio-level analyses, retrospectives across projects. Empty until you do cross-project work._

## Slides (0)

_None yet. Generate a deck by asking the agent to "make a Marp deck about X"._

---

## How to use this index

The agent reads this file FIRST when answering queries — it's cheaper than scanning every page. Keep one-line summaries under ~15 words. Group alphabetically within each section (Projects grouped by status first).

Each project's internal pages are catalogued inside its own `wiki/projects/<slug>/README.md` (mini-index) — not here — so this top-level index stays scannable even with dozens of projects.

When a cross-cutting section grows beyond ~50 entries, consider splitting into sub-sections (e.g. `### Entities > People`, `### Entities > Organizations`).
