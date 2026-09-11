# Data Handling Checklist — Meridian Markets Engagement

Source: client brief from Dana Okafor (VP Operations, Meridian Markets), August 2026, and the NDA that follows it. This checklist governs how the team handles Meridian's data across all four workshops.

Two sensitivity tiers, based directly on the brief's terms of engagement:

- **🔒 Restricted** — loyalty program data, labor scheduling/hours. Never touches any AI tool, in any form, including excerpts.
- **✅ Cleared for AI tools** — sales totals by store and week, store attributes. Safe to use with ChatGPT, Claude, Copilot, or similar.

POS transaction data is split across tiers depending on form: raw/itemized transactions are treated as Restricted (they can reveal individual customer behavior once joined to loyalty IDs); store/week sales totals are Cleared.

---

## 🔒 Restricted tier

**Covers:** loyalty program membership and purchase history (~40,000 members), labor scheduling and hours, and raw/itemized POS transactions.

### Intake
- [ ] Confirm the NDA is fully signed before requesting any data extract.
  *Dana's brief: "IT can pull an extract for you once the NDA is signed."*
- [ ] Route the extract request through Marcus (IT) only, per the brief's named contact.
- [ ] Log what was received, when, and in what format (file, access grant, etc.) before anyone opens it.

### Classification & access
- [ ] Tag loyalty and labor files as Restricted immediately on receipt — before anyone previews them.
- [ ] Limit access to team members actively working with this data; don't forward files or credentials outside the team.
- [ ] Treat any excerpt, sample, screenshot, or copy-pasted snippet of this data as equally Restricted.
  *Brief: "That includes the loyalty program data, the labor schedules, and any excerpts of them."*

### Analysis & tool use
- [ ] Never paste, upload, or otherwise input this data (or excerpts) into ChatGPT, Claude, Copilot, or any other AI tool.
  *Brief: "Our counsel is firm on this, and it is not negotiable."*
- [ ] Do all analysis of this data in local/secured environments (e.g., your own scripts, BI tools without AI features enabled) — not in AI-assisted notebooks or chat interfaces.
- [ ] Before generating any output derived from this data (charts, summaries, dashboard tiles) that you *do* want to discuss with an AI tool, confirm it's been aggregated enough that no individual member/employee is identifiable — see the boundary note below.
- [ ] If in doubt whether a derived figure is safe to use with AI tools, treat it as Restricted until confirmed otherwise.

### Storage & sharing
- [ ] Store Restricted files only in locations approved for the engagement (not personal drives, personal email, or general-purpose cloud storage tied to personal accounts).
- [ ] Do not include Restricted data in slides, reports, or board materials in raw or lightly-aggregated form — only fully de-identified, store/category-level rollups belong in shareable outputs.
- [ ] Strip or mask any customer/employee identifiers before including derived figures in shared deliverables.

### Retention & disposal
- [ ] Confirm data retention/disposal terms in the NDA and follow them at engagement close.
- [ ] Delete local copies of Restricted extracts once the final deliverable is accepted, unless the NDA specifies otherwise.

---

## ✅ Cleared-for-AI-tools tier

**Covers:** sales totals by store and week, store attributes (square footage, opening date, lease terms).

*Brief: "Sales totals by store and week, and the store attributes, are fine to use with those tools. We understand you use them on your own work."*

### Intake
- [ ] Same NDA/extract process as above (the NDA "covers everything below," including this tier) — no separate carve-out for how the data is obtained, only for how it's later used.

### Classification & access
- [ ] Confirm any "sales totals" figure is genuinely store/week-level aggregation, not a disaggregated view that could be reversed into transaction- or customer-level detail.
- [ ] When in doubt about whether a cut of the data is aggregated enough, default to Restricted handling rather than assuming it's cleared.

### Analysis & tool use
- [ ] OK to use with AI tools (ChatGPT, Claude, Copilot) for analysis, drafting, or visualization support.
- [ ] Avoid combining Cleared data with Restricted data in the same AI-tool session/prompt, even if the Restricted portion seems incidental.

### Storage & sharing
- [ ] No special restriction beyond standard engagement confidentiality (this is still client data, just not customer/employee PII) — don't publish externally without client sign-off.

---

## Data quality note: POS migration (not a confidentiality issue, but a handling risk)

- [ ] Before trusting any pre/post comparison across the ~3-year POS history, check for a discontinuity around the system migration (brief: "We migrated to a new POS system last spring, which has been an improvement").
- [ ] Confirm with Marcus/IT whether historical data was backfilled into the new system as-is, reformatted, or partially lost in migration, and note any category/field differences between old and new POS exports.
- [ ] Flag any pre-migration vs. post-migration metric definition changes (e.g., category naming, tax handling) before using them in store comparisons — especially for the preliminary board readout, where a data artifact could be mistaken for a real trend.

---

## Quick reference

| Data source | Tier | AI-tool use |
|---|---|---|
| Loyalty membership & purchase history | 🔒 Restricted | Never, including excerpts |
| Labor scheduling & hours | 🔒 Restricted | Never, including excerpts |
| POS transactions (raw/itemized) | 🔒 Restricted | Never |
| POS sales totals by store & week | ✅ Cleared | OK |
| Store attributes (sq ft, opening date, lease terms) | ✅ Cleared | OK |
