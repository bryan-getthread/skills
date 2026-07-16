---
name: New Ticket First Touch
description: One-pass first touch on a new ticket — classify it, check for duplicates, pull similar resolved tickets, and draft an acknowledgment for the tech to review and send.
category: Triage & Routing
tools: [search_tickets, search_knowledge_base, run_assistive_ai, add_ticket_note, update_ticket, list_ticket_priorities]
---

# New Ticket First Touch

Everything a good dispatcher does in the first five minutes, in one pass: what is this, have we seen it, how did we fix it last time, and a ready-to-review acknowledgment — so the tech starts warm instead of cold.

## When to use

- "First touch this ticket" / "give me the full first pass on this."
- A tech picks up a fresh ticket and wants classification, duplicate check, prior-resolution context, and a draft reply in one go.
- Intake wants a consistent first-touch package on every new ticket.

## Steps

1. Read the full ticket with `search_tickets`: all messages, requester, client, current fields.
2. Classify: issue category, proposed priority (using the desk's definitions and `list_ticket_priorities`; security/multi-user forces high), and Incident vs Request. Base it on the body, never the title alone.
3. Duplicate check: search open tickets for the same client on strong identifiers (contact, device/asset, error reference) within a recent window. Report exact matches as suspected duplicates with numbers; do not merge here.
4. Prior-resolution pull: search resolved tickets for the same or similar symptom (same client first, then any client) with `search_tickets`, and check `search_knowledge_base` for a matching article. Cite only real ticket numbers and articles found — never invent references.
5. Draft the acknowledgment with `run_assistive_ai` or directly: confirm receipt in plain language, restate the issue in one sentence, state the next step, no promised times unless desk policy sets one. Match the client's language if the ticket is non-English.
6. Post the package as one plain-text internal note via `add_ticket_note`: classification, duplicate findings, similar-resolution pointers, and the draft reply clearly marked DRAFT - FOR TECH REVIEW. Apply field changes with `update_ticket` only if the tech confirms.

## Guardrails

- The acknowledgment is a draft for review — never send it to the client from this skill, and never let it promise fixes, timelines, or causes that are not established.
- Recommendations vs. actions stay separate: classification and priority are proposals until confirmed (zero-assumption rule — nothing in the note may read as if a fix was applied).
- Duplicate findings here are advisory; merging belongs to the duplicate-hunter skill with its exact-match gate.
- Cite only tickets and KB articles that the searches actually returned; disclose result caps on the duplicate and similar-ticket searches.
- The internal note is plain text — no markdown, no emojis — for PSA compatibility.

## Unattended (Flows) variant

- Follows the Unattended Output Discipline contract: the entire reply is the plain-text internal note posted verbatim via `add_ticket_note` — classification proposal, duplicate findings, similar-resolution pointers, and the acknowledgment clearly marked DRAFT - FOR TECH REVIEW. No greeting, no narration, no markdown.
- Deterministic inputs from the flow: the triggering ticket id. One note per ticket, ever — if a first-touch note from this skill already exists, output nothing.
- Permitted writes: `add_ticket_note` only. `update_ticket` (classification/priority) stays attended — field changes need the tech's confirmation. Nothing is ever sent to the client.
- Sections that fail their confidence bar are marked, never guessed: classification uncertain → "CLASSIFICATION: UNCLEAR - needs tech review"; no genuine duplicates or priors found → "none found", never padded. Cite only tickets and KB articles the searches returned.
- Ticket unreadable or body empty → output nothing.
