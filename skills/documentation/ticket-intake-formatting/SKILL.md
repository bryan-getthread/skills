---
name: Ticket Intake & Formatting
description: Build a clean ticket title and description from a raw report, voicemail, email, form submission, or rough technician notes, following the house intake standard.
category: Documentation
tools: [search_tickets, create_ticket, update_ticket, add_ticket_note, run_assistive_ai]
---

# Ticket Intake & Formatting

Turn unstructured intake into a standardized title and description a dispatcher or
technician can act on immediately — restructured, not summarized away.

## When to use

- "Create a ticket from this email / voicemail transcript / chat paste."
- A ticket arrived with a junk title ("help!!", "FW: FW: printer") and needs reformatting to standard.
- A web-form or vendor-alert submission needs its fields parsed into the description.
- Rough tech notes from a call need to become a proper ticket.

## Steps

1. Read the raw source and extract: **who** (contact + <client>), **what** (the symptom, verbatim where useful), **when** (onset, recurrence), **impact** (users/systems affected, urgency signals), and any error text.
2. Write the **title** in the house convention. Default format when the desk has not defined one: `<Client> - <System/Device> - <Symptom>` (e.g. `<client> - VPN - Cannot connect from home office`). Keep it under ~80 characters, symptom-first, no "URGENT!!!" styling.
3. Write the **description** as structured intake:
   - Reported by / affected user(s)
   - Symptom and exact error messages
   - Onset and frequency
   - Business impact
   - Environment facts stated in the source (<device>, OS, application)
   - Original source text preserved below a separator when it adds detail.
   - **Do not put troubleshooting steps in the description.** Suggested or attempted fixes belong in a separate internal note — the description records the problem, not the work.
4. **Form-field parsing:** if the source is a structured form or alert email, map each labeled field to the intake sections; keep unmapped fields verbatim in a "Raw fields" block rather than dropping them.
5. If a follow-up or callback date is requested, compute it **weekend-adjusted**: a follow-up landing on Saturday or Sunday moves to the next business day (respect stated desk holidays if known). State the adjusted date explicitly.
6. Confirm the extracted details, then `create_ticket` (or `update_ticket` for reformatting) with the right board/status/priority; use `run_assistive_ai` for category/priority suggestions when requested. Post attempted-fix info, if any, with `add_ticket_note` as internal.

## Guardrails

- Restructure, do not summarize away specifics — exact error text, names, and timestamps survive intact.
- Only create or modify a ticket after confirming the extracted details are right; never guess the client/contact from name similarity — low confidence → ask, do not assign.
- Never place troubleshooting content, credentials, or internal commentary in the client-visible description.
- If the desk runs non-English, produce the title/description in the desk's working language.

## Consolidates

Format-ticket-title-and-description, structured ticket creation, form-intake parsing, and note-intake skills.
