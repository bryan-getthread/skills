---
name: Zapier Gmail Client Email
description: For Google-shop clients and desks — send or draft email from shared Gmail inboxes and pull a requester's recent emails into ticket context. Use for "email the client from support@", "draft it in Gmail for review", or "what has <user> emailed us recently" when the mailbox is Google, not Outlook.
category: Connectors
tools: [search_tickets, search_contacts, add_ticket_note, "zapier: Gmail"]
---

# Zapier Gmail Client Email

The Gmail twin of Zapier Outlook Client Email: work the desk's real Google mailboxes — send or draft from shared addresses (support@, billing@) via `zapier: Gmail "Send Email" / "Create Draft"`, and use "Find Email" to bring a requester's recent correspondence into the ticket picture.

## When to use

- "Email <user> from the support mailbox that their password reset is done" — and the desk runs on Google Workspace.
- "Draft the renewal email from billing@ — I'll review before it goes."
- "Search the shared inbox for what <user> sent us this week — the ticket's missing context."

## Steps

**Sending / drafting:**
1. Resolve the recipient from the contact record (`search_contacts`) — never from an address typed loose in a note or guessed from a name pattern.
2. Draft in the desk's client-email register: plain professional language, correct salutation, ticket reference, clear next step. Localize when the desk serves non-English clients.
3. Choose Draft vs Send: anything sensitive, billing-related, legal-adjacent, or first-of-its-kind goes to `zapier: Gmail "Create Draft"` in the shared mailbox for human review. Routine confirmations may Send after the member confirms recipient + body in chat.
4. Verify the From: the send must come from the shared address (alias/delegated mailbox), not the member's personal address — confirm the sending identity on first use and state it in the confirmation.
5. Show recipient, sending mailbox, subject, and body for confirmation, then execute. `add_ticket_note` (plain text): what was sent/drafted, from which mailbox, to whom.

**Pulling context:**
6. `zapier: Gmail "Find Email"` scoped to the requester's address and a stated window (default 14 days; Gmail search operators like `from:` and `newer_than:` keep it tight). Summarize what's relevant to the ticket — dates, asks, attachments mentioned — and offer to note the summary onto the ticket. Quote emails only as needed; don't dump full bodies into the ticket.

## Guardrails

- **Member-connected Zapier required**, and the connected Google account must have delegated/alias rights to the shared mailbox — a send that silently comes from the member's personal Gmail is a real failure mode. If Zapier is absent, degrade to drafting the email text in chat. Outlook-shop tenant → this skill's twin, Zapier Outlook Client Email.
- Confirm-before-send, always: recipient, mailbox, body. When in doubt, draft — a draft is free to fix; a sent email is not.
- Never invent email history: context summaries state only what the found emails actually say, and disclose when the search window or result cap limits the picture.
- Mailbox search results are personal data: summarize what the ticket needs, nothing more.
- No credentials or secure links in email bodies; use the desk's secure delivery method and reference it.
- Task cost: each Zapier call = 2 tasks — batch context pulls into one Find with a sensible window rather than per-day calls.
