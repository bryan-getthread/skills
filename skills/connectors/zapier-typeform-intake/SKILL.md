---
name: Zapier Typeform Intake
description: Run structured request forms — new hire, hardware, access — through Typeform into properly attributed Thread tickets, and look up a submission's answers mid-ticket. Use for "make the new-hire Typeform create tickets", "pull what they answered on the request form", or building a structured intake path on Typeform.
category: Connectors
tools: [create_ticket, search_tickets, search_clients, search_contacts, add_ticket_note, create_flow, "zapier: Typeform"]
---

# Zapier Typeform Intake

Typeform is where many MSPs put the requests that must arrive complete: new-starter forms, hardware requests, access changes. This skill converts a submission into a ticket that carries every answer faithfully — one response, one ticket, with the response identity recorded so nothing is ticketed twice or dropped — and pulls a submission's answers back mid-ticket when a tech asks "what exactly did they request?".

## When to use

- "When the new-hire Typeform is submitted, open an onboarding ticket with everything mapped."
- Mid-ticket: "look up their form response — did they ask for a laptop or a desktop?"
- Standing up or tightening a structured request form and its ticket path.

## Steps

**Response → ticket (the mapping discipline):**
1. One response = one ticket. The submission's identity (Typeform response ID, or submitted-at + respondent) goes into the ticket description — this is the dedupe key.
2. **Dedupe before creating:** `search_tickets` for that response identity. Found → report the existing ticket; never create a sibling.
3. Map, don't summarize: every question and answer lands in the ticket description as `question: answer` pairs (plain text), in the form's order; free-text verbatim; skipped questions recorded as unanswered — on a structured form, a blank is information.
4. Attribute deliberately: client via `search_clients` (from the form's binding or an answer), contact via `search_contacts` on the respondent's email. Ambiguous → create unattributed and flag for triage; never bind on name similarity.
5. Title per the desk's intake standard: form type + distinguishing answer ("New hire — <user>, starts <date>"); board/priority per the tenant's mapping for that form. Date-sensitive answers (start dates, deadlines) get surfaced in the title/description, not buried.
6. Recurring intake belongs in a Flow: wire the Zapier Typeform new-entry trigger into a ticket-create path via `create_flow`, embedding steps 1–5 as the mapping rules; unattended runs follow Unattended Output Discipline.

**Mid-ticket lookups:**
7. Use the Zapier Typeform response lookup to fetch the original submission; quote the relevant answers onto the ticket via `add_ticket_note` (plain text) with the form name and submission time. (For CSAT-survey response loops, keep this same lookup pattern — the discipline is identical.)

## Guardrails

- **Member-connected Zapier required** (the Typeform account owning the forms). Degrade: ask the requester to paste the response and map from the paste — same discipline, manual transport.
- The response-identity-in-ticket rule is non-negotiable; it is the only defense against duplicate and dropped submissions.
- The ticket says what the form said — never enrich with assumed answers or inferred scope; a structured form's whole value is that it isn't interpreted.
- Form responses are personal data (new-hire forms especially): map what intake needs, and don't propagate sensitive answers beyond the ticket that needs them.
- Changes to a live intake Flow are confirm-before-save; a silently broken intake path drops real requests.
- Task cost: each Zapier call = 2 tasks — the trigger fires per submission by design; one lookup per ticket, not per question.
