---
name: IT Glue Asset & Contact Lookup
description: On demand, pull a client's IT Glue contacts, credential links, and flex-asset configurations and post a plain-text summary with deep links onto the ticket — degrading to the KB or ticket history when IT Glue isn't connected. Answers the commonly requested "get me the IT Glue info for this client" workflow; runs manually on demand.
category: Devices & Infrastructure
tools: [search_itglue, search_knowledge_base, search_tickets, add_ticket_note]
---

# IT Glue Asset & Contact Lookup

The client-facing facts a tech needs mid-ticket — key contacts, where the credentials live, the config of a documented asset — usually sit in IT Glue. Pull them on request and drop a clean, plain-text summary with deep links right onto the ticket.

## When to use

- "Pull <client>'s IT Glue contacts / key people for this ticket"
- "Where are the credentials for <system> in IT Glue?"
- "Get the flex-asset config for <asset> and post it here"
- Any ticket where the tech needs the documented environment facts fast

## Steps

1. **Connection check first.** This needs IT Glue enabled for the tenant. If search_itglue isn't available, say so and go to the degrade path (step 5) instead of implying a search happened.
2. Identify the client and what's being asked (contacts, credential locations, or a specific flex-asset / configuration). Run search_itglue scoped to that client and asset type.
3. Assemble a plain-text summary suitable for a PSA-synced note (no markdown, no emojis):
   - **Contacts** — name, role, and how to reach them, for the key people only.
   - **Credentials** — the *label and deep link* to the IT Glue password entry, never the secret value itself.
   - **Configs / flex assets** — the fields the ticket needs (make/model, IP, versions, notes) with a deep link to the full record.
4. Include the IT Glue deep link for each item so the tech can open the authoritative record in one click.
5. **Degrade path.** If IT Glue is absent or has nothing: fall back to search_knowledge_base, then search_tickets for prior tickets on the same client/asset that captured the facts. State clearly which source the info came from, and if none has it, say so.
6. Preview the summary; on confirm, add_ticket_note it to the ticket. If the member only wanted to read it, skip the note.

## Guardrails

- Never surface actual secrets. Post the credential's label and deep link only — the value stays in IT Glue behind its own access control.
- Confirm before writing the note; posting to the ticket waits on an explicit go-ahead.
- Plain-text output for PSA compatibility.
- Cite real IT Glue deep links only; never fabricate a record, contact, or URL. If a field is missing, leave it out rather than inventing it.
- Verify the record's freshness where the data shows a last-updated date, and flag anything that looks stale.
- Member-invoked only; no unattended variant (Flows can't schedule it, and credential-adjacent lookups should stay human-initiated).
