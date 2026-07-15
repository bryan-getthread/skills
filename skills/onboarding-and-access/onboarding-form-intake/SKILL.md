---
name: Onboarding Form Intake
description: Parse a client's new-hire intake form into the structured onboarding checklist on the ticket, and surface the form link when a form should have been used. Use when an onboarding ticket arrives with a completed intake form, or without one when the client has a form.
category: Onboarding & Access
tools: [search_tickets, search_contacts, search_clients, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, log_time_entry]
---

# Onboarding Form Intake

Turns the client's intake form — attached, pasted, or linked — into the
structured checklist New Hire Onboarding runs from, and closes the loop the
other way: onboarding requests that skipped the form get the form link posted
so intake stays structured.

## When to use

- An onboarding ticket arrives with a completed intake form (attachment, pasted
  answers, or form-notification email).
- "New hire request came in by email — didn't they fill out the form?"
- Standardizing free-text onboarding requests for a client that has a form.
- Embedded in a Flow on the onboarding board to pre-process every new request.

## Steps

1. Find the client's intake-form definition and link in search_knowledge_base /
   search_itglue / search_hudu. If the client has no documented form, skip to
   step 4 and parse the free text as best-effort.
2. Extract from the submission into the standard fields: full name, start date,
   role/title, department, manager, location, hardware needs, requested apps
   and groups, and any mirror-a-user reference. Map form answers to the
   Required / Conditional / Optional checklist categories per New Hire
   Onboarding.
3. Validate: flag missing required answers, a start date in the past or within
   48 hours (priority bump per New Hire Onboarding), an unrecognized manager or
   department (verify via search_contacts), and any answer requesting elevated
   access — those route to approval, not straight onto the checklist.
4. If the request arrived WITHOUT the form and the client has one: post a note
   surfacing the form link, and (per client preference) either reply to the
   requester asking them to submit it, or proceed with what is parseable while
   noting which fields the form would have captured. Do not silently drop
   unstructured requests.
5. Post the structured checklist note in plain text (no markdown or emojis —
   PSA sync): each extracted field with its value or MISSING, the checklist by
   category, and the form link. Update the ticket title to the client's
   onboarding title convention if one is documented. Log time (log_time_entry).

## Guardrails

- Extract only what the form or request actually says; never infer values for
  missing answers (a guessed start date or manager corrupts everything
  downstream). Mark them MISSING instead.
- Elevated-access answers on a form are requests, not approvals — flag them
  for the approval path.
- Do not invent a form link; if none is documented for this client, say the
  client has no intake form on file.
- This skill structures intake only — it provisions nothing.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the ticket note — no narration, no
  questions, plain text only.
- Emit the structured checklist with MISSING markers and the form link. If the
  ticket cannot be confidently identified as an onboarding request, output
  nothing and stop. When in doubt, do nothing.
