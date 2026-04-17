---
title: <Project name>
type: project
project: <project-slug>
created: YYYY-MM-DD
updated: YYYY-MM-DD
status: active       # active | paused | completed | archived — project lifecycle (distinct from page-maturity status on other types)
kickoff: YYYY-MM-DD
deadline: YYYY-MM-DD # optional — set null if open-ended
tags: [domain, ...]
stakeholders: [[wiki/entities/...]]  # people who appear in this project
confidence: 0.8      # how sure the agent is that this README faithfully reflects the project's current state
quality: 0.6         # self-assessed craft of the README itself (updated at lint)
supersedes:          # optional — [[wiki/projects/older-slug/README]]
superseded_by:       # optional — [[wiki/projects/newer-slug/README]]
relationships:       # optional — typed edges to cross-cutting pages
  # - {type: uses,       target: [[wiki/concepts/...]]}
  # - {type: owns,       target: [[wiki/entities/...]]}
  # - {type: depends_on, target: [[wiki/projects/other-slug/README]]}
---

# <Project name>

## One-paragraph description

What this project is, who it's for, what "done" looks like. 2–4 sentences.

## Context

Why this project exists. Background, origin, how it connects to other projects in the portfolio. Reference top-level entities/concepts with wiki-links.

## Stakeholders

- [[wiki/entities/...]] — role in this project
- [[wiki/projects/<slug>/entities/...]] — project-scoped contact

## Top-level pages this project relies on

- [[wiki/concepts/...]]
- [[wiki/entities/...]]

## Mini-index (LLM keeps current)

### Entities in this project
- [[wiki/projects/<slug>/entities/...]] — …

### Concepts in this project
- [[wiki/projects/<slug>/concepts/...]] — …

### Sources ingested for this project
- [[wiki/projects/<slug>/sources/...]] — …

### Syntheses written about this project
- [[wiki/projects/<slug>/syntheses/...]] — …

## Open threads / next actions

- What's blocked, what's waiting, what the agent should nudge you about next.

## Changelog

- `YYYY-MM-DD` — Project created
- `YYYY-MM-DD` — status changed from `active` to `paused` because …
