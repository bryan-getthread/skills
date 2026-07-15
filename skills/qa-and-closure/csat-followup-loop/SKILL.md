---
name: CSAT Follow-Up Loop
description: Close the satisfaction loop after a ticket closes — make sure the CSAT ask went out (native survey or a form link), pull the response back into the ticket as a note, and flag detractors to a lead the same day.
category: QA & Closure
tools: [search_tickets, add_ticket_note, update_ticket, search_members]
---

# CSAT Follow-Up Loop

CSAT dies in two places: the survey never goes out, or the response never reaches anyone who acts on it. This skill closes both gaps — verifying the ask, recording the answer on the ticket, and making sure a bad score becomes a same-day conversation instead of a quarterly statistic.

## When to use

- "Did the CSAT go out on this ticket?" / "send the satisfaction survey for these closures"
- Pulling survey responses back onto their tickets so the record is complete.
- "Any detractors this week?" — surfacing bad scores with their ticket context.
- The delivery step after Ticket QA Review passes a ticket to close.

## Steps

1. Retrieve the closed ticket(s) with `search_tickets` and check whether a CSAT ask already exists: the desk's native CSAT survey fires on close where configured (verify the closure used the status that triggers it), or a survey link was included in the client-facing closure message. Never send a second ask when one is on record.
2. If no ask went out and the desk uses the native survey: recommend re-driving the close through the triggering status via `update_ticket` (with confirmation), so the platform sends its survey.
3. If the desk runs its survey on a form tool instead: draft the short client-facing CSAT message (thanks, one-line recap of what was resolved, the survey link with the ticket reference). Where the member has connected Zapier, `zapier: Typeform "Create Form"` variants manage the form itself, and delivery goes by client-facing note (or `zapier: Outlook "Send Email"`). Present the draft for approval before anything is sent.
4. Pull responses back: check the thread for a native survey result note; for form-based desks, `zapier: Typeform "Find Response"` filtered by the ticket reference. Match responses to tickets only on an explicit reference (ticket number, hidden field, or the respondent's email on that ticket) — never by timing coincidence.
5. Record each response on its ticket with `add_ticket_note`, internal, plain text: score, verbatim comment, response date, and source (native survey / form). The verbatim comment is quoted exactly — never paraphrased into something nicer.
6. Flag detractors (bottom-tier score or a negative comment): identify the lead or account owner via `search_members` and surface a same-day alert — an internal @mention note on the ticket, and where the desk prefers, `zapier: Microsoft Teams "Send Channel Message"` to the service channel. Include score, comment, client, tech, and the ticket reference. Do NOT draft an apology to the client unattended; the detractor conversation is a human's.
7. Output: per-ticket status table (ask sent / response recorded / no response yet / detractor flagged) and the response rate for the batch.

## Guardrails

- One ask per closure, ever: double-surveying a client is worse than not surveying. If ask history is ambiguous, do not send — flag for a human to check.
- Match responses to tickets on explicit references only; a mismatched detractor note on the wrong client's ticket is a serious error.
- Detractor handling is escalation, not automation: no automated client-facing damage control, no auto-reopening; the flag goes to a human with context.
- Quote survey comments verbatim in notes; scores and comments are never fabricated, extrapolated, or "estimated" for tickets without a response.
- Zapier steps (Typeform, Teams, Outlook) require the member to have connected the Zapier MCP; without it, use the native survey path and client-facing notes, and note the limitation.
- Notes are plain text, no markdown or emojis (PSA sync); survey messages stay short, friendly, and localizable — match the client's language where the thread is not in English.
