---
name: Halo Recurring Tickets
description: For desks synced to HaloPSA — handle Halo's template-generated recurring tickets and parent/child structures correctly: work the instance, never the template, and respect parent/child closure rules.
category: PSA-Specific
tools: [search_tickets, update_ticket, add_ticket_note]
---

# Halo Recurring Tickets

Applies to: **HaloPSA**. Halo generates tickets from templates on a schedule (maintenance checks, backup reviews, recurring invoicing tasks) and supports parent/child ticket structures. The recurring instance, the template that spawned it, and any parent ticket are different objects — and acting on the wrong one either breaks the recurrence or orphans children.

## When to use

- A ticket on a Halo desk looks auto-generated ("scheduled maintenance", identical monthly tickets).
- "Cancel this recurring ticket" — which needs disambiguation before anything is touched.
- Working a parent ticket with open children, or vice versa.

## Steps

1. Re-fetch the ticket with `search_tickets` at full detail and determine what you are holding: a recurring **instance** (one occurrence), a **parent** with children, or a **child**. Evidence: template-identical titles on a cadence (`search_tickets` for prior occurrences), parent/child references in the ticket detail.
2. For recurring instances: work and close the instance normally. Closing an instance never stops the recurrence — the next occurrence will still spawn. Say this explicitly when a requester asks to "stop these tickets": stopping recurrence is a template change made in Halo by an admin; flag it rather than simulating it by closing instances.
3. If an instance is obsolete (the underlying maintenance no longer applies), close it with a plain-text note stating why, and separately recommend the template be retired — two distinct outputs, never conflated.
4. For parent/child structures: children carry the work; the parent tracks the whole. Do not close a parent with open children — sweep the children first (`search_tickets`), and either complete them or explicitly note why the parent closes despite them, per the desk's convention. Closing a child updates only that child; verify the parent reflects reality afterwards.
5. When a recurring instance keeps arriving broken (wrong client, wrong assignment, stale checklist), collect the evidence across 2–3 occurrences and output a template-correction request for the Halo admin — include ticket numbers of the occurrences as proof.
6. Output: object type identified (instance/parent/child/template-issue), action taken or proposed, and any template-level recommendation clearly labeled as *requires Halo admin*.

## Guardrails

- **Never attempt to edit or stop the recurrence itself from Thread** — templates live in Halo config; the skill's job is the instance plus a well-evidenced recommendation.
- **Sync lag:** re-fetch full ticket detail before closing anything — a child may have been reopened or a sibling spawned Halo-side since your last read.
- Do not bulk-close recurring instances as "noise" without confirmation; a skipped maintenance instance is a missed contractual obligation on many desks.
- Parent/child closure ordering is a desk convention — when undocumented, ask rather than assume Halo's default.
- Do not invent template names or recurrence schedules; report the observed cadence ("appears monthly, last 3 occurrences: <dates>") and let evidence speak.

## Cross-references

- Status mechanics on close: `skills/psa-specific/halo-status-actions`
- Noise decisions: `skills/triage-and-routing/noise-auto-close`
