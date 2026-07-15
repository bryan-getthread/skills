---
name: Distribution List Management
description: Add or remove distribution-list members with owner approval and a documented reason. Use when a ticket asks to add someone to, remove someone from, or clean up a distribution list or email group.
category: Onboarding & Access
tools: [search_tickets, search_contacts, search_clients, search_knowledge_base, search_itglue, add_ticket_note, send_approval, log_time_entry]
---

# Distribution List Management

Handles DL membership changes so every add and remove has an approver and a
rationale on record — no more mystery members on the all-staff list.

## When to use

- "Add <user> to the sales distribution list."
- "Remove <user> from the announcements list — they moved teams."
- "Who approves changes to the leadership DL?"
- A DL membership change arrives inside a broader onboarding/offboarding ticket.

## Steps

1. Identify the exact list (clients often have similarly named DLs — confirm
   the address, not just the display name), the member to add or remove
   (search_contacts), and the stated reason.
2. Find the list owner or the client's documented approval rule
   (search_knowledge_base / search_itglue). Get owner approval before changing
   membership (send_approval or the client's channel). Exceptions: removals
   that are part of an authorized offboarding, and adds covered by a documented
   role profile (new sales hire → sales DL) — cite the covering policy instead.
3. Sanity-check the change: adding an external address to an internal list, or
   anyone to a broad-reach list (all-staff, leadership, clients-facing), gets
   flagged to the approver explicitly rather than buried in a routine request.
4. Execute the change. If the list is synced from on-prem AD, make the change
   on the AD side, run an AD Connect delta sync, and verify before reporting done.
5. Post a plain-text note: list address, member added/removed, approver, and
   the documented rationale. Log time (log_time_entry) and confirm to the
   requester.

## Guardrails

- No membership change without owner approval or a citable standing policy.
- External addresses and broad-reach lists always get an explicit approver
  callout — never treat those as routine.
- Confirm the exact list address before changing anything; a near-name match
  is not a match.
- Record the rationale in the note; "user requested" alone is not a rationale.
- Bulk cleanups (many removals at once) get a proposed list posted for approval
  first — never bulk-remove unattended.
