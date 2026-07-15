---
name: Scope Pushback
description: Draft the reply when a client asks for something outside their agreement — a helpful no that shows the path to yes (quote or agreement change) and loops in the account manager.
category: Communication
tools: [search_tickets, search_members, view_openDraft, add_ticket_note]
---

# Scope Pushback

Drafts the out-of-scope response that protects the agreement without souring the relationship: we can't do this under your current plan, here's exactly how we can.

## When to use

- "Client is asking for <project work / new hardware / unsupported system> — it's out of scope. Draft the reply."
- A ticket contains a request the tech believes falls outside the client's agreement.
- "How do I say no to this nicely?" on a scope question.

## Steps

1. Read the ticket and confirm with the technician that the request is genuinely out of scope. The agent does not adjudicate contracts — scope determination is the tech's or AM's call; the skill drafts once that call is made. If the tech is unsure, the first output is a recommendation to check with the account manager, not a client draft.
2. Draft per the Email Baseline Standard skill (communication/email-baseline-standard):
   - Acknowledge the request positively — it's usually a good idea, say so when true.
   - The no, framed as a boundary of the current agreement, not a refusal to help: "this falls outside what's covered under your current agreement."
   - **The path to yes:** the concrete route — "we'd handle this as a quoted project" or "this could be added to your agreement" — and the immediate next step: "<account manager> will reach out to walk through options."
   - Never leave the client at a dead end; the email ends with motion, not a wall.
3. Identify the account manager for this client (search_members / client record) and prepare the internal handoff: offer to add a plain-text internal note via add_ticket_note summarizing the request, the pushback sent, and that the AM owes the client an outreach.
4. Present the client draft via view_openDraft. Sending, and the AM handoff, are human-confirmed actions.

## Guardrails

- Never assert specific contract terms, coverage details, or prices in the client email — the agent has not read the agreement. "Outside your current agreement" per the tech's determination is the ceiling; quoting terms belongs to the AM.
- Never say yes-with-conditions that commits the desk to unquoted work ("we can probably squeeze this in").
- Never blame the client for asking or make scope sound like a punishment.
- The AM loop-in is mandatory whenever the path to yes is offered — a path with nobody walking the client down it is worse than none. If no AM is identifiable, flag that to the tech explicitly.
- Internal notes in plain text (PSA compatibility).
- Degradation: view_openDraft is in-app only — over external MCP, output the draft and the AM handoff note in chat, separately labeled.
