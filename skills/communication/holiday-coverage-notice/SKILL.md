---
name: Holiday Coverage Notice
description: Draft the holiday or reduced-hours notice for clients — dates, what changes, and an unmissable emergency path — verified against each client's actual coverage terms before anything goes out.
category: Communication
tools: [search_clients, search_tickets, view_openDraft]
---

# Holiday Coverage Notice

Drafts the schedule-change notice that prevents the two failure modes of every holiday: the client who didn't know the desk was closed, and the client with 24/7 coverage who was wrongly told it was.

## When to use

- "Draft the holiday closure notice for <holiday>."
- "We're on reduced hours next week — notify our clients."
- Year-end planning: the desk wants the seasonal coverage announcement prepared.

## Steps

1. Collect the schedule facts from the requester: which dates are affected, what the desk's hours are on each affected day (closed, reduced with hours, normal), and when full service resumes. Dates and hours come from the requester — never assume a holiday's observance or duration.
2. **Per-client accuracy check** — the step that keeps this notice honest: coverage varies by agreement. Ask the requester which clients have after-hours or 24/7 coverage that continues through the holiday, or check the client records via search_clients where plan terms are recorded. Clients whose coverage is unaffected need a *different* notice (or none) than clients facing a closure. If the recipient list mixes coverage levels, produce the variant messages rather than one wrong blanket message.
3. Draft per the Email Baseline Standard skill (communication/email-baseline-standard):
   - Dates and status first: "<dates>: closed / reduced hours <hours>." Scannable, no prose burying the facts.
   - **Emergency path, unmissable:** its own short section — exactly how to reach emergency support during the affected period (number/channel), what qualifies as an emergency, and what response to expect. This section is mandatory in every variant; a closure notice without an emergency path is incomplete.
   - What happens to non-urgent requests submitted during the period: queued and answered from <resume date>. Reset the expectation explicitly so the ticket-silence doesn't read as neglect.
   - When normal service resumes, as a specific date.
4. Timing guidance: recommend sending at least a week ahead, with a short reminder the day before if the desk does reminders. Note this in chat, not in the letter.
5. Present the draft(s) via view_openDraft — one per coverage variant, clearly labeled with which client group each targets. Distribution is a human action.

## Guardrails

- **Per-client schedule accuracy is the whole game:** never send a closure notice to a client whose agreement includes coverage through that period. When coverage terms can't be verified, say so and ask — do not default to the blanket message.
- The emergency path must be real and verified with the requester: a phone number or channel that is actually staffed during the period. Never invent a hotline.
- Dates, hours, and observed holidays come from the requester — regional holidays differ and many desks serve clients across regions/time zones; state times with a time zone.
- No cutesy holiday filler that buries the operational facts; greetings are fine, one line, after the information.
- Degradation: view_openDraft is in-app only — over external MCP, output the notice(s) in chat labeled "HOLIDAY COVERAGE DRAFT" with the target client group for each.
