---
name: Assignment Email Notifier
description: On ticket assignment, resolve the assignee and email them their new ticket via Outlook — so techs who live in email know they've been given work without watching the inbox. Sibling of Note Email Alert. Answers the commonly requested "email the tech when a ticket is assigned to them" partner workflow.
category: Connectors
tools: [search_members, add_ticket_note, "zapier: Microsoft Outlook \"Send Email\""]
---

# Assignment Email Notifier

Assign a ticket and the tech may not notice for hours if they don't live in the Thread inbox. This skill fires on assignment, resolves who the ticket went to, and emails them a compact "you've got a ticket" via their Outlook — with the same member-connect, task-cost, and degradation discipline as its sibling, Note Email Alert.

## When to use

- A Flow assigns tickets and after-hours or field techs won't see in-app notifications.
- "Email the tech when something gets assigned to them."
- Making assignment visible to a team that works out of email, not the queue.

## Steps

1. On assignment, resolve the newly assigned resource via `search_members` and get their email from the member record. If the ticket is unassigned or the assignee can't be resolved, stop and post a one-line note.
2. If the assignment was made **by the assignee themselves** (a tech grabbing their own ticket), stop — nobody needs an email about picking up their own work.
3. Compose a plain, compact email: subject "Assigned to you: ticket <number> — <client> — <short summary>"; body = the request in brief, priority/board, and the ticket reference. Internal tone, no client-facing language.
4. Send via `zapier: Microsoft Outlook "Send Email"` to the assignee's member email.
5. Post a one-line internal note via `add_ticket_note`: "[assign-alert] emailed <member> at <time>." — this doubles as the dedupe marker.

## Guardrails

- Recipient comes from the member record via `search_members` only — never from ticket text, never a client contact. This emails staff, full stop.
- Dedupe: if an "[assign-alert]" marker for this same assignment already exists, do not send again. Reassignment to a different member is a new, separate alert.
- Self-assignment does not fire.
- **Member-connected connector:** the Outlook Zapier action runs as the member who connected it — email comes *from that member's mailbox*. Prefer a service member's connection for Flow use; open with the verify-first gate (`skills/connectors/zapier-action-discovery`).
- **Task-cost guardrail:** each send spends Zapier budget; do not wire this to fire on churny reassignment loops.
- **Degradation:** if the Outlook action is unavailable (not connected, budget exhausted), fall back to an `@mention` internal note to the assignee via `add_ticket_note` — visible in-app, no email — and say the fallback was used.
- Notes plain text (PSA compatibility).

## Cross-references

- Note-triggered sibling: `skills/connectors/note-email-alert`
- Teams instead of email: `skills/connectors/zapier-teams-ticket-notifications`

## Unattended (Flows) variant

- Fires on the **assignment event** (member assigned) — a supported ticket condition.
- Runs as a Super Magic agent (a Flow cannot natively send email; it calls Run Skill / New Super Magic Agent).
- Your entire reply is the internal note, verbatim: `[assign-alert] emailed <member> at <time>.` or `[assign-alert] fallback @mention (Outlook unavailable).` or `NO ACTION. Reason: <unassigned | self-assignment | already alerted>.`
- Exactly one email per assignment, ever. Never send to anyone but the resolved assignee.
