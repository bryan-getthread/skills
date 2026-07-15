---
name: Ticket Triage
description: Classify a new or unassigned ticket, gauge severity, catch duplicates, and recommend where it should go.
category: Triage & Routing
tools: [search_tickets, list_boards, list_ticket_statuses, list_ticket_priorities, search_clients]
---

# Ticket Triage

**When to use:** A new or unassigned ticket needs a first read before it enters the queue.

**What you get:** A classified ticket with a proposed board, status, and priority, plus a one-line queue summary and any duplicate warnings.

## Steps

1. Read the full thread with `search_tickets` (include messages).
2. Classify the issue: access, hardware, software, network, email, or security.
3. Assess severity and urgency. Flag anything security-related or affecting many users as high priority.
4. Check for duplicate or related open tickets from the same contact or company.
5. Recommend the correct board, status, and priority, with a one-line summary for the queue.

## Guardrails

- Never change the ticket without confirming the proposed classification first.
- Treat multi-user or security issues as high priority by default.

## Consolidates

Morning triage, dispatch triage assist, P1 triage, and generic 'classify this ticket' skills.
