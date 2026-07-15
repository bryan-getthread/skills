---
name: Request Fulfillment SLA
description: Requests are not incidents — stop measuring a new-user setup against a break-fix SLA. Define separate fulfillment targets per request type and report fulfillment time honestly, including where ticket data can't prove what happened.
category: Change & Problem Management
tools: [search_tickets, add_ticket_note, list_boards, list_ticket_statuses, list_ticket_priorities]
---

# Request Fulfillment SLA

Incident SLAs answer "how fast do we restore service?" Request SLAs answer "how fast do we deliver the thing you asked for?" — different clocks, different fairness rules, different reports. This skill separates the two, sets fulfillment targets per request type, and produces fulfillment-time reporting that admits what the ticket data can and can't show.

## When to use

- "What's our SLA for new-user requests?" / "set fulfillment targets for the catalog."
- A client review needs request-fulfillment reporting separated from incident response stats.
- Requests are breaching incident SLAs (or vice versa) because both run on one policy.
- Auditing whether request tickets are even classified distinctly enough to measure.

## Steps

1. **Classification audit first** — the measurement is only as good as the Incident/Request split: sample recent tickets (search_tickets per board) and check whether requests are distinguishable (type field, board, or request source). If they aren't, report that honestly and stop at the recommendation: fix classification at intake (intake-classification-tree in triage-and-routing) before any SLA numbers are quoted, because numbers computed over a mixed population are fiction.
2. **Define the request clock**, which differs from the incident clock in three documented ways:
   - It starts when the request is *complete* — required information present and any client-side approval received. Waiting on the client's approver pauses the clock (and the ticket record must show that wait explicitly for the pause to be defensible).
   - It targets *fulfillment* (the thing delivered and confirmed), not first response — though a first-acknowledgment target is still worth keeping.
   - It's calendar-realistic per type: a password reset in hours; a new-user setup in days (hardware, licenses); anything gated on procurement gets a "subject to vendor lead time" clause rather than a fake fixed number.
3. **Propose targets per request type** from the catalog (service-catalog-design) using observed evidence: for each type, pull fulfillment times of well-handled instances and propose targets the desk actually achieves at ~80–90% today. Targets set from aspiration produce permanent red dashboards and SLA fatigue.
4. Document per type: clock start definition, pause conditions, the fulfillment event that stops the clock ("account created and requester confirmed access," not "ticket closed"), and the target. Route into the desk's SLA policy documentation via its KB pipeline (human-approved before anything is quoted to clients — client-facing SLA commitments are contractual decisions, not desk defaults).
5. **Fulfillment-time reporting** (the recurring output): per request type over the period — volume, median and 90th-percentile fulfillment time, %-within-target, and breach list with per-ticket reasons where evidence shows them (client-approval waits vs. desk delay — separate these; they have different owners). State the caveats every time: tickets closed without a recorded fulfillment confirmation are reported as "closure time, fulfillment unconfirmed," clock-pause data is only as good as the status hygiene behind it, and capped searches make counts minimums.
6. Recommend follow-ups the data supports: types consistently beating target (tighten, or automate — intent candidates), types consistently missing (understaffed step, or a target that was never realistic), and status-hygiene gaps that corrupt the clock (route to queue-hygiene work in qa-and-closure).

## Guardrails

- Never quote SLA performance over a population where incidents and requests aren't reliably separated — the classification audit gates everything else.
- Ticket-data honesty is the product: closure time is not fulfillment time, unpaused waiting-on-client time is not desk delay, and the report labels each number by what it actually measures.
- Proposed targets are internal proposals; committing SLAs to clients is a human/contract decision, always.
- Clock pauses require ticket evidence (status or note) — a retroactive "we were waiting on them" without record doesn't reclassify a breach.
- Result-cap honesty: capped searches are reported as partial counts, never extrapolated silently.
