---
name: Ticket Triage
description: Classify a new or unassigned ticket, gauge severity, catch duplicates, and recommend where it should go before it enters the queue.
category: Triage & Routing
tools: [search_tickets, list_boards, list_ticket_statuses, list_ticket_priorities, search_clients, update_ticket]
---

# Ticket Triage

First read of a new or unassigned ticket: classify it, force the right severity, catch duplicates, and propose board, status, and priority — then apply only after confirmation.

## When to use

- A new ticket just arrived and needs a first pass before dispatch.
- "Triage this ticket" / "classify this" / "where should this go?"
- Morning sweep of unassigned tickets on the intake board.
- A dispatcher wants a proposed board, status, and priority with a one-line queue summary.

## Steps

1. Read the full thread with `search_tickets`, including the earliest message and every reply. Do not classify or route from the title alone — titles are frequently stale, auto-generated, or wrong.
2. Classify the issue into your desk's categories (e.g. access, hardware, software, network, email, security). Base the call on the body of the request, not keywords in the subject.
3. Apply severity forcing rules before any judgment call: anything security-related (compromise, phishing, malware, unexpected MFA/login activity) or affecting multiple users or a whole site is high priority by default. Only the requester's own words de-escalating it, confirmed with the tech, can lower it.
4. Check for duplicates using strong identifiers, not wording similarity: same alert ID, same device/asset name, same monitoring reference, or same contact + same specific error within a recent window. Run a separate `search_tickets` per identifier; if any search may have hit a result cap, say so.
5. Look up the client with `search_clients` to confirm the company on the ticket matches the requester.
6. Propose: board, status, priority, category, and a one-line queue summary. List any suspected duplicates with ticket numbers and why they matched.
7. Only after the reviewer confirms the proposal, apply it with `update_ticket`. If they change anything, apply their version, not yours.

## Guardrails

- Never route or classify on the title alone; always read the thread first.
- Never change the ticket before the proposed classification is confirmed. This skill recommends; a person (or a separately configured unattended skill) applies.
- Security or multi-user impact forces high priority — do not let a calm tone in the message talk you down.
- Duplicate calls require a strong identifier match. Similar wording is a lead to mention, never grounds to merge or close.
- If board/status/priority names are ambiguous for this tenant, fetch them with `list_boards` / `list_ticket_statuses` / `list_ticket_priorities` rather than guessing labels.
- Do not invent ticket numbers, clients, or categories. If the ticket cannot be classified confidently, say so and leave it for a human.

## Consolidates

Morning triage, dispatch triage assist, P1 triage, duplicate-aware intake, and generic "classify this ticket" skills.
