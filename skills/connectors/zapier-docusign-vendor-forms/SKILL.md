---
name: Zapier DocuSign Vendor Forms
description: Send vendor/carrier paperwork — LOAs, number-porting forms, circuit orders, domain-transfer authorizations — for signature via DocuSign from a ticket, with the right client signatory and the vendor's own form. The vendor-paperwork variant of Zapier DocuSign Authorization. Use for "send the LOA for the port", "get the carrier form signed", or any vendor process blocked on a signed client form.
category: Connectors
tools: [search_tickets, search_contacts, add_ticket_note, update_ticket, "zapier: DocuSign"]
---

# Zapier DocuSign Vendor Forms

Ports, circuit orders, and transfers stall on paperwork: the vendor requires *their* form (an LOA, a porting authorization) signed by the *client's* authorized representative, and the desk sits in the middle. This skill runs that paperwork through DocuSign from the ticket — the vendor's template, the right signatory, exact-match service details — and holds the vendor process until the signed form is in hand. It is the vendor-paperwork sibling of Zapier DocuSign Authorization, which covers MSP-internal change/offboarding sign-offs.

## When to use

- "Send <client> the LOA so we can port their numbers to the new carrier."
- A vendor/carrier process (port, circuit, domain transfer, license transfer) requires a signed client form before it proceeds.
- "Where's that porting form — signed yet?"
- Filing the signed form and handing it to the vendor with the ticket trail intact.

## Steps

1. **Verify before promising:** DocuSign's core envelope actions (signature request, envelope from template, find envelope, download documents) are in this library's verified inventory — but confirm they're enabled for this tenant (or pinned on its curated server), and confirm the *vendor's form* exists as a template or fillable document in the tenant's DocuSign before promising a send. No template → the form must be obtained from the vendor first; that's a step, not a footnote.
2. **Get the vendor's requirements right:** vendor forms are rejected for exact-match failures, not typos of intent. Collect from the ticket/vendor documentation what this form demands — commonly: service/account number as it appears on the vendor's records, billing telephone number, the authorized name *as the losing/current vendor has it*, and service address. Missing or uncertain fields → ask; a bounced LOA costs the port date.
3. Resolve the signatory: the person the *vendor* will accept — usually the client's account holder or authorized contact on the vendor account, which is not always the ticket requester and not always the MSP's usual approver. Verify via `search_contacts` and the account records; unclear → ask the member, never guess a signer for a carrier document.
4. Fill the form fields from verified facts only; show the member the filled form summary, the signer, and the vendor deadline (port dates are calendar-critical) for confirmation before sending.
5. Send the envelope. `add_ticket_note` (plain text): form, vendor process it unblocks, signer, sent date, and any vendor deadline. Set the ticket to waiting-on-signature via `update_ticket` where the status exists.
6. **The gate:** the vendor submission does not happen until the envelope completes. Check status at sensible checkpoints (`zapier: DocuSign "Find Envelope"`); declined/voided → back to the member with the reason; approaching the vendor's deadline unsigned → escalate to the member, don't just remind again.
7. On completion: download the signed document, submit or hand it to the vendor per the tenant's process (some vendors take uploads, some take email — follow the ticket's vendor thread), file the signed copy durably per tenant convention, and `add_ticket_note` with where it's filed and when it went to the vendor.

## Guardrails

- **Member-connected Zapier required** (the tenant's DocuSign account). Degrade: prepare the filled field values, signer identity, and vendor requirements as a checklist for the member to run in DocuSign directly — and still enforce the gate: no signed form, no vendor submission.
- Exact-match discipline: vendor-record values (account numbers, authorized names, addresses) come from the vendor account records or the client — never normalized, "corrected", or guessed. The vendor's validation is literal.
- Signed-before-submitted is absolute; a port request on a promised signature gets rejected and resets the queue.
- Template fidelity: fill the vendor's form; never edit its language or substitute a home-made form for a vendor-required one.
- The signed artifact is compliance material: file it durably, reference the location on the ticket, and keep account numbers out of any broader-visibility notes.
- Vendor deadlines are first-class: surface them in every status note; a signature that arrives after the port window is a signed failure.
- Task cost: each Zapier call = 2 Zapier tasks — template lookup + send + a few status checks + download is the expected shape; checkpoint on the vendor's timeline, don't poll.
