# LLM Wiki — Personal Knowledge Base Template

A persistent, compounding knowledge base maintained by an LLM agent. You drop sources into `raw/`; the LLM builds and maintains interlinked wiki pages in `wiki/`.

**This is not RAG.** The LLM reads sources once, distills them into structured pages, and keeps those pages current as new sources arrive. The wiki itself is the artifact — not the raw pile.

This file is the **only README** in the repo. Everything a user needs to know lives here. The agent reads [`AGENTS.md`](./AGENTS.md) for its operating manual.

---

## Table of contents

1. [Setup (one-time)](#1-setup-one-time)
2. [What's in this folder](#2-whats-in-this-folder)
3. [Immutability — who can write what](#3-immutability--who-can-write-what)
4. [Daily workflow](#4-daily-workflow)
5. [Creating a new project](#5-creating-a-new-project)
6. [Assigning a raw file to a project](#6-assigning-a-raw-file-to-a-project)
7. [Asking questions & filing answers](#7-asking-questions--filing-answers)
8. [Periodic lint](#8-periodic-lint)
9. [Conventions reference](#9-conventions-reference)
10. [Templates](#10-templates)
11. [Slides (Marp) & image handling](#11-slides-marp--image-handling)
12. [Using this repo as a template for multiple domain wikis](#12-using-this-repo-as-a-template-for-multiple-domain-wikis)

---

## 1. Setup (one-time)

Open this folder in Obsidian. Install these community plugins:

- **Dataview** — queries YAML frontmatter on wiki pages.
- **Marp** — previews slide decks in `wiki/slides/`.
- **Obsidian Web Clipper** (browser extension) — clips articles into `raw/` as markdown.

Obsidian attachment folder: go to _Settings → Files and links → Default location for new attachments_ → set to `raw/assets/` (for cross-cutting sources) or configure per-project as needed.

Optional hotkey: _Settings → Hotkeys → search "Download attachments for current file"_ → bind to `Ctrl+Shift+D`. After clipping an article, press the hotkey to download all its images locally.

Open your LLM agent (Claude Cowork, Claude Code, Codex, any agent that reads `AGENTS.md` or `CLAUDE.md`) with this folder as its working directory. The agent reads `AGENTS.md` at the start of every session — no setup needed on its side.

---

## 2. What's in this folder

```
/
├── AGENTS.md            ← schema + operating manual for the LLM
├── CLAUDE.md            ← pointer for Claude-specific runtimes → AGENTS.md
├── README.md            ← YOU ARE HERE — the only README for users
├── index.md             ← content catalog (LLM-maintained)
├── log.md               ← chronological record of ingests, queries, lints
│
├── raw/                 ← your source documents (IMMUTABLE for the LLM)
│   ├── assets/          ← images downloaded by Web Clipper
│   ├── projects/        ← project-scoped sources
│   │   └── <slug>/      ← one folder per project
│   └── *.md, *.pdf, ... ← cross-cutting sources
│
└── wiki/                ← LLM-owned markdown
    ├── entities/        ← CROSS-CUTTING people, orgs, products, places
    ├── concepts/        ← CROSS-CUTTING frameworks, ideas, theories
    ├── sources/         ← summaries of cross-cutting raw sources
    ├── syntheses/       ← CROSS-CUTTING analyses, comparisons, retrospectives
    ├── projects/        ← per-project folders (mirror of raw/projects/)
    │   └── <slug>/
    │       ├── README.md         ← project metadata + mini-index
    │       ├── entities/         ← project-scoped entities
    │       ├── concepts/         ← project-scoped concepts
    │       ├── sources/          ← summaries of project raw sources
    │       └── syntheses/        ← project-scoped analyses
    ├── slides/          ← Marp decks
    └── templates/       ← blank templates (_entity, _concept, _source,
                           _synthesis, _slide, _project)
```

---

## 3. Immutability — who can write what

| Location | User writes? | LLM writes? |
| --- | :---: | :---: |
| `raw/` (including `raw/projects/`) | ✅ | ❌ **Immutable for LLM** |
| `wiki/` (everything under it) | ⚠️ discouraged | ✅ Primary owner |
| `index.md` | ⚠️ discouraged | ✅ Rewritten on every ingest |
| `log.md` | ⚠️ append-only by LLM | ✅ Append-only |
| `AGENTS.md` / `CLAUDE.md` | ✅ | ✅ Co-evolved — LLM can propose edits |

**Rule of thumb:** `raw/` is the only folder the LLM will never touch. Everything else is LLM-writable. If you want to edit a wiki page by hand, you can — just know the LLM may overwrite it on the next ingest if the page is stale. Better to tell the agent what to change.

`AGENTS.md` is a special case: it's the schema that tells the LLM how this wiki is organized. You and the LLM should co-evolve it. If you discover a new convention you like, tell the agent *"update the schema"* and it will edit `AGENTS.md` and log the change.

---

## 4. Daily workflow

The LLM performs three kinds of operations. You trigger them in natural language.

| You say… | The LLM does… |
| --- | --- |
| *"ingest `raw/<file>`"* / *"process this"* | Reads the source, creates a summary page, touches related pages, updates `index.md`, logs it. |
| *"what does the wiki say about X?"* / *"compare X and Y"* | Reads `index.md` → reads relevant pages → answers with citations. Files the answer as a new synthesis if it was a real piece of analysis. |
| *"lint"* / *"audit"* | Health-checks the wiki: contradictions, orphans, stale claims, promotion/demotion candidates. Produces a report. You approve fixes. |

Full operational details live in `AGENTS.md` §4.

---

## 5. Creating a new project

Projects are the main unit of organization once you have more than a handful of sources.

**To create a project,** tell the agent:

> *"new project `acme-rollout`"*

The agent will:

1. Create `wiki/projects/acme-rollout/` with subfolders `entities/`, `concepts/`, `sources/`, `syntheses/`.
2. Copy `wiki/templates/_project.md` → `wiki/projects/acme-rollout/README.md` and **prompt you for metadata**: status (active/paused/completed/archived), kickoff date, deadline (optional), key stakeholders, one-paragraph description.
3. Create `raw/projects/acme-rollout/` for the project's raw sources.
4. Add the project to the top-level `index.md` under _Projects → 🟢 Active_.
5. Append a `schema-update` entry to `log.md`.

**Project slug rules:** lowercase, hyphenated, short, unique within this wiki. Examples: `acme-rollout`, `doctoral-chapter-2`, `q2-competitive-analysis`.

**Changing status:** say *"archive project `acme-rollout`"* or *"mark `bravo-pilot` as paused"*. The agent updates the project's `README.md` frontmatter and moves it between groups in the top-level index.

**Per-project mini-index.** Each project's `wiki/projects/<slug>/README.md` contains its own catalogue of entities, concepts, sources, and syntheses — so project navigation stays self-contained and the top-level `index.md` doesn't explode with project internals.

---

## 6. Assigning a raw file to a project

There are two ways. Pick whichever is more convenient.

### Method A — drop the file in the right folder

If you know upfront which project a source belongs to, place the file directly in `raw/projects/<slug>/`. Then tell the agent:

> *"ingest `raw/projects/acme-rollout/2026-04-17-kickoff.md`"*

The agent infers scope from the path and files the summary at `wiki/projects/acme-rollout/sources/2026-04-17-kickoff.md`.

### Method B — drop in `raw/` root, let the agent ask

If you're not sure yet (or the source might be cross-cutting), drop it in `raw/` root and say:

> *"ingest `raw/2026-04-17-some-article.md`"*

The agent will ask: *"Which project does this belong to, or is this cross-cutting?"* Answer with a slug (or `cross-cutting`), and the agent files accordingly — **moving** the raw file into `raw/projects/<slug>/` if project-scoped so the source and its summary live in parallel folders.

### Cross-cutting sources

A source that genuinely informs multiple projects (industry reports, reference papers, generic articles) stays at `raw/` root, with its summary at `wiki/sources/`. Don't force everything into a project.

### What about entities and concepts a source introduces?

The agent defaults to **project-scoped**: a new person mentioned in a project-ingested source gets a page at `wiki/projects/<slug>/entities/...`, not at top-level. The agent only promotes to top-level (`wiki/entities/...`) when the same entity clearly appears in ≥2 projects. This is the *"local-first, promote when shared"* principle (see `AGENTS.md` §2.5). Promotion happens with your approval — the agent flags candidates during ingest and lint, it doesn't auto-promote.

---

## 7. Asking questions & filing answers

Queries use the same natural language as ingest:

> *"what does the wiki say about middleware strategy for Acme?"*
>
> *"compare the risk profiles of acme-rollout and bravo-pilot"*
>
> *"summarize everything I know about Alice Smith"*

The agent reads `index.md` → navigates to relevant pages → answers with wiki-link citations. For real pieces of synthesis (comparisons, analyses, thesis-building), the agent files the answer as a new page in the right `syntheses/` folder so your explorations **compound**, not disappear into chat.

Output formats the agent can produce: inline markdown (default), a Marp slide deck in `wiki/slides/`, a matplotlib chart, or a comparison table. Tell it what you want.

---

## 8. Periodic lint

Every few weeks, say *"lint the wiki"*. The agent produces a report at `wiki/syntheses/lint-YYYY-MM-DD.md` listing:

- Contradictions between pages
- Stale claims superseded by newer sources
- Orphan pages with no inbound links
- Hub pages that have grown too long
- Missing cross-references
- Stub pages ready for upgrade
- **Promotion candidates** — project-scoped pages referenced by ≥2 projects
- **Demotion candidates** — top-level pages only ever used by one project
- **Dead projects** — `active` projects with no activity in N months

You approve which fixes to apply. The agent never auto-fixes.

---

## 9. Conventions reference

Full details in `AGENTS.md`. Quick reference:

**File naming.** Lowercase, hyphenated slugs. Sources prefixed with ISO date: `2026-04-17-kickoff-notes.md`.

**Wiki-links.** Full path from repo root: `[[wiki/entities/alice-smith]]` or `[[wiki/projects/acme-rollout/concepts/payment-flow]]`.

**YAML frontmatter.** Every wiki page starts with:

```yaml
---
title: <Human-readable title>
type: entity | concept | source | synthesis | slide | project
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [tag1, tag2]
project: <slug>        # REQUIRED for project-scoped pages; OMIT for top-level
sources: [[wiki/sources/some-source]]
aliases: [alt name]
status: stub | draft | stable
---
```

**Citations.** Every non-trivial claim in a wiki page cites a source via wiki-link: `The payment flow handles X.[[wiki/projects/acme-rollout/sources/2026-04-17-kickoff]]`.

**Log entries.** Prefixed `## [YYYY-MM-DD] <op> | <title>`. Greppable:

```bash
grep "^## \[" log.md | tail -10          # last 10 entries
grep "^## \[.*ingest" log.md | wc -l     # total ingests
grep "^## \[.*acme-rollout" log.md       # everything touching this project
```

---

## 10. Templates

Blank templates live in `wiki/templates/`. The agent copies + fills them when creating new pages. You generally don't need to touch them, but it's useful to know they exist:

| Template | Used when… |
| --- | --- |
| `_project.md` | Creating a project's `README.md` |
| `_entity.md` | Creating a person / org / product / place page |
| `_concept.md` | Creating a concept / framework / theory page |
| `_source.md` | Summarizing an ingested raw source |
| `_synthesis.md` | Filing an analysis, comparison, or thesis |
| `_slide.md` | Starting a Marp slide deck |

If you want to change a template convention (e.g. add a new frontmatter field), edit the template AND `AGENTS.md` §3.1 so they stay consistent.

---

## 11. Slides (Marp) & image handling

**Slides.** Ask the agent to *"make a Marp deck about X"* or *"turn `wiki/projects/acme-rollout/syntheses/foo.md` into slides"*. The agent creates a `.md` file in `wiki/slides/` with `marp: true` frontmatter. Open it in Obsidian with the Marp plugin installed to get live preview; export to PDF/PPTX via the Marp CLI (`marp wiki/slides/<name>.md`).

Slides should **cite wiki pages**, not raw sources directly — the wiki is the compiled-knowledge layer.

**Images.** Downloaded images live in `raw/assets/` (cross-cutting) or `raw/projects/<slug>/assets/` (project-scoped). In markdown, reference them with relative paths like `![alt](../../raw/assets/foo.png)`. An LLM reading a markdown source can usually only process text natively — if an image matters (diagram, chart, screenshot), the agent will open that image file separately with a vision-capable tool.

---

## 12. Using this repo as a template for multiple domain wikis

**This repo is a template.** It's intentionally kept pristine — you are expected to clone it into separate folders for each domain boundary you care about (personal / per-company / academic, etc.) and then start filling each one with real projects.

Recommended top-level shape if you have multiple domains:

```
~/Obsidian/
├── llm-wiki/              ← THIS FOLDER — pristine template (don't work in it directly)
├── llm-wiki-personal/     ← your frameworks, self-knowledge, cross-cutting methodology
├── llm-wiki-company-a/    ← company A + all its projects
├── llm-wiki-company-b/    ← company B + all its projects
└── llm-wiki-academic/     ← master-doctoral + research projects
```

**To clone:** copy this entire folder to a new name. Delete the seed content (`raw/llm-wiki-pattern.md`, `wiki/sources/2026-04-17-llm-wiki-pattern.md`, `wiki/concepts/llm-wiki-pattern.md`), empty the `log.md` except for a fresh `## [YYYY-MM-DD] other | Wiki initialized` entry, rewrite `index.md` as empty, and start your first project with *"new project `<slug>`"*.

See `AGENTS.md` §2.5 for why multi-project within one wiki beats separate-wiki-per-project for most use cases.

---

_Inspired by the [LLM Wiki pattern](./raw/llm-wiki-pattern.md). Maintained by LLMs, curated by humans._
