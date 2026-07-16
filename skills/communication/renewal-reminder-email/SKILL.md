---
name: Renewal Reminder Email
description: Draft a value-first client nudge that an agreement, contract, or warranty is approaching renewal — account-manager gated, never a hard sell. Use when a renewal date is near and the client needs a heads-up.
category: Communication
tools: [search_tickets, search_contacts, view_openDraft]
---

# Renewal Reminder Email

Draft a warm, value-first reminder that a client's agreement, contract, or hardware warranty is coming up for renewal — framed around continuity of service, not a sales push. This skill produces a draft for a human to review; renewals are an account-owned motion, so it never sends and never commits pricing.

## When to use

- "The <client> agreement renews next month — draft them a heads-up."
- "Warranty on their servers expires in Q3, remind them."
- A ticket or record shows a renewal/expiry date approaching and the client should be told.
- You want an on-brand nudge that leads with the value they're already getting.

## Steps

1. Confirm the renewal facts from the record before writing: what is renewing, the exact date, and what stays in place if they renew. Do not draft from memory or assumption — if the date or scope isn't in the ticket/record, flag it as `NEEDS:` at the top of the output.
2. Lead with value: one or two concrete things the client has gotten from the service in this term (tickets resolved, uptime maintained, projects delivered) — only if the record supports them. If nothing is verifiable, keep it forward-looking instead of inventing wins.
3. State the renewal plainly: what renews, the date, and that continuity is the default. Do not quote a price or new terms — route pricing to the account manager.
4. Give one clear next step: "Your account manager, <AM name>, will reach out to confirm details" or an invitation to reply with questions.
5. Structure and tone per the Email Baseline Standard (communication/email-baseline-standard).
6. Present the draft with view_openDraft for review. Do not send.

## Guardrails

- **AM-gated.** Renewals belong to the account owner. This skill drafts a reminder; it does not negotiate, quote, or confirm terms. If no account manager is identifiable, flag that rather than promising follow-up from "someone."
- **No pricing or commitments.** Never state a renewal price, discount, or new term length unless it is explicitly recorded and cleared for the client. When in doubt, leave it out and route to the AM.
- **Value must be true.** Only cite outcomes supported by the ticket/record. No invented metrics or achievements.
- Keep it a nudge, not a pressure tactic — no artificial urgency or scarcity language.
- Degradation: view_openDraft is in-app only. Over external MCP, output the draft in chat labeled "DRAFT — review before sending."
- See account-management/renewal-prep (internal prep) and client-lifecycle/contract-renewal-routing (routing) — this skill is the client-facing message only.
