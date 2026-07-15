---
name: Praise Relay
description: A client praised a technician — draft the internal shout-out for the team and the thank-you reply to the client, so good feedback travels both directions.
category: Communication
tools: [search_tickets, view_openDraft, add_ticket_note]
---

# Praise Relay

The good-news counterpart to complaint handling: makes sure a compliment reaches the technician's team and the client hears that their kind words landed.

## When to use

- "The client just said <tech> was fantastic — write it up for the team."
- A CSAT comment, email, or review contains genuine praise for a technician or the desk.
- "Draft a thank-you back to the client for the nice feedback."

## Steps

1. Read the praise in context (the ticket thread or survey comment). Capture what was actually praised — speed, patience, clarity, going the extra mile — and the quotable line if there is one.
2. Draft the **internal shout-out**:
   - Short and specific: who, which client situation (described, not confidential detail), what the client said — quote their words where possible, quotes beat paraphrase for morale.
   - Positive-only: no coaching notes, no "great job, but..." — this artifact has one purpose.
   - Route per the desk's habit: a plain-text ticket note via add_ticket_note tagged for the service manager, and/or a team-channel message (e.g. `zapier: Microsoft Teams "Send Channel Message"`) if that connector is available to the member — post only with confirmation.
3. Draft the **client thank-you** per the Email Baseline Standard (communication/email-baseline-standard):
   - Two or three sentences: thank them for taking the time, note that the feedback was shared with <tech first name> and the team, and that it genuinely matters.
   - No marketing ask bolted on (no review-site solicitation) unless the member explicitly requests it.
4. Present both artifacts together — shout-out labeled INTERNAL, thank-you as the client draft via view_openDraft.

## Guardrails

- Quote the client accurately or paraphrase honestly — never embellish the praise ("said you were fantastic" must actually appear in the record in substance).
- The internal shout-out names the praised tech only; never turn it into an implicit comparison with other techs.
- Client confidentiality holds even in celebration: the shout-out describes the situation generically if it involves sensitive client details.
- Nothing posts or sends without confirmation — including the Teams message; note that the Teams/Zapier route requires the member to have connected that integration.
- Internal notes in plain text (PSA compatibility).
- Degradation: view_openDraft is in-app only — over external MCP, output both artifacts in chat, labeled INTERNAL SHOUT-OUT and CLIENT DRAFT.
