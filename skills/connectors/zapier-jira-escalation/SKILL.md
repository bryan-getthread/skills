---
name: Zapier Jira Escalation
description: Escalate a ticket into a client's (or vendor's) Jira — JQL dedupe first, create the issue with full evidence, then keep comments flowing both ways so neither side goes dark. Use for "raise this in their Jira", "file it with the client's dev team", or syncing an update across the Thread↔Jira boundary.
category: Connectors
tools: [search_tickets, add_ticket_note, update_ticket, "zapier: Jira Software"]
---

# Zapier Jira Escalation

Some escalations leave the MSP's world: the client's internal dev team, a vendor, a co-managed IT department — all living in Jira. This skill files the issue there properly (JQL dedupe first, evidence attached, both sides cross-referenced) and then keeps the two systems honest with two-way comment relay, so the ticket never says "waiting on client dev" while their Jira says "waiting on MSP".

## When to use

- "This is a bug in <client>'s internal app — raise it with their dev team's Jira."
- "The vendor wants it filed in their Jira project — do it with the full evidence."
- "Their Jira issue got an update — reflect it on our ticket" (or the reverse).

For escalating to the MSP's *own* engineering team, use Engineering Escalation to Linear — same pattern, home tooling.

## Steps

1. Establish the escalation package from the ticket: symptom, exact error text, environment/version, repro steps as documented, business impact, and the workaround in play.
2. **JQL dedupe first:** `zapier: Jira Software "Find Issue"` with JQL scoped to the agreed project — key error text, component, recent window. A matching open issue → add the new evidence as a comment on it instead of filing a twin, and cross-reference that issue.
3. Otherwise create the issue in the agreed project/issue-type (confirm both with the member if not established — never guess a project key): engineering-terms summary, full evidence package, the Thread ticket reference in the description, and only fields the member confirms (assignee/priority conventions belong to the Jira's owners).
4. Show the drafted issue for confirmation before creating.
5. Cross-reference both ways: `add_ticket_note` (plain text) with the Jira issue key and URL; the Jira description carries the ticket reference. Each side must be findable from the other — this is what makes the sync in step 6 possible.
6. **Two-way comment sync (on request or per pass, not a live daemon):**
   - Outbound: a material ticket update the other side needs → relay as a Jira comment, client-safe, prefixed with the ticket reference.
   - Inbound: check the Jira issue's comments/status since the last sync note; material updates land on the ticket via `add_ticket_note` with "per Jira <key>" provenance.
   - Relay facts and decisions, not transcripts — each system keeps its own chatter.
7. When the Jira issue resolves, verify against the ticket's reality before closing anything: their "Done" and the client's "working" are separate facts; note the first, confirm the second, then update the ticket.

## Guardrails

- **Member-connected Zapier required** (the target Jira instance — often the *client's*, so access is a standing arrangement, not an improvisation). Degrade: output the ready-to-file issue text and the comment relay text for manual posting.
- Dedupe before create, every time; a +1 comment beats a duplicate issue in someone else's backlog.
- The Jira instance belongs to someone else: create and comment only. Never transition their workflows, reassign their people, or edit their fields unless the member explicitly directs a named action.
- Everything written to their Jira is external-facing: client-safe, no MSP-internal commentary, no credentials, no end-user personal data beyond what repro requires.
- Sync notes carry provenance and timestamps ("per Jira <key>, <date>"); never mark the ticket resolved solely because Jira says fixed.
- Task cost: each Zapier call = 2 tasks — one JQL find + one create per escalation; batch sync passes rather than per-comment polling.
