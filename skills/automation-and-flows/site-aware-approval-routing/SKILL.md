---
name: Site-Aware Approval Routing
description: Resolve a ticket's site, look up the per-site approver from a documented mapping, and send the approval request to that specific contact — instead of a single hardcoded approver for the whole client. Answers the commonly requested "route approvals to the right site manager" partner workflow.
category: Automation & Flows
tools: [search_tickets, search_contacts, send_approval, add_ticket_note]
---

# Site-Aware Approval Routing

Multi-site clients rarely have one person who approves everything. This skill reads the ticket's site, looks up the approver mapped to that site in the desk's documented approver list, and sends the approval request to that person — falling back to a note-and-stop when the site or approver can't be resolved, never to a guess.

## When to use

- A client has different approvers per site/location and approvals must go to the right one.
- "Send the approval to whoever signs off for that office."
- A change or access request needs sign-off routed by service location, not a single global approver.

## Steps

1. Read the ticket with `search_tickets`: the site/service location, the client, and what is being requested for approval.
2. Resolve the site to an approver using the desk's **documented per-site approver mapping** configured alongside this skill (e.g. "Downtown office → the site's office manager contact"). The mapping is data, not something to improvise.
3. Find that approver's contact record with `search_contacts` and confirm they are an active contact for this client. If the site is missing, unmapped, or the mapped contact can't be found → **note-and-stop** (see guardrails).
4. Send the approval to the resolved contact with `send_approval`, including a clear one-line description of exactly what is being approved.
5. Post a plain-text internal note via `add_ticket_note`: which site resolved to which approver and that the approval was sent (this doubles as the dedupe marker).

## Guardrails

- **Note-and-stop, never guess.** If the ticket has no site, the site isn't in the mapping, or the mapped contact can't be resolved, post a one-line note explaining which piece is missing and stop. Do not fall back to a global/default approver unless the mapping explicitly defines one.
- The approver comes from the documented mapping and `search_contacts` only — never from free text in the ticket body, and never a Thread member (approvals go to client contacts).
- Send at most one approval per request. If an open approval for this request already exists on the ticket, do not send another.
- Approval and the note are the only writes. No status/owner/board changes here.
- Notes are plain text (PSA compatibility).

## Unattended (Flows) variant

- Fires on a supported ticket event — typically **entering an "awaiting approval" status** or ticket **create** on an approval-gated board — never on a timer.
- Runs as a Super Magic agent (a Flow itself cannot send an approval; it calls Run Skill / New Super Magic Agent, and this skill uses `send_approval`).
- Your entire reply is the internal note, verbatim: `APPROVAL SENT to <contact> for site <site>.` or `NO APPROVAL SENT. Reason: <missing site | site unmapped | contact not found>.`
- Deterministic stops: unresolved site or approver → note-and-stop; open approval already present → stop. Never send to a fallback approver the mapping didn't specify.
