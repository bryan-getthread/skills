---
name: Zapier Dropbox Log Collection
description: Collect logs, screenshots, and diagnostic files from an end user via a Dropbox File Request — no account needed on their side — and deliver files back with expiring, password-protected links. Use for "get the logs from <user>", "collect the crash dumps", or "send the user this file securely".
category: Connectors
tools: [search_tickets, search_contacts, add_ticket_note, "zapier: Dropbox"]
---

# Zapier Dropbox Log Collection

The attachment dance, solved: a `zapier: Dropbox "Create File Request"` link lets the user upload big diagnostic files straight into a ticket-named folder (no Dropbox account required), and deliverables go back out on expiring, password-protected links instead of naked attachments.

## When to use

- "Ask <user> to send the <application> logs — they're 200 MB."
- "Collect screenshots from the three affected users."
- "Send the user the installer/report with a link that expires."

## Steps

**Collecting (File Request):**
1. Determine exactly which files to ask for, per system: the specific log paths, how to find them, roughly how large. Write collection instructions the user can follow ("In <application>, Help → Export Logs, save the .zip").
2. `zapier: Dropbox "Create File Request"` with a destination folder named for the ticket ("/File Requests/<ticket number> — <client>") and a deadline matching the ticket's urgency.
3. Send the request link through the ticket's reply channel with the collection instructions and one explicit caution: **do not upload password lists, exports containing credentials, or personal files** — only the requested diagnostics.
4. `add_ticket_note` (plain text): file request created, what was requested, link deadline.
5. On follow-up, check the destination folder (`zapier: Dropbox "Find File/Folder"`); summarize what arrived vs what was requested; chase what's missing once before escalating to a call.

**Delivering (outbound files):**
6. Upload the deliverable to the ticket's folder and create a shared link with an expiry and password where the tenant's Dropbox plan supports it; send link and password through **separate channels** (link via ticket email, password via SMS/phone). Note the link and its expiry on the ticket.

## Guardrails

- **Member-connected Zapier required** (tenant's Dropbox account). Degrade to the desk's standard attachment path or secure-upload portal, and say which was used.
- Collected files may contain sensitive data regardless of the caution — treat the folder as confidential, reference files on the ticket by name/size rather than pasting contents, and follow the tenant's retention practice (propose cleanup when the ticket closes).
- Never deliver credentials via Dropbox links; credential delivery follows the desk's secure-credential procedure, full stop.
- Expiry on every outbound link, always noted on the ticket; password-protect anything client-confidential.
- One reminder per file request before escalating to a human touch — nagging uploads doesn't work.
- Task cost: each Zapier call = 2 tasks; check the folder when the user says "done" or at the deadline, not on a polling loop.
