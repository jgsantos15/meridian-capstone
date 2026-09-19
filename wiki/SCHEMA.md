# Wiki Schema — Meridian Markets Research Wiki

This document defines how this wiki works: what's immutable, what's AI-owned, and the rules for creating and updating pages. Read this before ingesting a new source, answering a query, or creating a page.

## Layers

- **`raw/`** — immutable source documents. Read-only. Never edit, rename, or delete anything here. Currently: `raw/client-brief.md`. Will grow to include the signed NDA, the POS/loyalty/labor data extract (once released), and interview notes/transcript after the stakeholder interview.
- **`wiki/`** — AI-generated and maintained markdown. Freely created, edited, and regenerated as new raw sources arrive or new questions get answered.
- **`wiki/SCHEMA.md`** (this file) — governs both layers above. Evolves through collaboration between the user and the AI.

## Folder structure

```
wiki/
  SCHEMA.md
  index.md
  log.md
  entities/          — pages about people and organizations
  concepts/           — pages about topics and themes
  open-questions.md   — living list of questions to raise with the client
```

## Page conventions

- Filenames: kebab-case, `.md` extension (e.g. `pasadena-expansion.md`).
- Every page opens with:
  1. A one-line purpose statement (what this page is about, in plain language).
  2. A `**Sources:**` line citing the `raw/` file(s) and any prior wiki pages that informed it.
- Cross-references between wiki pages use standard markdown relative links, e.g. `[Data Sources](../concepts/data-sources.md)`.
- Categories: `entities/` for people/organizations, `concepts/` for topics, plus the standalone `open-questions.md`.

## Operations

### Ingest
Triggered when a new file lands in `raw/`.
1. Read the new source in full.
2. Create or update the relevant wiki pages (entity/concept pages as appropriate).
3. Update `index.md` with any new or changed pages.
4. Append one line to `log.md` describing what happened.

### Query
Triggered when someone asks a question this wiki might answer.
1. Search wiki pages for relevant content.
2. Answer with citations back to the specific wiki page(s) and, through them, the underlying `raw/` source.
3. If the answer is durable and valuable (not just a one-off), file it into the relevant wiki page rather than letting it live only in conversation.

There is no dedicated *lint* operation in this version of the wiki — the team is small and the corpus is tiny. Add one later if the wiki grows enough that staleness or contradictions become a real risk.

## Data-handling rule

This project has an NDA (see `raw/` once it lands) and a companion checklist at `docs/data-handling-checklist.md` governing what data can and can't be used with AI tools.

**Rule for this wiki:** any future *restricted* raw source (loyalty program data, labor scheduling/hours, raw or itemized POS transactions) gets logged in `index.md` and `log.md` as received — but its *content* is never summarized, quoted, or reproduced into a wiki page beyond what `docs/data-handling-checklist.md` already clears for AI-tool use. A wiki page built from a restricted source describes that the source exists and what it covers in general terms (e.g., "loyalty extract received, covers ~40,000 members' purchase history") — never its actual contents (figures, names, individual records).

Sales totals by store/week and store attributes are cleared for AI-tool use per the checklist and may be described normally in wiki pages.
