---
name: P1 Incident Bridge
description: When a P1 lands, spin up the response bridge in one pass — urgent Linear issue, on-call page via PagerDuty, and key ticket updates streamed to the issue as comments.
category: Escalation
tools: [search_tickets, update_ticket, add_ticket_note, create_issue, create_comment, list_teams, 'zapier: PagerDuty "Add Trigger Event"', 'zapier: PagerDuty "Find User on Call"']
---

# P1 Incident Bridge

A P1 needs three things in the first minutes: engineering has an issue to rally on, the on-call human is paged, and everyone watching the issue sees what the desk learns as it happens. This skill stands all three up and keeps the middle one honest.

## When to use

- "This is a P1 — page on-call and open an incident" / "stand up the bridge for this ticket."
- A ticket is confirmed as a P1/major incident (client-wide outage, data loss, security incident) and needs coordinated response beyond the desk.
- During a running P1: "post the latest from the ticket to the incident issue."

## Steps

1. Confirm P1 status with the member (or trust the already-set priority on the ticket). This skill coordinates response — it does not decide severity.
2. Create the incident issue: create_issue on the agreed engineering/incident team (list_teams if ambiguous) at **urgent priority**, containing: one-line impact statement, affected <client>(s) and scope, start time as documented, symptoms and error text verbatim, what the desk has confirmed vs suspects (labeled as such), and the source ticket reference.
3. Page on-call: `zapier: PagerDuty "Find User on Call"` to identify the current on-call for the agreed schedule, then `zapier: PagerDuty "Add Trigger Event"` with a summary that fits a phone lock screen: client scope, symptom, ticket + Linear references. One page per incident — do not re-trigger on every update.
4. Cross-stamp: plain-text note on the ticket with the Linear issue ID and "on-call paged at <time>"; the Linear issue carries the ticket reference. Update the ticket's priority/status via update_ticket if not already reflecting P1.
5. Stream updates: while the incident runs, on request (or each significant ticket development — scope change, cause identified, mitigation attempted, client comms sent), post a create_comment on the Linear issue: timestamped, one paragraph, facts only. The Linear issue becomes the incident timeline.
6. On resolution: final comment with resolution summary and end time; note on the ticket pointing at the issue for the post-incident review. Recommend a PIR; do not write one unasked.

## Guardrails

- Page once. Duplicate PagerDuty triggers for the same incident train responders to ignore pages — subsequent developments go to the Linear issue, not the pager.
- Separate confirmed from suspected in every update; never post speculation as fact on an issue the whole company reads. Use defensive language for security-flavored P1s ("suspicious activity", not "breach") until confirmed.
- If PagerDuty actions are unavailable (Zapier not configured for this member/tenant), say so immediately and give the member the issue link plus the documented on-call contact from the KB — never silently skip the page.
- Linear connector is member-authenticated; if absent, output the incident summary text and the page must be triggered manually. Never let tool absence delay a P1 silently.
- Ticket notes plain text; timestamps on every streamed comment.
