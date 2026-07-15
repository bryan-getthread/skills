---
name: Config Change Log
description: Append a structured change-log entry to the client's change log (Notion database or doc platform) for every approved and completed change ticket.
category: Documentation
tools: [search_tickets, notion-search, notion-fetch, notion-create-pages, notion-update-page, add_ticket_note]
---

# Config Change Log

Keep a durable, per-client record of configuration changes: one structured entry per
approved change ticket, appended to wherever that client's change log lives.

## When to use

- A change ticket (firewall rule, DNS, MFA policy, server config) just completed and needs logging.
- "Log this change for <client>."
- Embedded in a Flow on change-board ticket closure to keep the log automatic.
- Audit prep found the change log has holes and recent tickets need back-filling.

## Steps

1. Read the change ticket with `search_tickets` and verify two facts before anything else: the change was **approved** (approval record, `send_approval` result, or explicit sign-off in the thread) and **completed** (executed, not just planned). Fail either check → stop and report why.
2. Build the structured entry with these fixed fields:
   - Date/time implemented · Client (<client>) · System/asset affected · Change summary (one line) · What changed (old → new where stated) · Reason/driver · Approved by (role, from the ticket) · Implemented by (role) · Ticket number · Rollback note (if the thread records one).
3. Locate the client's change log:
   - **Notion database:** `notion-search` for the client's change log; append a row with `notion-create-pages` (page in the database) with the fields as properties.
   - **Notion page:** append the entry via `notion-update-page`.
   - **IT Glue / Hudu or other doc platform:** these are search-only from here — output the entry as a paste-ready plain-text block and flag it for manual insertion.
   - No log found → ask before creating one; never invent a location.
4. Confirm with the requester before the first write to any given log location (Flows: only run against a pre-approved location — see below).
5. After logging, optionally record the trail on the ticket with `add_ticket_note` (plain text): "Change logged to <location> on <date>."

## Guardrails

- **Approved + completed or nothing.** Never log proposed, rejected, or in-flight changes; never convert a recommendation into a logged change.
- Field values come from the ticket verbatim; unknown fields are written as `not recorded`, never guessed. No credentials or secret values in the log — "credential rotated" without the value.
- Append-only: never edit or delete existing change-log entries; a correction is a new entry referencing the old one.
- Check for an existing entry for this ticket number before appending — re-runs must not duplicate.
- Notion tools are member-authenticated; without a Notion connection or writable location, output the paste-ready entry and stop. When in doubt about the destination, do nothing and ask.

## Unattended (Flows) variant

- Only run when the flow provides a **pre-approved log destination** for the client; no destination → output nothing and stop.
- Your entire reply is the change-log entry (or empty). No narration.
- Deterministic stops — do nothing when: approval evidence is missing; completion is unconfirmed; an entry for this ticket number already exists; the destination is unreachable.
