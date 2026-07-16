---
name: Zapier HubSpot Marketing Clean
description: When tickets reveal contact changes — someone left the company, changed email, changed role — push the hygiene update into HubSpot so sales and marketing stop emailing ghosts. The CRM-hygiene sibling of Zapier HubSpot Ticket Sync. Use for "update HubSpot, she left", "their new email is on the ticket — fix the CRM", or periodic hygiene passes over departure/offboarding tickets.
category: Connectors
tools: [search_tickets, search_contacts, add_ticket_note, "zapier: HubSpot"]
---

# Zapier HubSpot Marketing Clean

The desk learns about people-changes before anyone else: offboarding tickets say who left, bounced-reply tickets say whose email died, new-hire tickets say who replaced whom. This skill turns those ticket facts into HubSpot contact hygiene — updates on the *right* existing record, evidence-first — so the CRM stops carrying ghosts. It writes hygiene fields only; expansion signals and engagements belong to Zapier HubSpot Ticket Sync.

## When to use

- An offboarding ticket confirms a client contact has left — mark it in HubSpot.
- A ticket reveals a changed email, phone, title, or company move for a known contact.
- "Sales keeps emailing someone who left in March — clean it up from the offboarding tickets."
- A periodic hygiene pass over recent offboarding/departure tickets.

## Steps

1. **Verify before promising:** HubSpot's Zapier inventory is in this library's verified list (contacts/companies/tickets/deals CRUD + finds), but confirm the specific contact-update and find actions are enabled for this tenant (or pinned on its curated server), and check which lifecycle/status property the tenant uses to mark departed contacts — that property is tenant convention, not a HubSpot universal.
2. **Evidence gate:** the change must be explicit in a ticket — an offboarding request naming the person, a stated new email, an NDR-driven correction confirmed by the client — not inferred from silence or a hunch. Record which ticket evidences the change.
3. **Find, never create:** resolve the contact via `zapier: HubSpot "Find Contact"` (current email first; name + company when the email is what changed). Exactly one match → proceed. Zero → this skill stops; hygiene means correcting existing records, and whether a missing contact *should* exist is Ticket Sync's find-or-create conversation, not this one's. Multiple → show the candidates and ask; updating the wrong person's record is the exact harm this skill exists to prevent.
4. Compose the update, shaped by the change type:
   - **Departure:** set the tenant's departed/no-longer-with-company convention (lifecycle/status property, not record deletion), clear forward-looking fields per tenant convention, and note the departure date and evidencing ticket.
   - **Email/phone/title change:** update the field to the evidenced new value; keep the old email findable per tenant convention (HubSpot dedupes on email — changing it is identity surgery, so show old → new explicitly).
   - **Moved companies:** mark the departure on the old association; creating them fresh at the new company is a member decision (and often a sales conversation), not an automatic write.
5. Show the member: record found, old → new values, and the evidencing ticket. Confirm, then write.
6. Add a HubSpot note on the contact/company: what changed, effective date, "per service ticket <number>" — so sales sees provenance, not a mystery edit.
7. Close the loop: `add_ticket_note` (plain text) on the evidencing ticket — CRM updated, which record, what changed. Output ends with: records updated, records skipped (zero/multiple match) and why.

## Guardrails

- **Member-connected Zapier required** (the tenant's HubSpot). Degrade: output the change list (contact, old → new, evidencing ticket) formatted for the CRM owner to apply, and still note the pending hygiene on the ticket.
- **Never delete, never merge:** departed contacts get marked per tenant convention, not erased — history, attribution, and compliance hang off those records. Merging duplicate records is a CRM-admin decision.
- Hygiene fields only: this skill never touches deal stages, owners, list memberships, or marketing-consent/subscription properties — a support desk unsubscribing or resubscribing people creates compliance problems it can't see.
- Evidence-first, always: every write traceable to a ticket; "I think she left" cleans nothing.
- Wrong-record risk is the primary failure: ambiguity resolves to asking, never to best-guess writes on someone's identity fields.
- Contact data stays minimal: business contact fields only — no personal forwarding addresses or departure circumstances beyond "no longer with company".
- Task cost: each Zapier call = 2 Zapier tasks — one find + one update (+ one note) per contact; batch hygiene passes and cap them, don't sweep the whole CRM.
