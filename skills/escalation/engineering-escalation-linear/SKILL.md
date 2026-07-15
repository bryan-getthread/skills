---
name: Engineering Escalation to Linear
description: Escalate a ticket (or a recurring ticket pattern) to the engineering team as a Linear issue, with aggregated evidence and affected-client count, cross-referenced both ways.
category: Escalation
tools: [search_tickets, add_ticket_note, list_issues, get_issue, create_issue, create_comment, list_teams, list_issue_labels]
---

# Engineering Escalation to Linear

Bridges the service desk and the engineering backlog: turns a ticket — or a cluster of tickets pointing at the same product defect — into one well-evidenced Linear issue, and stamps the reference on both sides so the loop can be closed when the fix ships.

## When to use

- "This is a product bug — escalate it to engineering" / "create a Linear issue for this ticket."
- The desk keeps seeing the same defect across clients and wants it raised once, with the full blast radius attached.
- A recurring-issue review concluded the real fix is a code change.

## Steps

1. Confirm the Linear connector is available (it is member-authenticated — the current member must have connected Linear; if the tools are absent, stop and say so).
2. Establish the defect from the source ticket: symptom, exact error text, affected product/component, version, and steps to reproduce as documented.
3. Measure the blast radius: search_tickets for other open and recent tickets with the same signature (split searches per signal; disclose caps). Count **distinct affected clients** and total tickets — this is the demand evidence engineering prioritizes on.
4. Dedupe before creating: list_issues / get_issue for an existing Linear issue matching this defect. If one exists, add the new evidence and client count as a create_comment on it instead of filing a duplicate, and use that issue for the cross-references below.
5. Otherwise create_issue on the agreed team (confirm team via list_teams if ambiguous) containing: one-line summary in engineering terms; environment and version; repro steps; verbatim error text; **affected-client count and ticket count with ticket references**; business impact; and the workaround currently in use, if any. Apply an agreed label (e.g. "from-service-desk") when one exists.
6. Cross-reference both ways: post a plain-text internal note on each linked ticket with the Linear issue identifier ("Escalated to engineering as <issue-id>"), and make sure the Linear issue lists every ticket reference. Both sides must be findable from the other.
7. Show the drafted issue for confirmation before creating it. Output ends with: the Linear issue ID/URL, the client and ticket counts, and which tickets were annotated.

## Guardrails

- Requires member-connected Linear; degrade gracefully by outputting the ready-to-file issue text if the connector is absent.
- Search before create — a +1 comment on an existing issue beats a duplicate. Never merge distinct defects into one issue just because symptoms rhyme.
- Evidence only from documented tickets: never inflate the affected-client count, and never invent repro steps engineering will burn time on.
- Strip client-identifying details to what engineering needs; no credentials or end-user personal data in the Linear issue.
- Confirm before creating the issue; ticket notes are plain text.
