---
name: Expectation-Setting Acknowledgment
description: Draft the first-touch acknowledgment on a new ticket — what we understood, how seriously we're treating it, what happens next, and when the client will hear from us.
category: Communication
tools: [search_tickets, view_openDraft]
---

# Expectation-Setting Acknowledgment

The first human message on a ticket, done right: proves the request was actually read, sets the working expectation, and buys the desk room to work without the client chasing.

## When to use

- "Acknowledge this new ticket for the client."
- A fresh ticket needs a first response that's more than an auto-reply.
- Intake process calls for a personalized acknowledgment before triage work starts.

## Steps

1. Read the client's full request. The acknowledgment must demonstrate comprehension — a generic "we received your request" is what the auto-responder already sent.
2. Draft per the Email Baseline Standard skill (communication/email-baseline-standard), with four beats:
   - **What we understood:** a one-sentence restatement of their issue in their terms ("you're unable to open shared files since this morning, and it's affecting your whole team"). This is the beat that builds trust — get it right.
   - **How we're treating it:** the working severity in client terms ("we're treating this as a priority") — reflecting the ticket's actual priority, never overstating urgency to please.
   - **What happens next:** the immediate next step and who's on it ("a technician is picking this up to <first action>").
   - **When you'll hear from us:** a specific update commitment ("you'll have an update from us by <time>") that fits the desk's real response norms — this is the line that stops the "any update?" email.
3. If anything in the request is ambiguous and blocks work, fold the clarifying question into the acknowledgment — one email, not two.
4. Keep it to one short paragraph plus the update-time line.
5. Present via view_openDraft.

## Guardrails

- The restatement must come from their actual message; if the request is too vague to restate, the acknowledgment leads with the clarifying question instead of a guessed summary.
- Never promise a resolution time at first touch — commit to an **update** time only. "Fixed by Friday" at intake is a guess wearing a promise's clothes.
- Severity language must match the ticket's real priority; if the client's expectation and the assigned priority clash, flag it to the tech rather than papering over it in the email.
- The update-time commitment must be one the desk will keep — align with SLA/board norms, and when unsure, ask the tech before committing.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft in chat labeled "DRAFT."

## Unattended (Flows) variant

- Your entire reply is the acknowledgment email body — no narration.
- Derive the update-time line from the board's configured expectation only; if none is available to the run, end with "we'll follow up shortly" rather than inventing a deadline. If the request text is empty or auto-generated noise, output nothing.
