---
name: Zapier Outlook Client Email
description: Send or draft email from shared mailboxes via Outlook, and pull a requester's recent emails into ticket context. Use for "email the client from the support mailbox", "draft a reply in Outlook", or "what has <user> emailed us recently".
category: Connectors
tools: [search_tickets, search_contacts, add_ticket_note, "zapier: Microsoft Outlook"]
---

# Zapier Outlook Client Email

Work the desk's real mailboxes: send or draft messages from shared addresses (support@, billing@) with `zapier: Microsoft Outlook "Send Email" / "Create Draft"`, and use "Find Emails" to bring a requester's recent correspondence into the ticket picture.

## When to use

- "Email <user> from the support mailbox that their account is unlocked."
- "Draft the renewal email from billing@ — I'll review before it goes."
- "Pull up what <user> emailed us this week — I think there's context missing from the ticket."

## Steps

**Sending / drafting:**
1. Resolve the recipient from the contact record (`search_contacts`) — never from an address typed loose in a note or guessed from a name pattern.
2. Draft in the desk's client-email register: plain professional language, correct salutation, ticket reference, clear next step. Localize when the desk serves non-English clients.
3. Choose Draft vs Send: anything sensitive, billing-related, legal-adjacent, or first-of-its-kind goes to `zapier: Microsoft Outlook "Create Draft Email"` in the shared mailbox for human review. Routine confirmations may Send after the member confirms recipient + body in chat.
4. Show recipient, sending mailbox, subject, and body for confirmation, then execute.
5. `add_ticket_note` (plain text): what was sent/drafted, from which mailbox, to whom.

**Pulling context:**
6. `zapier: Microsoft Outlook "Find Emails"` scoped to the requester's address and a stated window (default 14 days). Summarize what's relevant to the ticket — dates, asks, attachments mentioned — and offer to note the summary onto the ticket. Quote emails only as needed; don't dump full bodies into the ticket.

## Guardrails

- **Member-connected Zapier required**, and the connected Microsoft account must have rights to the shared mailbox — a send that silently comes from the member's personal address is a real failure mode; verify the From on first use. If Zapier is absent, degrade to drafting the email text in chat.
- Confirm-before-send, always: recipient, mailbox, body. When in doubt, draft — a draft is free to fix; a sent email is not.
- Never invent email content history: context summaries state only what the found emails actually say, and say when the search window or result cap limits the picture.
- Mailbox search results are personal data: summarize what the ticket needs, nothing more.
- No credentials or secure links in email bodies; use the desk's secure delivery method and reference it.
- Task cost: each Zapier call = 2 tasks — batch context pulls into one Find with a sensible window rather than per-day calls.
