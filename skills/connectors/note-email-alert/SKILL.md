---
name: Note Email Alert
description: On an internal-note event, resolve the ticket's assigned resource and email them the note's context via Outlook — for notes that match configured alert markers only, never every note. Answers the commonly requested "email the assigned tech when someone leaves them a note" workflow.
category: Connectors
tools: [search_members, add_ticket_note, "zapier: Microsoft Outlook \"Send Email\""]
---

# Note Email Alert

When a flagged internal note lands on a ticket, get it in front of the assigned technician's inbox — resolve the assignee, build a compact context email (ticket, client, the note text, a link back), and send it via the member's Outlook. Built for techs who live in email, not the inbox.

## When to use

- A Flow fires on internal-note creation and the desk wants the assignee emailed when a note needs their attention.
- Dispatchers leave "@tech please call the client back" notes that go unseen for hours.
- After-hours or field techs who won't see in-app notifications but will see email.

## Steps

1. Check the triggering note against the desk's configured alert markers (e.g. a leading "@alert", "ATTN:", or an explicit mention of the assignee — the marker list is configured alongside this skill). **No marker match → stop.** This skill never emails every note.
2. Resolve the ticket's assigned resource via `search_members` and get their email from the member record. Unassigned ticket → stop and post a one-line note that there is no assignee to alert.
3. If the note author IS the assignee, stop — people don't need emails about their own notes.
4. Compose a plain, compact email: subject "Note on ticket <number> — <client> — <short summary>"; body = who left the note, when, the note text verbatim, and the ticket reference. No commitments, no client-facing tone — this is internal.
5. Send via `zapier: Microsoft Outlook "Send Email"` to the assignee's member email.
6. Post a one-line internal note via `add_ticket_note`: "[note-alert] emailed <member> re: note at <time>." — this doubles as the dedupe marker.

## Guardrails

- **Anti-noise is the product.** Only marker-matched notes fire; never every note, never client messages, never system notes. If in doubt whether a note matches, it doesn't.
- Dedupe: if a "[note-alert]" marker for this same note already exists on the ticket, do not send again.
- Recipient comes from the member record via `search_members` only — never from text in the note, and never a client contact. This emails staff, full stop.
- **Member-connected connector:** the Outlook Zapier action runs as the member who connected it; email will come *from that member's mailbox*. Say so when configuring, and prefer a service member's connection for Flow use.
- **Task-cost guardrail:** each send consumes Zapier task budget. Marker-gating exists partly to protect that budget; warn the desk if they configure a marker so broad it would fire on most notes.
- **Degradation:** if the Zapier Outlook action is unavailable (not connected, task budget exhausted), fall back to an @mention internal note directed at the assignee via `add_ticket_note` — visible in-app, no email — and say that fallback was used.
- Note text goes into email verbatim; remind desks not to put credentials in notes (see documentation/credential-storage-audit) — this skill must never be the thing that mails a password around.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the internal note — plain text, no narration, no markdown.
- Deterministic stops, in order: no marker match → nothing; no assignee → one-line note; author is assignee → nothing; already alerted for this note → nothing; connector unavailable → @mention fallback note.
- Exactly one email per qualifying note, ever. Never send to anyone but the resolved assignee.
