---
name: Intent Coverage Report
description: Someone asks which of the desk's ticket intents have automation coverage (intents, flows, skills) versus which are still fully manual — the coverage map that feeds the automation roadmap.
category: Reporting & Analytics
tools: [search_tickets, list_intents, list_flows, list_boards]
---

# Intent Coverage Report

Map what the desk's tickets are actually about against what automation exists for each theme — configured intents, flows, and (where visible) skills — producing a coverage matrix: covered, partially covered, uncovered. This is the supply-side inventory; Zero-Touch Opportunity Mining is the demand-side ranking of what to build next — run this first, feed it that.

## When to use

- "What do we have automation for, and where are the gaps?"
- Planning the automation roadmap or a quarterly automation review.
- After building several flows: "what's still manual-only on this desk?"

## Steps

1. Confirm the period for ticket-demand data (default: last full quarter) and boards in scope (list_boards).
2. **Demand side** — search_tickets per board to derive the desk's real ticket themes: top categories/types by volume, plus recurring request shapes visible in subjects (password reset, new user, printer, VPN, disk space...). Record caps; theme volumes from capped searches are labeled floors ("at least N").
3. **Supply side** — list_intents and list_flows; for each, note what theme it targets, whether it is active, and what it actually does (routes only, gathers info, or resolves end-to-end). If skills are listed for this tenant, include them; if the skills family is unavailable in this context, say the skills column is not visible rather than assuming empty.
4. **Join** — build the coverage matrix: theme / period volume / covering automation(s) / coverage depth (none / routes-triages only / partial resolution / full zero-touch). "An intent exists" is not "covered" — an intent that only tags the ticket is routing coverage, not resolution coverage; the depth column is the honest part.
5. **Gap read** — highest-volume themes with no or shallow coverage are the roadmap candidates; also flag the inverse — automations targeting themes with near-zero current volume (decommission candidates, hand those to Automation ROI Report for a verdict).
6. Output: the matrix sorted by volume, a short gap list (top 3–5 uncovered or under-covered themes with volume floors), the orphaned-automation list, an explicit pointer to run Zero-Touch Opportunity Mining to rank and spec the builds, and a methodology note — period, boards, how themes were derived, caps hit, and whether skills were visible.

## Guardrails

- Coverage depth must be verified from what the intent/flow actually does (get_intent/get_flow when in doubt), not inferred from its name — a flow named "Password Reset" that only posts a note is not resolution coverage.
- This skill reports and recommends; it never creates or modifies intents or flows. Never convert a recommendation into a completed action.
- Theme derivation from ticket text is an estimate — label theme volumes as estimates, and never present capped counts as totals.
- Aggregate, not enumerate — themes and counts, not ticket lists.
- Intent/flow tools are admin-only: if they are not available on this token, say the supply side cannot be read and stop rather than guessing at what automation exists.
