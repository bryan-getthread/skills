---
name: Doc Importance Audit
description: Inventory the SOPs and procedures across the documentation platform and rank them by how much current ticket volume actually depends on them.
category: Documentation
tools: [search_itglue, search_hudu, notion-search, search_knowledge_base, search_tickets]
---

# Doc Importance Audit

Answer "which of our documents matter most?" with evidence: map each SOP/procedure to the
ticket volume that relies on it, so review effort and quality bars go where the load is.

## When to use

- "Which SOPs are most important to keep accurate?"
- Prioritizing a documentation review cycle with limited hours.
- Deciding which docs need owners, review cadences, or translation.
- After an acquisition/merger of doc estates: what to keep, merge, or let decay.

## Steps

1. Inventory the procedural docs: `search_itglue` / `search_hudu` where enabled, `notion-search` if the member's runbooks live in Notion, and `search_knowledge_base` for the Thread KB. Scope to one <client> or one category per run — full-estate inventories exceed result caps; audit in slices and say which slice this is.
2. For each doc, derive its subject (system, procedure, client scope) from title and content summary.
3. Measure demand: `search_tickets` per subject for the window (default: last 90 days) — how many tickets touched this procedure's territory, at what priority mix, and across how many clients.
4. Score importance per doc: ticket volume × priority weight × client breadth, with a **criticality floor** — security, backup/restore, and disaster-recovery docs rank no lower than "high" regardless of ticket volume (low volume is the point).
5. Output the ranked inventory: doc title + location, subject, tickets touched (count + examples), breadth, importance tier (Critical / High / Medium / Low), and a recommended action per tier — Critical/High: named owner + review cadence; Medium: annual check; Low: candidate for archive (verify first, see stale-doc-hygiene).
6. Flag inversions explicitly: high-ticket subjects with no doc at all belong to kb-coverage-report — list them at the end as coverage gaps, not audit rows.

## Guardrails

- **Result-cap honesty:** both doc listings and ticket counts cap; label every number that came from a capped search as a floor, and state the audited slice.
- Volume is not the only importance signal — never recommend archiving a DR/security/compliance doc on low ticket volume; low usage there is success.
- Cite real doc locations and ticket numbers; no invented inventory rows.
- Read-only: this skill ranks and recommends; archiving, assigning owners, and editing are human actions.
- Degradation: with no external doc platform enabled and no Notion connection, audit the Thread KB only and say so.
