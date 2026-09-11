# Research Wiki Design — Meridian Markets Engagement

## Purpose

A research wiki to prepare for the Dana Okafor stakeholder interview, built to keep compounding as a running project knowledge base across all four workshops — not a one-off prep doc. Follows the "LLM wiki" pattern (karpathy): a three-layer system of immutable raw sources, an AI-maintained wiki layer, and a schema doc that governs conventions.

## Audience

The capstone workshop team (multiple people), not just one person. Conventions in the schema doc need to be explicit enough that any teammate can read or contribute without the original author explaining them.

## Layers

- **Raw layer** (`raw/`) — immutable source documents. AI reads, never edits. Currently just `raw/client-brief`; will grow to include the NDA, the data extract (once released), and interview notes/transcript after the interview.
- **Wiki layer** (`wiki/`) — AI-generated and maintained markdown. Freely created, edited, and regenerated as new raw sources arrive.
- **Schema layer** (`wiki/SCHEMA.md`) — conventions and workflow rules governing the other two layers. Evolves through collaboration between the user and the AI as the project's needs become clearer.

## Folder layout

```
raw/
  client-brief                    (existing, untouched)
wiki/
  SCHEMA.md                       (conventions — the schema layer)
  index.md                        (page catalog, by category)
  log.md                          (append-only ingest/query history)
  entities/
    meridian-markets.md
    dana-okafor.md
  concepts/
    data-sources.md
    terms-of-engagement.md
    pasadena-expansion.md
    growth-timeline.md
  open-questions.md               (living page, drives interview prep)
```

## `wiki/SCHEMA.md` contents

- **Layer definitions** as above — what's immutable, what's AI-owned, what this doc governs.
- **Page conventions**: kebab-case filenames; every page opens with a one-line purpose statement and a "Sources" line citing the `raw/` file(s) and any prior wiki pages that informed it; cross-references use relative markdown links between wiki pages.
- **Categories**: `entities/` (people, organizations), `concepts/` (topics/themes), plus the standalone `open-questions.md`.
- **Operations**, scoped to this project:
  - *Ingest* — a new file lands in `raw/` → read it, create/update relevant wiki pages, append an entry to `log.md`, update `index.md`.
  - *Query* — someone asks a question → answer from wiki pages with citations back to `raw/`; if the answer is durable and valuable, file it into a wiki page.
  - (No dedicated *lint* operation for now — team is small and the corpus is tiny. Add one later if the wiki grows enough that staleness/contradictions become a real risk.)
- **Data-handling rule**: explicitly references `docs/data-handling-checklist.md`. Any future restricted source (loyalty extract, labor schedules, raw/itemized POS transactions) gets logged in `index.md` and `log.md` as received, but its *content* is never summarized, quoted, or reproduced into a wiki page beyond what the checklist already clears for AI-tool use. A wiki page built from a restricted source describes that the source exists and what it covers in general terms — never its actual contents (figures, names, records).

## Initial wiki pages (first ingest, from `raw/client-brief`)

| Page | Captures |
|---|---|
| `entities/meridian-markets.md` | 14 stores across LA/OC/Ventura counties, ~$78M annual revenue, ~620 employees, competitive positioning (prepared foods, local sourcing, smaller footprint than national chains), growth from 6→14 stores in 5 years via taking over leases in underserved neighborhoods |
| `entities/dana-okafor.md` | VP of Operations; communication style and availability (travels Tuesdays/Wednesdays, slow to reply, assistant can schedule but can't answer analytics questions); how to reach her |
| `concepts/data-sources.md` | The four data types on offer — POS transactions (~3 years), loyalty membership & purchase history (~40,000 members), labor scheduling & hours, store attributes (sq ft, opening date, lease terms) — plus the spring 2026 POS migration caveat |
| `concepts/terms-of-engagement.md` | NDA gate before any extract is released, the AI-tool restriction split (restricted vs. cleared, per the data-handling checklist), Marcus (IT) as the named extract contact |
| `concepts/pasadena-expansion.md` | The stated next-site hypothesis (Pasadena) and that leadership wants data to back it up before committing — the decision the whole engagement centers on |
| `concepts/growth-timeline.md` | 6→14 stores in 5 years, uneven performance across stores (some took off immediately, others slower), the 8-week engagement timeline and 3-week preliminary board readout |

## `open-questions.md` format

Each entry: the question itself, which wiki page/gap prompted it, and why it matters for the interview. Seeded from real gaps identified in the brief:

- *"Which stores took off immediately vs. were slower to find their footing — can you name them?"* — the brief states this happened but never identifies which stores. (`growth-timeline.md`)
- *"What does 'category performance' mean to leadership — margin, unit velocity, or both?"* — the brief asks for "sales performance by store and category" but never defines what "performance" means for a category. (`pasadena-expansion.md`)
- *"Has the loyalty data ever been analyzed before, even informally?"* — brief: "I don't think we've ever really used that data." (`data-sources.md`)
- *"What does the board actually need to see in three weeks — a finished view or a work-in-progress?"* — affects scoping for Workshop 1–2. (`growth-timeline.md`)
- *"Are there data quality concerns from the POS migration beyond what IT would surface on its own?"* — the brief mentions the migration positively but doesn't address continuity risk. (`data-sources.md`)

As ingests happen (NDA, data extract, interview notes), items here get resolved and removed rather than accumulating indefinitely — an informal, manual version of the "lint" check the schema otherwise omits.

## Out of scope for this design

- A lint operation (may be added later if the corpus grows).
- Ingesting anything beyond `raw/client-brief` right now — the NDA, data extract, and interview notes will each trigger their own ingest once they exist.
- Any published/artifact view of the wiki — this is a plain-markdown, repo-native tool for now.
