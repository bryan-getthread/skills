---
name: Post-Incident Action Tracking
description: Turn post-incident review action items into real tickets with owners and due dates, then run the follow-through audit that catches the ones quietly dying — the discipline that makes postmortems more than a feelings meeting.
category: Change & Problem Management
tools: [search_tickets, create_ticket, update_ticket, add_ticket_note, list_boards, search_members]
---

# Post-Incident Action Tracking

Every post-mortem produces action items; almost every desk loses half of them within a month. This skill does the two things that prevent that: converts each accepted action into a tracked ticket at PIR time, and audits the set on a cadence until every item is done, deliberately dropped, or escalated — never just forgotten.

## When to use

- A post-mortem/PIR just concluded ("track the action items from the <incident> post-mortem").
- The periodic follow-through audit: "where are we on post-incident actions?" across one incident or all recent ones.
- An RFO letter promised prevention measures (rfo-letter) and those promises need tracking behind them.
- Before closing a major incident's master ticket: are the actions independently tracked so closure doesn't orphan them?

## Steps

**Conversion (at PIR time):**

1. Pull the action items from the post-mortem document or ticket (post-mortem-rca-author output; security incidents via security-incident-postmortem). Only convert items a human accepted at the review — the agent doesn't promote every "we could..." musing into committed work.
2. For each accepted item, require the three fields before it becomes a ticket: a **specific verifiable action** ("add disk-space alerting on the database volume at 80%," not "improve monitoring"), an **owner** (a named person — "the infrastructure team" is a place actions go to die; ask the requester to name one, search_members to disambiguate), and a **due date** proportionate to the risk it mitigates. Items missing any field go back to the requester as "not trackable yet: needs <field>" — recorded as such, not silently dropped and not created hollow.
3. Create one ticket per action (create_ticket, on the board the desk uses for internal work — confirm via list_boards): title "PIR ACTION: <verb phrase> [<incident ref>]", body carrying the action, the incident and post-mortem references, why it matters (which failure it prevents recurring), owner, due date.
4. Cross-link: a plain-text note on the incident master ticket listing every action ticket created — the closure audit trail. If the RFO letter promised specific prevention measures, verify each promise has a corresponding action ticket and flag any that don't: a written client promise with no ticket behind it is a liability.

**Follow-through audit (on a cadence, default every 2 weeks until the set closes):**

5. search_tickets for open PIR-action tickets; for each, classify from the evidence: DONE (closed with completion evidence — apply the change-completion-verification standard where the action was a change; a bare "done" is unverified), ON TRACK, AT RISK (due within a week, no activity), OVERDUE, or STALLED (no activity in 30+ days).
6. For overdue/stalled items, post a plain-text nudge note on the ticket addressed to the owner, and roll repeat offenders up to the lead: the audit's product is the uncomfortable list — "incident <ref> was 6 weeks ago; 3 of 5 prevention actions have had zero activity."
7. If a stalled action is being deliberately dropped, require the drop to be explicit: a note by the owner or lead stating it's descoped and why — deliberate drops are legitimate; silent deaths are not. Close the ticket referencing that decision.
8. Report the audit: per incident, actions done / on track / at risk / overdue / dropped-with-decision, and the follow-through rate over time. A desk that sees its rate improves it.

## Guardrails

- The agent creates tickets, nudges, and reports — it never marks an action done without evidence, never picks owners itself, and never descopes an item on its own judgment.
- One action, one ticket, one owner. Compound items get split; shared ownership becomes one accountable owner plus helpers, or it isn't tracked as owned.
- Verification standard is real: prevention actions that were changes get the same evidence bar as any change completion.
- Actions that were client promises (RFO) can only be dropped with the lead's explicit sign-off, because dropping them breaks a written commitment.
- Result-cap honesty in audits: if the action-ticket search may be capped, say the audit is partial.
