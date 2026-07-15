---
name: Zapier SharePoint Ticket Filing
description: File ticket artifacts — reports, PIRs, signed docs, exports — into the client's SharePoint library with correct foldering and sharing-link hygiene (expiry, least privilege). Use for "put this in the client's SharePoint", "file the incident report to their site", or "share this document with the client safely".
category: Connectors
tools: [search_tickets, add_ticket_note, "zapier: SharePoint"]
---

# Zapier SharePoint Ticket Filing

Ticket-born documents deserve a permanent, findable home: upload artifacts to the right client's SharePoint library via `zapier: SharePoint "Upload File"`, in the agreed folder structure, and share them with links that expire instead of links that leak.

## When to use

- "File the post-incident report to <client>'s SharePoint documentation library."
- "Upload the signed change authorization to their site and link it on the ticket."
- "Send the client a link to their monthly report — safely."

## Steps

1. Identify the artifact and its client. Resolve the destination: `zapier: SharePoint "Find Site"` / "Find File or Folder" to confirm the client's library and target folder — on first use for a client, confirm the mapping with the member and follow the tenant's folder convention (e.g., /Documentation/<year>/) rather than inventing one.
2. Name the file to the desk's convention: date-prefixed, client and subject in the name, no internal shorthand ("2026-07-15 <Client> Post-Incident Report — <system>").
3. Confirm destination + filename, then upload via `zapier: SharePoint "Upload File"` (or "Replace File" only when the member explicitly wants a new version of an existing document).
4. **Sharing-link hygiene**, when a link is needed:
   - Scope the link to specific people or the client's existing site access — no "anyone with the link" for client documents unless the member explicitly overrides.
   - Set an expiry on external links (default: 30 days; shorter for sensitive material).
   - View-only unless editing is the point.
   - When a temporary broad share was used, schedule its cleanup: `zapier: SharePoint "Remove Sharing Permissions"` after the stated window.
5. `add_ticket_note` (plain text): what was filed, where (site/folder/filename), the link if one was created, and its expiry.

## Guardrails

- **Member-connected Zapier required** (Microsoft account with rights to the client's site). Degrade by preparing the document + a filing instruction note for the member to upload manually.
- Cross-tenant filing is the catastrophic failure: verify the site belongs to the artifact's client before upload — never resolve a site by name similarity between clients. For recurring/unattended filing, use a curated Zapier server with the site/library field locked per workflow.
- Replace-vs-upload: replacing a file destroys the previous version for viewers — only on explicit instruction.
- No credentials or secret values inside filed documents' shareable copies; reference the credential store.
- Every external link gets an expiry and a note on the ticket; a link nobody remembers issuing is a standing exposure.
- Task cost: each Zapier call = 2 tasks; a normal filing is 2–3 calls (find, upload, share) — avoid per-file permission audits in loops.
