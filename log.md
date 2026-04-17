---
title: Wiki Log
type: log
created: YYYY-MM-DD
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

_← Replace with your first entry. Example:_

Bootstrapped the repo. Created schema files and scaffolded all directories. Ready to ingest first source.
