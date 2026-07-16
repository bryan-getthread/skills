---
name: Fixed Assignee Alert Routing
description: Deterministic routing rule — configured alert classes always go to a named technician, with an ordered out-of-office fallback chain and a weekly "is this rule still right" check. The rule-based counterpart to skill-based-routing.
category: Scheduling & Dispatch
tools: [search_tickets, search_members, update_ticket, add_ticket_note]
---

# Fixed Assignee Alert Routing

Some alert classes have exactly one right owner: the senior tech who owns the backup platform, the engineer who built the client's firewall stack. This skill routes those classes to the configured person every time — no inference, no load balancing — and falls back down a defined chain when that person is out. Where skill-based-routing infers the best tech from history, this skill executes a fixed rule the desk wrote down.

## When to use

- A flow fires on new tickets matching a configured alert class (e.g. "all backup-failure alerts go to <senior tech>", "all firewall alerts for the VIP client go to <engineer>").
- "Make sure every <alert class> ticket lands on <tech>'s queue."
- Weekly review: "are our fixed routing rules still pointing at the right people?"

## Steps

1. Read the ticket with `search_tickets` and match it against the configured rule table (alert class → primary assignee → ordered fallback chain). Matching is on the alert class definition (source, title pattern, board, client scope as configured) — exact per the rule, not on vibes. No rule matches → do nothing; this skill never improvises a route.
2. Verify the primary assignee is available: `search_members` for active status; check for OOO/absence signals the desk records (calendar status, an OOO flag, or a currently-set absence per desk convention). If availability cannot be determined, treat the primary as available — the fallback chain exists for known absences, not guesses.
3. If the primary is out, walk the fallback chain in order and take the first available member. If the chain is exhausted (everyone out), do NOT assign to an arbitrary person: leave unassigned, escalate per desk convention (e.g. route to the dispatch board), and say so in the note.
4. Assign with `update_ticket` and post a plain-text `add_ticket_note`: which rule matched, who was assigned, and — if a fallback was used — who was skipped and why. The note makes the deterministic decision auditable.
5. Do not change priority, status, or board unless the rule itself specifies it; this skill routes, nothing more.
6. Weekly rule check (scheduled or on request): for each configured rule, report — does the named primary still exist and remain active (`search_members`)? How many tickets did the rule route this week (`search_tickets`)? Did fallbacks fire repeatedly (a chronically absent primary means the rule is stale)? Were any matching tickets manually re-routed away from the fixed assignee after assignment (signal the rule is wrong)? Output a short plain-text report with a keep/update recommendation per rule — recommendations only; changing a rule is a human decision.

## Guardrails

- Deterministic means deterministic: only configured rules route; never extend a rule to a "similar" alert class or pick a "reasonable" assignee for an unmatched ticket. Unmatched → untouched. Inference-based assignment belongs to skill-based-routing.
- Never assign to an inactive or departed member — verify with search_members before every write; a rule pointing at a ghost gets the ticket escalated and the rule flagged, not a silent best-guess reroute.
- Fallback chain order is fixed; do not reorder by load or preference. Load-aware assignment is a different skill (workload-balancing-assignment).
- One assignment write per ticket per run. If the ticket is already assigned to the rule's target (or a valid chain member), do nothing — do not churn assignments.
- The weekly check reports and recommends; it never edits rules or reassigns historical tickets.
- Notes are plain text (PSA-synced desks).

## Unattended (Flows) variant

- Your entire reply is the internal note, verbatim, plain text: `ROUTED #<n> to <member> per rule <rule-name>.`, `ROUTED #<n> to <member> (fallback; primary OOO) per rule <rule-name>.`, `NO RULE MATCH — no action.`, or `CHAIN EXHAUSTED — left unassigned, escalated to <dispatch convention>.`
- Deterministic stops: no rule match → no action; already correctly assigned → no action; assignee identity ambiguous in search_members → no action, flag for human.
- Never send outbound messages; never change anything but the assignee (plus any field the matched rule explicitly specifies).
