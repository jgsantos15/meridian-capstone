# Research Wiki Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Stand up the `wiki/` layer described in the design spec — schema doc, log, index, and a first ingest of `raw/client-brief` into entity/concept pages plus a seeded open-questions list — so the team has a working knowledge base to prep for the Dana Okafor stakeholder interview.

**Architecture:** Three-layer LLM-wiki pattern (karpathy). `raw/` stays untouched. `wiki/SCHEMA.md` defines conventions once, then five content tasks populate `wiki/` by hand-authoring the pages the spec already specifies verbatim. No code, no automated test framework — every file's content is fully specified in this plan; "done" means the file exists with exactly that content and passes a grep-based presence check.

**Tech Stack:** Plain markdown files, git. No build step, no runtime.

**Spec:** `docs/superpowers/specs/2026-09-11-research-wiki-design.md`

## Global Constraints

- `raw/` is immutable — no task may create, edit, or move anything under `raw/`.
- All new files live under `wiki/`, at the repo root, as a sibling of `raw/` (never nested inside it).
- Filenames: kebab-case, `.md` extension.
- Every wiki page (not `index.md` or `log.md`) opens with a one-line purpose statement, then a `**Sources:**` line.
- Cross-references between wiki pages use standard markdown relative links.
- Categories: `entities/`, `concepts/`, plus the standalone `open-questions.md`.
- No lint operation in this version.
- Restricted-data rule from `docs/data-handling-checklist.md` must be stated in `wiki/SCHEMA.md` verbatim in spirit: restricted sources get logged, never summarized into page content.
- `log.md` entries: minimal format — one line per entry, `- YYYY-MM-DD — <summary>`.
- `index.md`: grouped bullet list by category (Entities / Concepts / Living pages), not a table.

---

### Task 1: Schema and log scaffolding

**Files:**
- Create: `wiki/SCHEMA.md`
- Create: `wiki/log.md`

**Interfaces:**
- Consumes: nothing (first task).
- Produces: the conventions every later task follows (page format, operations, data-handling rule) and the log file Task 5 appends to.

- [ ] **Step 1: Create `wiki/SCHEMA.md`**

```markdown
# Wiki Schema — Meridian Markets Research Wiki

This document defines how this wiki works: what's immutable, what's AI-owned, and the rules for creating and updating pages. Read this before ingesting a new source, answering a query, or creating a page.

## Layers

- **`raw/`** — immutable source documents. Read-only. Never edit, rename, or delete anything here. Currently: `raw/client-brief`. Will grow to include the signed NDA, the POS/loyalty/labor data extract (once released), and interview notes/transcript after the stakeholder interview.
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
```

- [ ] **Step 2: Create `wiki/log.md`**

```markdown
# Wiki Log

Append-only. One line per ingest or notable query, newest entry at the bottom.
```

- [ ] **Step 3: Verify both files exist with required sections**

Run:
```bash
test -f wiki/SCHEMA.md && test -f wiki/log.md && echo "files exist: OK"
grep -c "^## " wiki/SCHEMA.md
grep -q "Data-handling rule" wiki/SCHEMA.md && echo "data-handling rule present: OK"
grep -q "raw/" wiki/SCHEMA.md && echo "raw/ referenced: OK"
```
Expected: `files exist: OK`, a count of 5 (Layers, Folder structure, Page conventions, Operations, Data-handling rule), and both `OK` lines.

**Done looks like:** `wiki/SCHEMA.md` and `wiki/log.md` exist with the exact content above.
**How you check it:** run the Step 3 commands — all four checks print `OK` / the count 5. Then open `wiki/SCHEMA.md` yourself and confirm it reads clearly to someone with zero context on the project.

- [ ] **Step 4: Commit**

```bash
git add wiki/SCHEMA.md wiki/log.md
git commit -m "Scaffold research wiki schema and log"
```

---

### Task 2: Entity pages

**Files:**
- Create: `wiki/entities/meridian-markets.md`
- Create: `wiki/entities/dana-okafor.md`

