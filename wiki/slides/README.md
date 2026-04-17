# slides/ — Marp decks

Marp decks generated from wiki content. See [`../templates/_slide.md`](../templates/_slide.md) for the template.

## Workflow

1. Ask the agent: *"make a Marp deck about X"* (or *"turn `syntheses/foo.md` into slides"*).
2. Agent creates `wiki/slides/<slug>.md` with `marp: true` frontmatter.
3. Open in Obsidian with the Marp plugin for live preview, or run `marp slides/<slug>.md` locally to export PDF/PPTX.

## Principles

- Slides should **cite wiki pages**, not raw sources directly. The wiki is the layer of compiled knowledge; slides restate it visually.
- Keep one thought per slide. Bullets, not paragraphs.
- Images reference `../../raw/assets/...` so they survive export.

## Listing

_No decks yet. The first one you generate will appear here._
