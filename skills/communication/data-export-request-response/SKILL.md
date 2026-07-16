---
name: Data Export Request Response
description: Draft the reply to a client asking for a copy/export of their data — acknowledge, confirm scope and identity, and route to the authorized export process. The email itself never contains the data. Use for data-access/export requests.
category: Communication
tools: [search_tickets, search_contacts, view_openDraft]
---

# Data Export Request Response

Draft the acknowledgment a client receives when they request a copy or export of their data (a mailbox, files, a report, a subject-access-style request): confirm the request, clarify exactly what's being asked for, verify it's coming from an authorized person, and route it into the proper, secure export process. The reply informs and routes — it never carries the data or a data link itself.

## When to use

- "The client wants an export of all their data — draft the response."
- "Someone's asking for a copy of their mailbox/files — how do we reply?"
- A data-access, export, or "send me my data" request lands on a ticket.

## Steps

1. Read the request and confirm from the contact record that the requester is authorized to receive this data before drafting anything (search_contacts). If authorization is unclear, the draft must ask for verification, not proceed.
2. Draft the reply:
   - **Acknowledge** — confirm the request was received and is being handled.
   - **Clarify scope** — restate what you understand they want and ask them to confirm/narrow (what data, which timeframe, what format). Scope precision protects everyone.
   - **Explain the process** — that the export will be handled through the secure, authorized channel, with the expected next step and rough timing (only if known).
   - **Set expectations** — any approval, verification, or turnaround the process requires.
3. Keep it calm and helpful; a data request can carry legal or privacy weight, so be precise and non-committal on anything not confirmed.
4. Structure and defensive tone per the Email Baseline Standard (communication/email-baseline-standard).
5. Present the draft with view_openDraft for review. Do not send.

## Guardrails

- **No data in the email.** The reply must never contain the exported data, a raw dump, credentials, or an unsecured download link. Data moves only through the authorized, secure export process — not this message. (Sharing/permission changes and file downloads are separate approved actions, handled outside this draft.)
- **Verify authorization first.** Do not confirm you'll hand over data until the requester's authority is established. When in doubt, the draft asks for verification and routes to the right owner (account manager / security / compliance).
- **Don't promise scope or timing you can't confirm.** Route legal/compliance-flavored requests (subject access, litigation) to the appropriate owner rather than committing on their behalf.
- Degradation: view_openDraft is in-app only. Over external MCP, output the draft in chat labeled "DRAFT — review before sending."
- Related: onboarding-and-access/litigation-hold and compliance-and-audit skills for the underlying process; this skill is the client-facing acknowledgment only.