**Interfaces:**
- Consumes: `wiki/SCHEMA.md` conventions from Task 1 (page format, link style).
- Produces: two pages that `wiki/concepts/*.md` (Task 3), `wiki/open-questions.md` (Task 4), and `wiki/index.md` (Task 5) link to as `../entities/meridian-markets.md` and `../entities/dana-okafor.md` (or `entities/...` from `index.md`/`open-questions.md` at the wiki root).

- [ ] **Step 1: Create `wiki/entities/meridian-markets.md`**

```markdown
# Meridian Markets

Specialty grocery chain and the client for this engagement — who they are and how they compete.

**Sources:** `raw/client-brief`

## Overview
- Specialty grocery chain with **14 stores** across Los Angeles, Orange, and Ventura counties.
- Roughly **$78M** in annual revenue.
- About **620 employees**.
- Competes on prepared foods, local sourcing, and a smaller footprint than the national chains.

## Growth
- Grew from **6 stores to 14 in five years**.
- Growth came mostly from taking over leases from chains that pulled out of neighborhoods Meridian considered underserved.
- Growth has been uneven: some stores took off immediately, others have been slower to find their footing (specific stores not yet identified — see [Open Questions](../open-questions.md)).

See also: [Growth Timeline](../concepts/growth-timeline.md), [Pasadena Expansion](../concepts/pasadena-expansion.md).
```

- [ ] **Step 2: Create `wiki/entities/dana-okafor.md`**

```markdown
# Dana Okafor

VP of Operations at Meridian Markets, and the primary stakeholder for this engagement.

**Sources:** `raw/client-brief`

## Role
- VP of Operations, Meridian Markets.
- Wrote the client brief (dated August 2026) that kicked off this engagement.

## Reaching her
- **Email is best.**
- Travels **Tuesdays and Wednesdays**; slow to reply — don't read silence as a problem.
- Her assistant can schedule time but **cannot answer questions about the analytics**.

## What she's said she wants
- A dashboard showing sales performance by store and category, to inform the next-location decision.
- Data to validate (or challenge) the assumption that Pasadena is the obvious next site — see [Pasadena Expansion](../concepts/pasadena-expansion.md).
- Better use of the loyalty program data, which she believes has gone largely unanalyzed.
- Something preliminary to show the board in three weeks, even if incomplete.

See also: [Growth Timeline](../concepts/growth-timeline.md), [Terms of Engagement](../concepts/terms-of-engagement.md).
```

- [ ] **Step 3: Verify both pages exist with required structure**

Run:
```bash
for f in wiki/entities/meridian-markets.md wiki/entities/dana-okafor.md; do
  test -f "$f" && grep -q "^\*\*Sources:\*\*" "$f" && echo "$f: OK"
done
```
Expected: both files print `OK`.

