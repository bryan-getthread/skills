---
name: Onboarding Offboarding Volume
description: Someone wants onboarding and offboarding ticket volume and cycle time per client — how much identity churn each account generates and how fast we turn it around.
category: Reporting & Analytics
tools: [search_tickets, search_clients, list_boards, list_ticket_statuses]
---

# Onboarding Offboarding Volume

Report the volume of new-hire onboarding and departure offboarding tickets per client, plus how long each takes to complete — a workload, agreement-scoping, and security-timeliness signal in one. Slow offboarding especially is a security exposure, not just a metric.

## When to use

- "How many onboards/offboards did we do per client?" / "how fast do we turn them around?"
- Agreement review — is a client's identity churn matching what the contract assumes?
- A security check that departures are being deprovisioned promptly.

## Steps

1. Confirm the period, boards in scope (list_boards), and how onboarding vs offboarding tickets are identified for this tenant — type/category, a template, or a title convention. State the identification rule; it defines the population.
2. Resolve the client set (search_clients) and the grouping key (end-customer vs parent/agreement).
3. Run split searches per client for onboarding and for offboarding separately (result-cap honesty; record caps).
4. **Volume** — onboards and offboards per client, ranked, with totals and the net (a client with heavy offboarding and little onboarding may be shrinking — worth flagging to the account team).
5. **Cycle time** — created-to-completed duration per type, reported as median and p90 (not just mean), per client where volume allows. Call out offboarding cycle time specifically against any deprovisioning SLA or expectation — a long tail here is a security finding.
6. Output: the per-client volume table, the cycle-time distribution per type, offboarding-timeliness flags, any onboarding/offboarding imbalance worth an account note, and a methodology note (identification rule, grouping key, boards, period, caps).

## Guardrails

- State the identification rule; if onboards/offboards are not reliably distinguishable from general access tickets, say so and report only what is confidently classifiable.
- Report cycle time as median and p90 with denominators; a single average hides the slow offboards that matter most.
- Frame slow offboarding as a security-timeliness risk, not merely a throughput metric.
- Cycle time reflects process and dependencies (waiting on client, hardware lead time), not individual speed — do not rank techs here; distinguish paused clocks where possible.
- Never present capped searches as exact counts — disclose and estimate.
- Do not invent ticket counts or completion dates; exclude tickets with unreliable timestamps and say how many.
