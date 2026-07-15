---
name: Alert Title Normalization
description: Classify a monitoring or security alert ticket and rewrite its title into the desk's standard format using a controlled vocabulary, posting an internal note with what changed.
category: Triage & Routing
tools: [search_tickets, update_ticket, add_ticket_note]
---

# Alert Title Normalization

Turn a raw tool-generated alert title into a standard, scannable one — `<AlertClass> - <client> - <device or user> - <symptom>` — so the queue reads consistently regardless of which tool raised the alert.

## When to use

- Alert tickets arrive with cryptic or verbose tool-generated subjects.
- A flow on the alert board should rewrite every new title to the house standard.
- "Normalize these alert titles" during a queue cleanup.

## Steps

1. Read the ticket with `search_tickets`: current title, description, and the raw alert body.
2. Classify the alert into one class from the controlled vocabulary configured for this desk (e.g. Backup Failure, Disk Space, Offline Device, AV/EDR Detection, Patch Failure, Certificate Expiry, Login/Identity, Other). Pick exactly one; use Other only when nothing else fits.
3. Extract the concrete subjects from the alert fields: client, device or user, and the specific symptom (error, threshold, service name). Take them from the alert body, never from guesswork.
4. Compose the new title in the standard format with the controlled-vocabulary class first. Keep it under ~90 characters; drop tool boilerplate, IDs already tracked elsewhere, and repeated timestamps.
5. Apply it with `update_ticket` and post a plain-text internal note via `add_ticket_note`: old title, new title, alert class chosen.

## Guardrails

- Never remove information that exists only in the title (unique error codes, hostnames) — move it into the note if it does not fit the new title.
- Use only classes from the controlled vocabulary; do not coin new ones mid-run.
- If the alert body does not identify a client or device, do not fabricate placeholders in the title — keep the original title and note why.
- Title and the internal note are the only writes. No status, priority, board, or owner changes.
- Notes are plain text — no markdown or emojis.

## Unattended (Flows) variant

- Your entire reply is the internal note, posted verbatim: `TITLE NORMALIZED. Old: <old>. New: <new>. Class: <class>.` or `TITLE UNCHANGED. Reason: <one line>.`
- One title rewrite per ticket, ever: if the current title already matches the standard format, stop without writing.
- When classification is uncertain between two classes, do nothing.
