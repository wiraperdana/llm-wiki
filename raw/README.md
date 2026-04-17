# raw/ — Source documents

This directory holds **immutable source material**. The LLM agent reads from here but must never modify files in `raw/` unless explicitly asked.

## What goes here

- Articles clipped from the web (via Obsidian Web Clipper)
- PDFs (papers, reports, books)
- Meeting transcripts, podcast transcripts
- Your own raw notes / journal entries
- CSVs, data files
- Images / screenshots (in `assets/`)

## Naming

Any filename you like — but ISO-date prefixes help (`2026-04-17-some-article.md`) so files sort chronologically.

## How to add a source

1. Drop the file here.
2. Open your LLM agent in the repo root.
3. Say *"ingest `raw/<filename>`"*.
4. The agent will produce a summary page at `wiki/sources/` and update everything else.

## `assets/`

Downloaded images live here. Obsidian Web Clipper + the "Download attachments for current file" hotkey will populate it automatically once configured (see [`../AGENTS.md`](../AGENTS.md) §7).
