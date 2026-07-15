---
name: PIR Document Generator
description: Assemble a client-ready post-incident review document from the incident ticket plus, where the member has connected them, calendar, email, and chat evidence — into a branded template.
category: Documentation
tools: [search_tickets, add_ticket_note, notion-search, notion-fetch, notion-create-pages]
---

# PIR Document Generator

Build the formal post-incident review deliverable a client receives after a major incident:
ticket evidence plus cross-tool evidence sweep, poured into the MSP's branded template.

## When to use

- "Prepare the PIR for <client>'s outage."
- A P1 closed with a commitment to deliver a post-incident review.
- An account manager needs the incident package before a client debrief meeting.

## Steps

1. Establish the incident record: pull the primary and related tickets with `search_tickets` — thread, notes, time entries, status history, and client communications.
2. **Evidence sweep (member-connected sources only, and only with the requester's go-ahead):**
   - Meetings/bridge calls: `notion-search` / `notion-fetch` for incident meeting notes if Notion is connected.
   - Email/chat/calendar evidence held in Microsoft 365 or Slack: via the tenant's configured Zapier connector (e.g. `zapier: Outlook "Find Emails"`, `zapier: Outlook "Find Events"`) when available.
   - Anything not reachable via connected tools: list it as an evidence request for the requester to paste in, rather than skipping silently.
3. Assemble the PIR using the desk's template if one is provided; otherwise use this default with **branded-template placeholders** left intact for the human to fill:
   - `{{MSP_LOGO}}` / `{{MSP_NAME}}` / `{{CLIENT_NAME}}` / `{{DOCUMENT_DATE}}` / `{{PREPARED_BY}}`
   - Sections: Incident overview · Detection & response timeline · Impact assessment · Root cause · Remediation performed · Preventative actions & owners · Communication log · Appendix: evidence index.
4. The communication log lists each client touchpoint (time, channel, summary) sourced from the ticket and swept evidence — every entry cites its source.
5. Sanitize: no credentials, no internal-only commentary, no blame, no unconfirmed "breach/compromise" language. Client-safe throughout.
6. Output the document in chat. If the requester wants it in Notion, create it with `notion-create-pages` in the location they specify — after they confirm. Optionally post an internal note on the ticket (`add_ticket_note`) recording that the PIR was drafted and where it lives.

## Guardrails

- Evidence-cited or absent: every timeline and communication entry traces to a ticket event or a swept artifact. Gaps are listed as "evidence needed", never filled in from plausibility.
- Notion and Zapier sources are **member-authenticated** — they work only if this member connected them; degrade to ticket-only evidence plus an explicit "not swept" list otherwise.
- Leave `{{...}}` placeholders unfilled rather than guessing branding, names, or dates.
- The PIR is a draft deliverable — a human reviews before anything is sent to the client; this skill never emails or shares the document externally.
- Blameless, client-safe language; root-cause claims must match the RCA evidence (pair with post-mortem-rca-author for the analysis itself).
