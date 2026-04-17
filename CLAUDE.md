# CLAUDE.md

This wiki uses an **agent-agnostic** schema that lives in [`AGENTS.md`](./AGENTS.md). Claude Code, Claude Cowork, and any other Claude-powered agent should read `AGENTS.md` in full at the start of every session — it contains all conventions, directory structure, operations (ingest / query / lint), frontmatter specs, and citation rules.

This file exists as a pointer so that Claude runtimes that look for `CLAUDE.md` by default are directed to the canonical schema.

---

## Claude-specific quick notes

- **Treat `AGENTS.md` as the source of truth.** If this file and `AGENTS.md` ever disagree, `AGENTS.md` wins. Update this file to match when the schema changes.
- **Todo lists** — When running a multi-step ingest or lint, use Claude's TodoWrite tool so the user can track progress.
- **Ask before batch operations.** Before touching >10 pages in one pass, summarize the plan and confirm.
- **Logging.** After every session that writes to the wiki, append a `log.md` entry (see `AGENTS.md` §6).
- **Lint cadence.** Per `AGENTS.md` §9 principle #10: every 20 ingests or 30 days since last lint, proactively suggest a lint pass at the end of the current operation. Check with `grep "^## \[.*ingest" log.md | wc -l` vs. last `lint` entry timestamp.
- **Uncertainty hygiene.** Per `AGENTS.md` §9 principle #11 and §3.1b/c: every wiki page gets a `confidence` and `quality` score in frontmatter. Use `{c=...}` markers inline for load-bearing claims. Never silently overwrite an older claim — apply the supersession protocol (§3.1d).
- **Scrub before writing.** Per `AGENTS.md` §4.1 step 2: before any text lands in `wiki/`, redact credentials, API keys, PII, NDA-covered client names. `raw/` stays untouched (immutable); the scrub applies only to wiki summaries/quotes. Record what was scrubbed in the source page's frontmatter `scrubbed:` list.
- **Entity extraction before writing.** Per `AGENTS.md` §4.1 step 5: produce a structured `{ENTITIES, CONCEPTS, CLAIMS, RELATIONS}` list first, then write pages. This catches naming collisions and ensures completeness.
- **Auto-crystallize synthesis.** Per `AGENTS.md` §4.2 step 4: if a query answer exceeds ~500 words of analysis, connects ≥3 concepts, or surfaces a contradiction, file it as a synthesis page automatically.
- **Feature flags are off by default.** Per `AGENTS.md` §11: `auto_resolve_contradictions`, `self_healing_lint`, `auto_quality_score_on_write`, `retention_decay`, `hybrid_search` stay disabled until the user says *"enable `<flag>`"*. Log toggles as `schema-update`.
- **Skills to prefer** when relevant: `engineering:documentation`, `product-management:synthesize-research`, `anthropic-skills:pdf` (for PDF sources), `anthropic-skills:pptx` / Marp (for slide output).

---

Go read [`AGENTS.md`](./AGENTS.md) now.