**Done looks like:** both entity pages exist, each opening with a purpose line and a `**Sources:**` line, containing the facts listed above.
**How you check it:** run the Step 3 command (both print `OK`), then read both pages and confirm every fact matches `raw/client-brief` (14 stores, $78M, 620 employees, 6→14 in five years, Dana's role and availability).

- [ ] **Step 4: Commit**

```bash
git add wiki/entities/meridian-markets.md wiki/entities/dana-okafor.md
git commit -m "Ingest client brief: entity pages"
```

---

### Task 3: Concept pages

**Files:**
- Create: `wiki/concepts/data-sources.md`
- Create: `wiki/concepts/terms-of-engagement.md`
- Create: `wiki/concepts/pasadena-expansion.md`
- Create: `wiki/concepts/growth-timeline.md`

**Interfaces:**
- Consumes: `wiki/SCHEMA.md` conventions (Task 1); links to `wiki/entities/meridian-markets.md` and `wiki/entities/dana-okafor.md` (Task 2) as `../entities/....md`; references `docs/data-handling-checklist.md` (existing file from a prior task, unchanged).
- Produces: four pages that `wiki/open-questions.md` (Task 4) and `wiki/index.md` (Task 5) link to as `concepts/<name>.md`.

- [ ] **Step 1: Create `wiki/concepts/data-sources.md`**

```markdown
# Data Sources

What data Meridian has offered to share, and what's known about each source's shape and quality.

**Sources:** `raw/client-brief`

## What's on offer
| Source | Size / shape | Notes |
|---|---|---|
| POS transactions | ~3 years of history | Migrated to a new POS system in spring 2026 — see Data Quality below |
| Loyalty program membership & purchase history | ~40,000 members | Per Dana: "I don't think we've ever really used that data" |
| Labor scheduling & hours | Not yet sized | — |
| Store attributes | 14 stores | Square footage, opening date, lease terms |

## Access
Extracts are released by IT (contact: **Marcus**) only after the NDA is signed. See [Terms of Engagement](terms-of-engagement.md).

## Data quality: POS migration
Meridian migrated to a new POS system in spring 2026, which Dana describes as "an improvement." This is a data-handling risk, not a confidentiality one: any pre/post comparison across the ~3-year POS history should be checked for a discontinuity around the migration date before being trusted. See `docs/data-handling-checklist.md` for the full checklist item.

## Sensitivity
Per `docs/data-handling-checklist.md`:
- **Restricted** (never touches AI tools): loyalty data, labor scheduling/hours, raw/itemized POS transactions.
- **Cleared** (OK with AI tools): sales totals by store/week, store attributes.

See also: [Open Questions](../open-questions.md).
```

- [ ] **Step 2: Create `wiki/concepts/terms-of-engagement.md`**

```markdown
# Terms of Engagement

The confidentiality and process rules governing this engagement, per the client brief and the NDA that follows it.

**Sources:** `raw/client-brief`

## NDA
- An NDA follows the client brief and "covers everything below" in the brief — i.e., all data-handling terms.
- IT will release a data extract only **after the NDA is signed**.
- The named IT contact for extracts is **Marcus**.

## AI-tool restriction
- Customer records and employee data — including the loyalty program data, the labor schedules, and *any excerpts of them* — must never go into ChatGPT, Claude, Copilot, or any other AI tool. Per Dana: "Our counsel is firm on this, and it is not negotiable."
- Sales totals by store and week, and the store attributes, are explicitly cleared for use with AI tools.
- Full operational checklist: `docs/data-handling-checklist.md`.

## Timeline
- Engagement is expected to run **about eight weeks**.
- A **preliminary** view is wanted for the board meeting in **three weeks** — even if incomplete.

See also: [Data Sources](data-sources.md), [Dana Okafor](../entities/dana-okafor.md).
```

- [ ] **Step 3: Create `wiki/concepts/pasadena-expansion.md`**

```markdown
# Pasadena Expansion

The specific business decision this engagement is meant to inform: whether Pasadena is the right next store location.

**Sources:** `raw/client-brief`

## The hypothesis
- Leadership already believes Pasadena is "the obvious next step" for a new store.
- They want data to back that up before committing — this is the stated reason for wanting the store/category performance dashboard.

## What's still unclear
- What "sales performance ... by category" should mean for this decision — margin, unit velocity, both? (See [Open Questions](../open-questions.md).)
- How the uneven performance across the existing 14 stores (see [Growth Timeline](growth-timeline.md)) should factor into the Pasadena case.

## Why it matters for the interview
This is the decision the whole engagement is oriented around. Understanding exactly what would make Dana and leadership confident in — or reconsider — the Pasadena hypothesis is the highest-value thing to get out of the stakeholder interview.

See also: [Meridian Markets](../entities/meridian-markets.md), [Dana Okafor](../entities/dana-okafor.md).
```

- [ ] **Step 4: Create `wiki/concepts/growth-timeline.md`**

```markdown
# Growth Timeline

How Meridian got from 6 stores to 14, and the project's own timeline.

**Sources:** `raw/client-brief`

## Company growth
- 6 stores → 14 stores over **five years**.
- Growth mostly came from taking over leases from national chains that pulled out of neighborhoods Meridian considered underserved.
- Performance across the 14 stores has been uneven — some took off immediately, others have been slower to find their footing. The brief doesn't name which stores fall into which group. (See [Open Questions](../open-questions.md).)

## Engagement timeline
- Total engagement: **~8 weeks**.
- **3 weeks** in: a preliminary view is wanted for the board meeting, even if incomplete.

See also: [Meridian Markets](../entities/meridian-markets.md), [Pasadena Expansion](pasadena-expansion.md).
```

- [ ] **Step 5: Verify all four pages exist with required structure**

Run:
```bash
for f in wiki/concepts/data-sources.md wiki/concepts/terms-of-engagement.md wiki/concepts/pasadena-expansion.md wiki/concepts/growth-timeline.md; do
  test -f "$f" && grep -q "^\*\*Sources:\*\*" "$f" && echo "$f: OK"
done
```
Expected: all four files print `OK`.

**Done looks like:** all four concept pages exist, each opening with a purpose line and a `**Sources:**` line, and each references `docs/data-handling-checklist.md` or `raw/client-brief` accurately.
**How you check it:** run the Step 5 command (four `OK` lines), then read each page against `raw/client-brief` for factual accuracy, and click through each cross-reference link to confirm it resolves to a real file (entity pages exist from Task 2; `open-questions.md` will exist after Task 4 — recheck those two specific links once Task 4 lands).

- [ ] **Step 6: Commit**

```bash
git add wiki/concepts/data-sources.md wiki/concepts/terms-of-engagement.md wiki/concepts/pasadena-expansion.md wiki/concepts/growth-timeline.md
git commit -m "Ingest client brief: concept pages"
```

---

### Task 4: Open questions

**Files:**
- Create: `wiki/open-questions.md`

**Interfaces:**
- Consumes: links to `wiki/concepts/growth-timeline.md`, `wiki/concepts/pasadena-expansion.md`, `wiki/concepts/data-sources.md` (Task 3).
- Produces: the living page that `wiki/index.md` (Task 5) links to, and that the entity/concept pages already link back to as `../open-questions.md`.

- [ ] **Step 1: Create `wiki/open-questions.md`**

```markdown
# Open Questions

Questions to raise with Dana Okafor in the stakeholder interview, seeded from gaps in `raw/client-brief`. Each entry links to the wiki page whose gap prompted it. As ingests happen (NDA, data extract, interview notes) and a question gets answered, move the answer into that page's content and remove the question from this list — don't let answered questions linger here.

**Sources:** `raw/client-brief`

- [ ] **Which stores took off immediately vs. were slower to find their footing — can you name them?**
  The brief states this happened but never identifies which stores. → [Growth Timeline](concepts/growth-timeline.md)

- [ ] **What does "category performance" mean to leadership — margin, unit velocity, or both?**
  The brief asks for "sales performance by store and category" but never defines what "performance" means for a category. → [Pasadena Expansion](concepts/pasadena-expansion.md)

- [ ] **Has the loyalty data ever been analyzed before, even informally?**
  Dana: "I don't think we've ever really used that data." → [Data Sources](concepts/data-sources.md)

- [ ] **What does the board actually need to see in three weeks — a finished view or a work-in-progress?**
  Affects scoping for the first stretch of the engagement. → [Growth Timeline](concepts/growth-timeline.md)

- [ ] **Are there data quality concerns from the POS migration beyond what IT would surface on its own?**
  The brief mentions the migration positively but doesn't address continuity risk. → [Data Sources](concepts/data-sources.md)
```

- [ ] **Step 2: Verify the page exists with all five questions**

Run:
```bash
test -f wiki/open-questions.md && echo "file exists: OK"
grep -c "^- \[ \]" wiki/open-questions.md
```
Expected: `file exists: OK` and a count of `5`.

**Done looks like:** `wiki/open-questions.md` exists with exactly the five seeded questions above, each linking to the concept page whose gap prompted it.
**How you check it:** run the Step 2 command (`OK` and count `5`), then click each of the five links and confirm they resolve to the concept pages created in Task 3.

- [ ] **Step 3: Commit**

```bash
git add wiki/open-questions.md
git commit -m "Seed open questions for stakeholder interview"
```

---

### Task 5: Catalog and log entry

**Files:**
- Create: `wiki/index.md`
- Modify: `wiki/log.md` (append one line)

**Interfaces:**
- Consumes: every page created in Tasks 1–4 (links to all of them).
- Produces: nothing further downstream — this is the closing task of the initial ingest.

- [ ] **Step 1: Create `wiki/index.md`**

```markdown
# Index

Catalog of every page in this wiki, grouped by category. Update this whenever a page is created, renamed, or removed.

## Entities
- [Meridian Markets](entities/meridian-markets.md) — the client: 14 stores, ~$78M revenue, growth history, competitive positioning.
- [Dana Okafor](entities/dana-okafor.md) — VP of Operations, primary stakeholder, how to reach her.

## Concepts
- [Data Sources](concepts/data-sources.md) — the four data types on offer, access process, POS migration data-quality note.
- [Terms of Engagement](concepts/terms-of-engagement.md) — NDA, AI-tool restriction, timeline.
- [Pasadena Expansion](concepts/pasadena-expansion.md) — the next-site decision this engagement is meant to inform.
- [Growth Timeline](concepts/growth-timeline.md) — company growth history and the engagement's own timeline.

## Living pages
- [Open Questions](open-questions.md) — questions to raise with Dana Okafor in the stakeholder interview.
```

- [ ] **Step 2: Append the ingest entry to `wiki/log.md`**

Add this line to the end of `wiki/log.md`, after the header:

```markdown
- 2026-09-11 — Ingested `raw/client-brief`; created 6 pages (2 entities, 4 concepts) and seeded `open-questions.md` with 5 questions.
```

- [ ] **Step 3: Verify the catalog and log**

Run:
```bash
test -f wiki/index.md && echo "index exists: OK"
grep -c "^- \[" wiki/index.md
grep -q "2026-09-11" wiki/log.md && echo "log entry present: OK"
```
Expected: `index exists: OK`, a count of `7` (6 pages + open-questions), `log entry present: OK`.

- [ ] **Step 4: Check every link in the wiki resolves**

Run:
```bash
for f in wiki/*.md wiki/entities/*.md wiki/concepts/*.md; do
  grep -oE '\]\(([^)]+\.md)\)' "$f" | sed -E 's/\]\(//; s/\)$//' | while read -r link; do
    dir=$(dirname "$f")
    target=$(realpath -m "$dir/$link" --relative-to=.)
    if [ ! -f "$target" ]; then
      echo "BROKEN LINK in $f -> $link"
    fi
  done
done
echo "link check complete"
```
Expected: no `BROKEN LINK` lines, just `link check complete`.

**Done looks like:** `wiki/index.md` lists all 7 pages grouped correctly, `wiki/log.md` has the ingest entry appended after its header, and every markdown link across the wiki resolves to a real file.
**How you check it:** run Steps 3–4's commands (all `OK`/expected counts, no broken links), then open `wiki/index.md` and confirm each linked page's one-line description actually matches that page's content.

- [ ] **Step 5: Commit**

```bash
git add wiki/index.md wiki/log.md
git commit -m "Finalize wiki index and log entry for initial ingest"
```

---

## Final verification (whole plan)

- [ ] Run the full link check from Task 5, Step 4 one more time against the final state of `wiki/`.
- [ ] Confirm `git log --oneline -5` shows five commits, one per task, and `git status` is clean.
- [ ] Read `wiki/open-questions.md` end to end as if prepping for the interview tomorrow — confirm it reads as immediately useful, not just a mechanical gap list.

**Done looks like:** a clean working tree, five commits, zero broken links, and an open-questions page you'd actually walk into the interview with.
**How you check it:** the commands above, plus your own read-through of `wiki/open-questions.md` and `wiki/index.md`.
