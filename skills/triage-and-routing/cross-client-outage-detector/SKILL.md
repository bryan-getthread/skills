---
name: Cross-Client Outage Detector
description: Spot the same symptom appearing across multiple clients within a short window, flag a possible vendor or major incident, and propose a parent incident ticket.
category: Triage & Routing
tools: [search_tickets, search_clients, create_ticket, add_ticket_note]
---

# Cross-Client Outage Detector

Three clients reporting the same mail delay in an hour is not three tickets — it is probably one upstream incident. This skill detects the cross-client pattern, names the suspected shared dependency, and proposes (never silently creates) a parent incident.

## When to use

- Several tickets with a similar symptom arrived from different clients recently.
- A tech senses "haven't we seen this a few times today?" and wants it checked.
- Dispatch runs a periodic sweep for emerging vendor/major incidents.

## Steps

1. Take the symptom fingerprint from the triggering ticket (or the stated symptom): affected service, error text, and behavior — via `search_tickets`.
2. Search recent tickets (default window: 4 hours) for the same symptom family across ALL clients, split per board to respect result caps; disclose caps, since undercounting hides outages.
3. Group hits by client via `search_clients`. The pattern threshold: same symptom at 3+ distinct clients inside the window (2+ if the symptom names the same vendor/service explicitly).
4. Identify the plausible shared dependency: a common vendor, cloud service, ISP/region, or a platform the affected clients share. Say "unknown shared cause" when you cannot name one — do not guess a vendor into the record.
5. If the threshold is met, propose a parent incident: a draft `create_ticket` (title, affected-client list, symptom summary, suspected dependency, first-seen time) presented for confirmation. On approval, create it and post a plain-text note on each child ticket referencing the parent number.
6. If below threshold, report the near-pattern (counts, clients, window) and recommend a re-check interval — a negative finding is still useful.

## Guardrails

- Flag and propose; never declare. Wording stays factual: "possible upstream/vendor incident" — never "vendor X is down" or breach language without confirmation.
- Create the parent only after confirmation, and never merge the child tickets — children stay open under their own clients for per-client communication and SLA tracking; they are linked by note, not merged.
- Distinct clients means distinct companies — multiple sites of one client is a client-level incident, not a cross-client one; say which you found.
- Result-cap honesty is critical here: if searches capped, present counts as "at least N" and recommend a narrower re-run.
- Do not invent affected clients, vendors, or ticket numbers; every listed child must be a real ticket you found.

## Unattended (Flows) variant

- Follows the Unattended Output Discipline contract: this variant detects and alerts only. The entire reply is either the plain-text alert — `POSSIBLE CROSS-CLIENT INCIDENT: <symptom family> at <k> distinct clients since <first-seen time>. Tickets: #<n>, #<n>, ...` plus the suspected shared dependency or "unknown shared cause" — or exactly `NO PATTERN.`
- Deterministic inputs from the flow: the detection window, the threshold (default 3+ distinct clients, 2+ with an explicit shared vendor), and the boards to sweep.
- Threshold not met → `NO PATTERN.` — a near-miss is never reported as an incident. If any search hit a result cap, append ` (SWEEP PARTIAL)` to either reply; an undercounted sweep must say so.
- Distinct clients means distinct companies; a multi-site single-client cluster → `NO PATTERN.` (that is a client-level incident for attended handling).
- Permitted writes: none. Creating the parent incident and noting child tickets require human confirmation and stay attended. Every ticket number in the alert must be one the sweep actually returned.
