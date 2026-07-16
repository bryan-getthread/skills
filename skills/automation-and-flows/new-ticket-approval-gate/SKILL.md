---
name: New Ticket Approval Gate
description: For configured clients, every new ticket requires the client's designated approver to authorize work before it begins — fire send_approval on intake, hold the ticket in a waiting status, and record the outcome; timeout means not approved.
category: Automation & Flows
tools: [search_tickets, search_clients, search_contacts, send_approval, update_ticket, add_ticket_note]
---

# New Ticket Approval Gate

Some clients contractually require sign-off before any billable work starts — a designated manager approves every request, or a co-managed IT lead vets what the MSP touches. This skill is the flow-embedded gate: on new ticket for a configured client, request approval from the designated approver, park the ticket until the answer arrives, and make the outcome part of the record.

## When to use

- A flow fires on ticket creation for clients configured as approval-gated.
- "For <client>, no work starts until their office manager approves the request."
- Co-managed desks where the client's internal IT authorizes which tickets the MSP works.

## Steps

1. Confirm the ticket's client is on the configured approval-gated list (`search_clients` to resolve the client if needed). Not on the list → do nothing; this gate applies only where configured.
2. Check the gate has not already run: if the ticket already carries an approval request, outcome note, or gate marker from this skill (`search_tickets` on the ticket's notes), do nothing — one gate per ticket.
3. Exclusions before gating: if the ticket is itself an emergency per the client's configured carve-outs (e.g. security incidents, outages — whatever the client agreed), skip the gate, route normally, and note that the carve-out applied. No carve-out configured → everything gates.
4. Resolve the designated approver from the client's configuration (`search_contacts` to confirm the contact exists and is active). Approver missing or inactive → do NOT guess a substitute; flag the ticket for a human with a plain-text note and stop.
5. Fire `send_approval` to the designated approver: what was requested (ticket title + one-line summary), who requested it, and the response window per the client's configuration. Structure the request per the approval-request-email skill's what/why/decision framing (that skill covers attended drafting; this gate is its unattended sibling — Teams-based desks may route the same gate through zapier-teams-approval-gate instead).
6. Park the ticket: `update_ticket` to the desk's waiting-on-approval (or waiting-on-client) status and post a plain-text `add_ticket_note`: gate fired, approver, sent time, timeout deadline.
7. Record the outcome when it arrives:
   - Approved → move the ticket to the normal intake/triage status, note "Approved by <approver role> at <time>", and let standard routing take over.
   - Declined → note the decline verbatim reason if given, and route per desk convention for declined work (typically close-with-note or return to the client contact). Never silently delete the request.
   - Timeout (no response by the configured deadline) → NOT approved. Note "Approval timed out at <time>; work not authorized," and route per the client's configured timeout handling (default: remain waiting, escalate to the account owner — never treat silence as consent).

## Guardrails

- Timeout = not approved. Silence never authorizes work. This is the gate's defining rule.
- No work-adjacent writes while parked: the gate must not assign a tech to begin work, log time, or send troubleshooting replies before approval lands. Acknowledgment-only communication is allowed if the desk's intake flow sends one.
- One approval request per ticket — never spam the approver with re-sends unless the desk's configuration defines a single reminder.
- Never substitute an approver. The designated approver is client configuration; if they are unreachable or departed, that is a human/account-manager problem, not a routing improvisation.
- The approval outcome (approved/declined/timeout, who, when) must be on the ticket permanently — it is the billing and scope defense.
- Notes are plain text; on PSA-synced desks keep the gate notes internal (see psa-note-visibility-rules).
- Degradation: if send_approval is unavailable for the tenant, do not fake the gate with an ordinary email and an assumption — flag the ticket for manual gating and stop.

## Unattended (Flows) variant

- Your entire reply is the internal note, verbatim, plain text: `APPROVAL GATE: request sent to <approver role>, deadline <time>. Ticket parked.`, `GATE SKIPPED: client not configured.`, `GATE SKIPPED: emergency carve-out (<class>).`, `APPROVED by <approver role> <time> — released to intake.`, `DECLINED by <approver role> <time> — routed per convention.`, or `TIMEOUT <time> — not approved; escalated.`
- Deterministic stops: client not configured → no action; gate already ran → no action; approver unresolvable → flag and stop; send_approval unavailable → flag and stop.
- Never message the end user about the gate's internals; the approver interaction happens only through send_approval.
