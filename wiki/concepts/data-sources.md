# Data Sources

What data Meridian has offered to share, and what's known about each source's shape and quality.

**Sources:** `raw/client-brief.md`

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
