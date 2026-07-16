---
name: Inbox Link Backfill Note
description: On ticket create, compose the ticket's Thread conversation URL from the desk's documented link pattern and post it as an internal note, so PSA-side techs can jump straight into the Thread conversation. Answers the commonly requested "give my PSA techs a one-click link back to the Thread thread" partner workflow.
category: Documentation
tools: [search_tickets, add_ticket_note]
---

# Inbox Link Backfill Note

Techs who work primarily in the PSA can't easily get back to the live Thread conversation. This skill drops the ticket's Thread conversation URL into an internal note on create, so anyone reading the ticket in the PSA is one click from the real thread.

## When to use

- Techs live in the PSA and keep asking "where's the actual Thread conversation for this?"
- "Put a link back to the Thread inbox on every new ticket."
- Bridging a PSA-first team into Thread's live conversation view.

## Steps

1. Read the ticket with `search_tickets` and get its identifier(s) needed to build the link.
2. Compose the Thread conversation URL from the desk's **documented link pattern** (the workspace/base URL and the ticket-or-conversation path, configured alongside this skill). Fill in only the ticket's real identifier — do not guess a URL shape.
3. Post a plain-text internal note via `add_ticket_note`: `Thread conversation: <url>` with one short line of context if useful ("open in Thread to reply in the live thread").

## Guardrails

- **Never fabricate a link.** Build the URL only from the documented pattern and the ticket's real identifier. If the pattern isn't configured or the identifier is missing, post a note saying the link couldn't be composed rather than inventing one.
- Internal note only — this link is for staff; it is not client-facing.
- One backfill note per ticket: if a "Thread conversation:" note already exists, do not add another.
- The note is the only write. No status/owner/classification changes.
- Notes plain text — no markdown link syntax that a PSA sync would mangle; post the raw URL.

## Unattended (Flows) variant

- Fires on **ticket create** — a supported event.
- Your entire reply is the note, posted verbatim: `Thread conversation: <url>` — plain text, no narration. If the URL cannot be composed, output `NO LINK. Reason: <no pattern configured | no identifier>.`
- Deterministic stop: link pattern unconfigured or identifier missing → no fabricated URL.
- One note per ticket, ever.
