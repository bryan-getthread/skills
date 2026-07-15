---
name: Zapier Google Drive Filing
description: File ticket artifacts — reports, configs, sign-off documents — into the client's Google Drive folder structure with sharing-permission hygiene. Use for "put this in the client's Drive folder", "share the report with <user>", or artifact filing when the document store is Drive, not SharePoint.
category: Connectors
tools: [search_tickets, search_clients, add_ticket_note, "zapier: Google Drive"]
---

# Zapier Google Drive Filing

The Drive twin of Zapier SharePoint Ticket Filing: land ticket artifacts in the client's agreed Drive location via `zapier: Google Drive "Upload File"`, in the right folder, with a link that reaches exactly the intended audience and nobody else — and the file's location recorded on the ticket.

## When to use

- "File the network assessment under <client>'s project folder in Drive."
- "Upload the signed change-approval PDF and link it on the ticket."
- "Share this report with <user>" — where "share" means a Drive link, done safely.

## Steps

1. Resolve the destination: the tenant's client-folder convention (e.g. Clients/<client>/<category>). Ambiguous or no established folder → ask; a misfiled document in another client's folder is a confidentiality incident, not a tidiness problem.
2. Verify the folder exists with `zapier: Google Drive "Find a Folder"`; create the standard subfolder via "Create Folder" only when the convention calls for it and the member confirms.
3. Name by convention: `<date> - <ticket-number> - <artifact-type>` unless the tenant has its own standard. Findability is the point of filing.
4. Upload via `zapier: Google Drive "Upload File"`, then confirm the file landed where intended (folder path in the result).
5. **Sharing-link hygiene — the heart of this skill:**
   - Default is inherit-from-folder / restricted. Do not create a share link at all unless someone outside the folder's existing audience needs access.
   - Share to **named accounts** (the client contact's address from `search_contacts`) over "anyone with the link"; anyone-with-link is only for content that is genuinely fine to be public, and needs the member's explicit okay.
   - Viewer unless editing is the purpose. Never grant broader-than-needed roles to save a future request.
6. `add_ticket_note` (plain text): what was filed, the folder path, and who it was shared with at what level. The ticket must be able to answer "where is that document and who can see it".

## Guardrails

- **Member-connected Zapier required** (Google account with rights to the client folder tree). Degrade: attach/store per the tenant's fallback and give the member a filing checklist — never park client artifacts in a personal Drive as a workaround.
- Wrong-client-folder is the catastrophic failure: confirm the resolved client (`search_clients`) and destination path before upload, every time.
- Sharing defaults conservative: restricted > named accounts > anyone-with-link, and the escalation up that ladder is a member decision, not a convenience.
- Never overwrite an existing file silently; a name collision means version it per convention or ask.
- No credentials in filed artifacts destined for client-visible folders; secrets go in the tenant's documentation system (IT Glue/Hudu), not Drive.
- Task cost: each Zapier call = 2 tasks — find folder + upload (+ share) is the expected shape; don't re-verify the folder tree on every file of a batch.
