---
name: Client Wiki Page
description: Assemble a one-page client cheat sheet — key contacts, environment quirks, systems, and escalation rules — from ticket history and documentation.
category: Documentation
tools: [search_tickets, search_clients, search_contacts, search_knowledge_base, search_itglue, search_hudu, notion-search, notion-create-pages]
---

# Client Wiki Page

Build the page a tech reads in two minutes before touching a client for the first time:
who to talk to, what is unusual about the environment, and how this client wants to be handled.

## When to use

- "Give me a cheat sheet for <client>" before a first ticket, onsite, or on-call shift.
- A new hire is taking over a client and needs the tribal knowledge in one place.
- After a coverage scramble exposed that only one tech "knows" the client.

## Steps

1. Establish the client record with `search_clients`, then gather evidence:
   - Contacts: `search_contacts` for active people; ticket history for who actually opens tickets, who approves, and who escalates.
   - History: `search_tickets` for the last 6–12 months — recurring issues, sensitive systems, past incidents, tone/communication patterns recorded in notes.
   - Docs: `search_knowledge_base`, `search_itglue` / `search_hudu` where enabled, and `notion-search` if connected — existing client docs, procedures, and standards.
2. Compose the one-pager with these sections (omit empty ones; keep it to one page):
   - **Snapshot** — what the client does, size, environment shape (cloud/on-prem/hybrid) as evidenced.
   - **Key contacts** — primary contact, approver for changes/spend, escalation contact, VIPs whose tickets get priority; each with role and how they prefer contact **as recorded in tickets/docs**.
   - **Systems that matter** — the line-of-business apps and infrastructure that generate tickets, with the doc link for each where one exists.
   - **Quirks & gotchas** — environment oddities the history proves (legacy app that breaks on updates, site with flaky ISP, maintenance windows, "never reboot <server> during business hours").
   - **Escalation & handling rules** — SLAs, after-hours policy, approval requirements, and any standing warnings recorded on tickets.
   - **Recent themes** — top 3 recurring issues with example ticket numbers.
3. Every claim carries its source (ticket number or doc title). Anything remembered-but-unsourced goes in a "Verify with the account owner" list instead of the body.
4. Output the page in chat. If the desk keeps wiki pages in Notion and the requester confirms a location, create it with `notion-create-pages`; otherwise it is a paste-ready draft.

## Guardrails

- **No credentials on the cheat sheet** — reference the credential store by entry title only. No personal data beyond business-contact info.
- Evidence-based only: quirks and preferences must trace to tickets or docs; do not launder hearsay into a wiki page. Unsourced items are flagged for verification, not stated as fact.
- Neutral, professional wording for difficult-contact notes ("prefers written confirmation before changes"), never insults or diagnosis of people.
- **Result-cap honesty:** ticket history may be capped; label theme counts accordingly.
- Draft by default; writing to Notion requires the member's connection and explicit confirmation of the destination.
