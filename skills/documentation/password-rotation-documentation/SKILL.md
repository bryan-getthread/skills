---
name: Password Rotation Documentation
description: Document a credential rotation event cleanly — what was rotated, when, where, by whom, and why — recording secrets by vault reference only, so the rotation leaves an audit trail without exposing the new credential.
category: Documentation
tools: [search_tickets, search_clients, search_itglue, search_hudu, notion-search, notion-create-pages, add_ticket_note, create_ticket]
---

# Password Rotation Documentation

Captures the record of a credential rotation — the what/when/where/who/why — so the desk has an audit trail of the change without ever writing the new secret into a ticket, doc, or note. The new credential lives in the vault; this skill documents that it was rotated and where it now lives.

## When to use

- A credential (admin account, service account, shared login, Wi-Fi/PSK, appliance password) was just rotated and the event needs recording.
- Scheduled rotation of privileged credentials as a hygiene or compliance practice.
- Post-incident: a credential was rotated because it may have been exposed and the rotation must be evidenced.
- A departing technician's shared/known credentials are being rotated as part of offboarding (see documentation/offboarding-runbook-per-client).

## Steps

1. Confirm the client with `search_clients` and the trigger for the rotation (routine cadence, suspected exposure, offboarding, audit finding). If exposure-driven, link the source finding/ticket with `search_tickets`.
2. Establish the vault location: which vault holds the credential (IT Glue passwords, Hudu passwords, or a password manager outside this tool surface). Verify the vault entry exists / was updated via `search_itglue` / `search_hudu` or `notion-search` where the record is kept — **read metadata only, never the secret value**.
3. Build the **rotation record**, secrets by reference only:
   - **What** — the account/credential label and the system it unlocks (e.g. "firewall admin account", "M365 break-glass account"), by name — never the old or new value.
   - **When** — date/time of rotation.
   - **Where** — the system it was changed on, and the vault entry (by title) that now holds the new value.
   - **Who** — the technician who performed it and the approver if one was required.
   - **Why** — routine cadence / exposure remediation / offboarding / audit.
   - **Scope of impact** — dependent services, scheduled tasks, integrations, or saved connections that use this credential and were updated (or still need updating) so the rotation does not break them.
   - **Verification** — that the new credential was tested and works, and that old sessions/tokens were revoked where applicable.
4. Record the event: if the desk tracks rotations on a ticket, add it with `add_ticket_note` (or `create_ticket` for a standalone rotation-log ticket). If a rotation log is kept in Notion and the destination is confirmed, append the entry with `notion-create-pages`/the log page. Output the record as a paste-ready block regardless.
5. If any dependent service was NOT yet updated, list those as open follow-ups (with a ticket via `create_ticket` if requested) — a rotation that breaks an integration is not a clean close.

## Guardrails

- **Never write the credential — old or new — anywhere.** The record references the vault entry by title and the system by name only. Documenting a rotation must not re-expose the secret.
- This skill documents a rotation that a human performed and vaulted; it does not perform the rotation and does not read or verify the secret value itself.
- If a plaintext credential is discovered in a ticket during this work, treat it as exposed and trigger rotate-and-flag per documentation/credential-storage-audit — never quote it.
- Notes and tickets are plain text per the Note Format Standard (documentation/note-format-standard) — they may sync to a PSA, so they must be free of secrets.
- Docs tools are search-only; the record goes to Notion only when the member has connected it and confirmed the destination, otherwise paste-ready output.
- Degradation: without IT Glue/Hudu, confirm the vault location from the requester and record the entry reference they give; never substitute the value.
