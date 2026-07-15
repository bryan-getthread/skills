---
name: Zapier DocuSign Authorization
description: Send a change-authorization or offboarding-acknowledgment for signature from a ticket via DocuSign, track completion, and file the signed document. Use for "get sign-off on this change", "send the offboarding acknowledgment for signature", or any work that needs a signed authorization before it proceeds.
category: Connectors
tools: [search_tickets, search_contacts, add_ticket_note, update_ticket, "zapier: DocuSign"]
---

# Zapier DocuSign Authorization

Signature-grade approval for signature-grade actions: send the tenant's authorization template via `zapier: DocuSign "Create Signature Request"` (envelope from template), hold the gated work until the envelope completes, and file the signed document where audits can find it.

## When to use

- A change (firewall rules, domain transfer, data deletion) requires the client's signed authorization.
- Offboarding: the client sponsor signs the acknowledgment of access removal and data handling.
- "Send <client> the authorization form for the migration and track it."

## Steps

1. Identify the correct template: `zapier: DocuSign "Envelope from Template"` using the tenant's existing change-authorization / offboarding-acknowledgment templates. No template for the case → stop and tell the member; do not compose ad-hoc legal-ish documents.
2. Resolve the signer from the ticket/contact record (`search_contacts`): the client's *authorized* signer per the tenant's policy, not merely the requester — for offboarding, the requester is sometimes the person losing access; the signer never is.
3. Fill template fields from ticket facts only: what's authorized, systems affected, effective date, ticket reference. Show signer + filled fields for confirmation.
4. Send the envelope. `add_ticket_note` (plain text): envelope sent, template, signer, date. Set the ticket to the desk's waiting-on-signature status via `update_ticket` if one exists.
5. **The gate:** the authorized work does not start until the envelope status is completed. Check status on follow-up (`zapier: DocuSign "Find Envelope"`); declined or voided → route back to the member with the reason; expired/silent past the tenant's window → one reminder, then human follow-up.
6. On completion: download the signed document (`zapier: DocuSign "Download Envelope Documents"`), file it per the tenant's convention (client's SharePoint library — see Zapier SharePoint Ticket Filing — or attach to the ticket), and note completion with the storage location.

## Guardrails

- **Member-connected Zapier required** (tenant's DocuSign account). Degrade by preparing the field values and signer details for the member to send from DocuSign directly — and still enforce the gate: no signature, no action.
- Signed-before-started is absolute: never begin the gated work on a promise, a verbal, or an envelope that is sent-but-not-completed.
- Signer identity comes from the authorization policy/contact record; when the authorized signer is unclear, ask the member — never guess a signer for a legal document.
- Template fidelity: fill fields; don't edit template language. Wording changes are a human/legal decision.
- The signed artifact must end up somewhere durable and be referenced on the ticket — an unfiled signature protects no one.
- Task cost: each Zapier call = 2 tasks; check envelope status at sensible checkpoints, not on a polling loop.
