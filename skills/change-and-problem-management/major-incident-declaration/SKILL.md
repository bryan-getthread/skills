---
name: Major Incident Declaration
description: Something big is breaking — run the criteria check, declare (or explicitly don't), assign the incident roles, and start the comms clock. The declaration checklist that turns "everyone panicking in chat" into a managed major incident.
category: Change & Problem Management
tools: [search_tickets, update_ticket, add_ticket_note, create_ticket, list_ticket_priorities, search_members]
---

# Major Incident Declaration

Declaring a major incident is a mode switch: normal queue rules stop, roles and a comms cadence start. The declaration itself is a checklist, and running it fast and identically every time is the whole value — including the times the right answer is "this is a P1, not a major."

## When to use

- "I think this is a major — declare it" / "should we declare on this?"
- Multiple tickets flooding in for the same client-wide or multi-client outage (cross-client-outage-detector found a cluster).
- A P1 has outgrown single-ticket handling: multiple workstreams, executive attention, client comms pressure.

## Steps

1. **Criteria check** — declare when any is true, and record which one(s):
   - Client-wide loss of a core service (auth, email, primary LOB app, network) with no workaround.
   - Multiple clients impacted by a shared cause.
   - Active security incident with spreading or destructive behavior (also engage the security runbooks).
   - Contractual/regulatory trigger (a client whose contract defines major-incident terms is in scope of those terms).
   - A lead's judgment call — the checklist supports judgment, it doesn't replace it.
   If none hold: explicitly record "assessed, not declared, because <reason>" on the ticket and continue as P1 (see p1-incident-bridge in escalation). A documented non-declaration prevents the 3am re-litigation.
2. **Declare**: designate one ticket as the master incident record (create_ticket if the cluster has no natural master; link the rest to it). Set priority to the desk's highest (list_ticket_priorities) and mark the declaration in a plain-text note: declared-at time, declared-by, criteria met, known impact so far.
3. **Assign roles** — named humans, confirmed available, recorded on the master ticket:
   - Incident Commander (owns decisions and priorities; see incident-commander-brief for what they get at takeover).
   - Comms lead (owns the update clock — internal and client; see incident-comms-cadence).
   - Technical lead(s) per workstream.
   One person may hold multiple roles on a small desk, but every role has a name. Suggest candidates via search_members / on-call records; a human confirms assignments.
4. **Start the comms cadence**: record the update interval (default: internal every 30 min, client every 60 min, or the client's contractual terms if stricter) and the time of the first due update. Hand execution to incident-comms-cadence; the first client notification pairs with outage-notification (communication).
5. **Open the timeline**: from declaration onward, timestamped notes on the master ticket for every significant event — this feeds the IC brief, the RFO letter, and the post-mortem. Start it now, not retroactively.
6. Output the declaration summary: master ticket, criteria, roles, cadence, next update due.

## Guardrails

- Declaration is a human decision — the agent runs the checklist and recommends; a named person declares. Same for stand-down.
- Never let declaration wait on complete information: declare on impact, refine the cause later. The checklist asks "how bad," not "why."
- A non-declaration is recorded with reasoning, never silent.
- No client-facing statements from this skill — that's the comms lead's lane via incident-comms-cadence, with defensive-writing rules for anything security-related.
- Roles must be named individuals who acknowledged — "the team is on it" is not a role assignment.

## Unattended (Flows) variant

- Trigger: a ticket cluster or monitoring signal matching major-incident criteria (e.g. N tickets, same client, same symptom class, within M minutes).
- The agent NEVER declares unattended. Its entire reply is one plain-text internal note on the candidate master ticket: "MAJOR INCIDENT CANDIDATE: <criteria matched, counts, clients affected>. Declaration checklist ready — needs a human declare/hold decision from <on-call lead role>." Nothing else — no priority changes, no role assignments, no client comms.
- Deterministic stop: if the cluster already has a declaration note or an existing major-incident master ticket, do nothing. When signal confidence is below certain (ambiguous symptom match, possible test/maintenance traffic), do nothing.
