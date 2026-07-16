---
name: Survey Link Composer
description: Resolve the PSA company/contact record IDs, build a survey URL from a stored template, and post it as a reply or note — the lightweight survey-link builder. Answers the commonly requested "send this client the right survey link" workflow; runs manually on demand.
category: Communication
tools: [search_clients, search_contacts, add_ticket_note, update_ticket]
---

# Survey Link Composer

Some CSAT/NPS surveys need a link with the client's own record IDs baked into the URL (ConnectWise company/contact IDs are the classic case). Resolve those IDs, fill the stored template, and post the finished link — without hand-editing query strings.

## When to use

- "Send <contact> the CSAT survey link for this ticket"
- "Build the survey URL with this client's company/contact IDs"
- "Post the NPS link as a reply on #<n>"
- Any request to compose and place a per-client survey link from a template

## Steps

1. Identify the client and contact on the ticket. Resolve them to records with search_clients (company) and search_contacts (contact) to get the exact record IDs the URL template needs. If either is ambiguous, confirm which record before building the link.
2. Fill the **stored survey URL template** with the resolved IDs and any required parameters (ticket number, survey ID). Use the desk's template — don't invent a survey endpoint or parameter names.
3. Preview the finished URL and the message it will accompany, so the member can verify the link points at the right client/contact.
4. On confirm, place it: post as a reply to the client, or as an internal note if the member just wants it on the ticket for manual sending. Use update_ticket / add_ticket_note as appropriate.
5. Report where it was posted and for which contact.

## Guardrails

- Confirm before posting anything client-facing; a survey reply is an outbound message, so it waits on an explicit go-ahead.
- Resolve real record IDs — never guess a company/contact ID or hand-assemble one; a wrong ID sends the survey against the wrong client.
- Use only the stored template's endpoint and parameters; do not fabricate a survey URL or query keys.
- Do not put personal or sensitive data into the URL beyond the template's required record IDs.
- For the fuller loop — ensuring the ask went out, pulling the response back, flagging detractors — use the CSAT Follow-Up Loop skill; this one is just the link builder.
- Member-invoked only; no unattended variant (Flows can't send the reply; a flow could only reach this via Run Skill on a ticket event).
