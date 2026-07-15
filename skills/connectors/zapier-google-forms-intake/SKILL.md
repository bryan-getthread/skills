---
name: Zapier Google Forms Intake
description: Turn Google Forms responses into properly attributed Thread tickets — new-response triggers as intake, response lookups for context, and a strict one-response-one-ticket mapping discipline. Use for "make form submissions create tickets" or "pull up what they answered on the intake form" when the form vendor is Google.
category: Connectors
tools: [create_ticket, search_tickets, search_clients, search_contacts, add_ticket_note, create_flow, "zapier: Google Forms"]
---

# Zapier Google Forms Intake

Forms are how structured requests (new starter, hardware, access) arrive at many desks. This skill turns a Google Forms response into a ticket that carries the response faithfully — every answer mapped to the intake standard, the response identity recorded so no submission is ever ticketed twice or lost.

## When to use

- "When someone submits the new-hire form, open a ticket on the onboarding board."
- "Ticket came from the hardware-request form — pull the full response for context."
- Designing or tightening a form→ticket intake path for a client.

## Steps

**Response → ticket (the mapping discipline):**
1. One response = one ticket, always. The response's identity (timestamp + respondent, or the form's response ID when exposed) goes into the ticket description — this is the dedupe key.
2. **Dedupe before creating:** `search_tickets` for that response identity. Found → do not create a second ticket; report the existing one.
3. Map, don't summarize away: every form answer lands in the ticket description as `question: answer` pairs (plain text), in the form's order. The requester's free-text goes in verbatim. Unanswered questions are recorded as unanswered — absence is information on an intake form.
4. Attribute deliberately: resolve the client from the form's context or an answer via `search_clients`; the contact from the respondent's email via `search_contacts`. Ambiguous → create unattributed and flag for triage rather than guessing; never bind a ticket to a client on name similarity.
5. Title per the desk's intake standard: form name + the distinguishing answer ("New hire — <user>, starts <date>"). Board/priority per the tenant's mapping for that form type.
6. Recurring intake belongs in a Flow: build the trigger→create path with `create_flow` (Zapier "New Form Response" trigger feeding the ticket-create), embedding steps 1–5 as the mapping rules. Unattended runs follow Unattended Output Discipline.

**Context lookups:**
7. Mid-ticket, pull the original response via the Zapier Google Forms response lookup when the ticket references a submission; quote the relevant answers onto the ticket with `add_ticket_note` (plain text), citing the form and submission time.

## Guardrails

- **Member-connected Zapier required** (Google account with access to the form/its response sheet). Degrade: ask the requester to paste the response and map from the paste — same discipline, manual transport.
- The response-identity-in-ticket rule is non-negotiable; it is the only thing standing between the desk and duplicate/dropped submissions.
- Never enrich a response with assumed answers; the ticket says what the form said, and only that.
- Form responses contain personal data — map what intake needs; don't propagate sensitive answers (e.g. an HR note) beyond the ticket that needs them.
- Changes to a live intake Flow are confirm-before-save; a broken intake path silently drops requests.
- Task cost: each Zapier call = 2 tasks — the trigger fires per response by design; keep lookups to one per ticket, not per question.
