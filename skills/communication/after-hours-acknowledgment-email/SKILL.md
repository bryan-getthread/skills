---
name: After-Hours Acknowledgment Email
description: Draft the acknowledgment a client gets when they submit a ticket outside business hours — confirming receipt, setting a realistic response expectation, and giving the emergency path. Use for after-hours intake auto-acks.
category: Communication
tools: [search_tickets, view_openDraft]
---

# After-Hours Acknowledgment Email

Draft the reply a client receives when they contact support outside business hours: it confirms the ticket landed, sets an honest expectation for when they'll hear back, and points genuine emergencies to the right path. It prevents the "did anyone get this?" anxiety without over-promising.

## When to use

- "Someone logged a ticket at 11pm — draft the after-hours ack."
- A ticket arrives outside the client's coverage window and needs a receipt-and-expectations reply.
- Setting up the standard after-hours acknowledgment wording.

## Steps

1. Confirm the client's actual coverage terms before writing: business hours, whether they have after-hours/emergency coverage, and how emergencies are reached. Only state what's true for this client — coverage varies by agreement. If unknown, flag as `NEEDS:` rather than guessing.
2. Draft a short reply:
   - **Receipt** — confirm the request was received and a ticket is open.
   - **Expectation** — when they can realistically expect a response (next business day / within the SLA that applies), stated honestly.
   - **Emergency path** — how to escalate a true emergency *if* the client has that coverage. If they don't, say support resumes at <time> — do not invent an emergency line.
3. Keep it brief and reassuring; no filler.
4. Structure and tone per the Email Baseline Standard (communication/email-baseline-standard).
5. Present the draft with view_openDraft for review, unless running unattended (see below).

## Guardrails

- **No over-promising.** Never imply live after-hours support or a response time the agreement doesn't cover. An honest "next business day" beats a false "shortly."
- **Emergency path must exist.** Only cite an emergency/on-call route if this client actually has one.
- Match the client's coverage terms exactly; do not generalize one client's SLA to another.
- Degradation: view_openDraft is in-app only. Over external MCP, output the draft in chat labeled "DRAFT — review before sending."
- See scheduling-and-dispatch/after-hours-coverage-handoff for the internal handoff and reporting-and-analytics/after-hours-volume-report for volume tracking.

## Unattended (Flows) variant

- A Flow can fire on the ticket-creation event filtered by **timeEntered** (time-of-day) and **dateEntered** (day-of-week) conditions to detect after-hours intake, then call Run Skill to post this acknowledgment as a Reply. Note: Flows are event-triggered — this fires when the ticket is *created* after hours, not "N minutes later."
- Your entire reply is posted verbatim to the client — no narration, no preamble, no internal notes.
- Use only the coverage wording verified for this client's agreement. If coverage terms can't be resolved from the ticket/agreement, post the safe generic form (receipt + "a technician will respond during business hours") and nothing about emergencies.
- When in doubt, do the minimum safe thing: confirm receipt only. Never invent an SLA, an emergency number, or a response time.
