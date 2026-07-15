---
name: Zapier OneDrive User Files
description: Work a user's OneDrive during troubleshooting and data-recovery tickets — fetch the specific file in question, run KQL search to locate "lost" documents, and mint sharing links with hygiene. Use for "the user says the file is gone", "find <document> in their OneDrive", or "get me a safe link to that file".
category: Connectors
tools: [search_tickets, search_contacts, add_ticket_note, "zapier: OneDrive"]
---

# Zapier OneDrive User Files

"My file disappeared" is a top-ten ticket, and the answer is usually findable: locate the document with OneDrive **KQL search**, fetch the specific file the ticket is about, and when something must be shared, create the narrowest link that works. The skill touches exactly the files the ticket names — a user's OneDrive is their working life, not a browsing ground.

## When to use

- Data-recovery tickets: "<user> says the Q3 proposal vanished — find it."
- Troubleshooting: "pull the file that won't open so we can check it."
- "Send <user> a link to the recovered file" — with permissions done right.

## Steps

1. Anchor to the ticket: the exact file name/description, the user, and why access is needed. This scope statement is what the notes will justify later — no ticket-relevant reason, no file access.
2. **Locating ("it's gone"):** run KQL search via `zapier: OneDrive` scoped to the user's drive — filename fragments, `filetype:`, and modified-date ranges. Files are usually misplaced, renamed, or auto-saved elsewhere. Report candidates as name + path + modified date **only** — enough for the user to say "that's it" without the desk reading anything.
3. Not found in scope → report the searched terms and scope honestly, and hand off to the tenant's recovery process (recycle bin, retention/restore tooling) — this skill searches; it doesn't promise restoration.
4. **Fetching (troubleshooting):** retrieve the named file only when inspecting it is the ticket's actual task (corrupt file, version check). State what was accessed and why in the ticket note. Content beyond what the ticket needs stays unread and unquoted.
5. **Sharing-link hygiene:** narrowest scope that works — specific-people link to the named recipient over org-wide, org-wide over anyone-with-link; view unless editing is the purpose; expiration set where the tenant supports it. "Anyone with the link" requires the member's explicit okay.
6. Close the loop: `add_ticket_note` (plain text) — what was searched/accessed, what was found (name/path, not contents), any link created and its scope/audience. The access trail is part of the deliverable.

## Guardrails

- **Member-connected Zapier required** (Microsoft account whose permissions actually reach the user's OneDrive — often via admin/delegated access; if the connected account can't see the drive, say so rather than concluding the file doesn't exist). Degrade: give the user self-serve search/recycle-bin steps.
- Minimum-necessary access is the product: search results are reported as metadata; file contents are opened only when inspection is the task, and never quoted into tickets beyond the ticket's need.
- Every access is ticket-justified and noted — an unexplained trawl through an employee's files is an HR incident, not support.
- Sharing links default private-and-narrow; never widen an existing link's scope to save a step.
- No conclusions beyond evidence: "not found by these searches in this scope" — never "the file never existed" or "it was deleted by <user>".
- Task cost: each Zapier call = 2 tasks — one well-formed KQL search beats five vague ones; refine terms before re-calling.
