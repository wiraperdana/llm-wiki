# Templates

Blank templates for each wiki page type. Copy a template, rename (lowercase-hyphenated slug), update the frontmatter, and fill in the body.

| File | For… |
| --- | --- |
| `_entity.md` | People, organizations, products, places |
| `_concept.md` | Ideas, frameworks, theories, methodologies |
| `_source.md` | Per-source summary — one per file in `raw/` |
| `_synthesis.md` | Analyses, comparisons, theses, lint reports |
| `_slide.md` | Marp slide decks |

Files starting with `_` are ignored by the wiki at query time (treat them as scaffolding, not content).

See [`../../AGENTS.md`](../../AGENTS.md) §3 for frontmatter and naming conventions.
