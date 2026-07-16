---
name: Reopen Rate Report
description: Someone wants the reopen rate sliced by tech, client, and category for a period, with a first read on why tickets are bouncing back.
category: Reporting & Analytics
tools: [search_tickets, search_members, search_clients, list_boards, list_ticket_statuses]
---

# Reopen Rate Report

Report how often resolved/closed tickets get reopened, broken out by tech, client, and category, with a root-cause angle on the reopens so the fix targets the driver. This is the periodic reporting view; for a deep single-cohort teardown of why reopens happen, cross-reference Reopen Forensics.

## When to use

- "What's our reopen rate?" / "which categories keep coming back?"
- A QA lead wants reopen trends by team or by client for a review.
- Suspicion that "resolved" is being used to stop the SLA clock rather than to fix the issue.

## Steps

1. Confirm the period, boards in scope (list_boards), and how a reopen is identified for this tenant — a status transition back out of a resolved/closed status, a reopen count field, or a re-open within N days of close. State the definition; it drives every number.
2. Establish the denominator: resolved/closed tickets in the period. Run split searches per board and per status path (result-cap honesty: many small searches; record caps).
3. Compute the overall reopen rate (reopens ÷ resolved), then slice it three ways: by category/type, by client, and by resolving tech.
4. For each slice show the rate AND the denominator — a 100% reopen rate on 1 ticket is noise; rank by volume-weighted impact, not raw percentage.
5. **Root-cause angle** — sample reopened tickets in the top slices and read the threads for the pattern: premature close, incomplete fix, recurring underlying problem, client-side recurrence, misrouted resolution. Group and rank the causes.
6. Output: overall rate + trend vs prior period, the three slice tables, the ranked cause list, 2–3 targeted fixes, and a methodology note (reopen definition, boards, period, searches, caps, exclusions).

## Guardrails

- Fix the reopen definition first and state it; "reopen" means different things across PSAs and the number is meaningless without it.
- Report rate and denominator together everywhere; suppress or caveat slices below a minimum ticket count.
- The per-tech slice is a coaching and process signal, not a leaderboard — aggregate patterns, role-aware framing (harder categories reopen more), never rank people by reopen count alone.
- Distinguish a legitimate reopen (issue genuinely recurred) from a client reply-after-close that auto-reopens; if the data cannot, say so.
- Never present capped searches as exact totals — disclose and estimate.
- Do not invent ticket numbers or causes; if a thread is ambiguous, count it as unclassified. For the forensic root-cause deep dive, cross-reference Reopen Forensics.
