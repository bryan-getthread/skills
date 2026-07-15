---
name: Multi-Issue Ticket Splitter
description: Detect when one ticket bundles two or more distinct problems and split it into cross-linked sibling tickets, one purpose each — purposes confirmed by the tech, never inferred silently.
category: Triage & Routing
tools: [search_tickets, create_ticket, update_ticket, add_ticket_note]
---

# Multi-Issue Ticket Splitter

Clients love the everything-email: "my laptop is slow, also we need a license for the new hire, and the conference room TV is broken." This skill proposes a split into one-ticket-per-problem siblings, gets the tech's sign-off on the list, then creates cross-linked tickets.

## When to use

- A ticket clearly describes two or more unrelated problems or requests.
- A tech says "split this ticket" or "there are three asks in here".
- Intake review flags bundled requests that will each need different owners or boards.

## Steps

1. Read the full ticket with `search_tickets`, including all messages — later replies often add or resolve asks.
2. Identify candidate distinct problems: different systems, different affected users, or different request types (incident vs purchase vs access). Two symptoms of one root cause are ONE problem — do not split those.
3. Present the proposed split to the tech: for each sibling, a one-line purpose, suggested board/type, and which original text supports it. Explicitly mark anything you are unsure belongs separate.
4. Wait for the tech to confirm, edit, or reject the purposes. The confirmed list — not your draft — defines the split.
5. Keep the original ticket as the anchor for the first (or primary) problem. For each additional confirmed problem, `create_ticket` under the same client and contact, titled with its confirmed purpose, and copy in only the relevant text.
6. Cross-link everything: post a plain-text note on each sibling listing all related ticket numbers and each one's purpose, and update the original's description/note to state what was split out and where.

## Guardrails

- Purposes are confirmed by the tech, never inferred and executed in one step. No confirmation → no split.
- Never split symptoms that plausibly share a root cause; when unsure, keep together and say why.
- The client's own words travel with each sibling — quote, do not paraphrase away detail.
- Every sibling must carry the cross-link note; an unlinked sibling is a lost ticket.
- Do not close or reassign the original as part of the split unless the tech asks.
- Notes are plain text; no invented ticket numbers — link only tickets you actually created.
