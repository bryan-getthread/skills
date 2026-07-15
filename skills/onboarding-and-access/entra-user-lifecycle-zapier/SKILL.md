---
name: Entra User Lifecycle (Zapier)
description: Create, update, or disable Microsoft Entra ID users through the Zapier connector — identity resolved from the PSA/ticket first (Entra has no search), approval gated before every write. Use when an onboarding/offboarding/update ticket should execute directly against Entra via Zapier.
category: Onboarding & Access
tools: [search_tickets, search_contacts, search_clients, search_knowledge_base, search_itglue, add_ticket_note, send_approval, log_time_entry]
---

# Entra User Lifecycle (Zapier)

Executes Entra ID account writes from the ticket via Zapier — with the one hard
constraint of this connector designed in: Zapier's Entra integration can write
users but cannot search them, so identity must be fully resolved on the
PSA/ticket side before any write is attempted.

## When to use

- An onboarding, offboarding, or user-update ticket where the desk executes
  Entra changes itself via the Zapier connector.
- "Create the Entra account for <user> per the onboarding ticket."
- "Disable <user> in Entra now — offboarding effective today."
- "Update <user>'s title/department/manager in Entra."

## Steps

1. Confirm the connector: the acting member must have the Zapier MCP connected
   with the Microsoft Entra ID app authorized for this client's tenant. If not
   available, this skill degrades to producing the exact change specification
   as a plain-text note for a tech to execute manually — never pretend the
   write happened.
2. Resolve identity from the PSA side FIRST. Zapier's Entra integration has
   Create/Update/Disable/Delete User and group-membership actions but NO user
   search: you cannot look a user up in Entra to check your target. Build the
   full identity from search_contacts, search_clients, the ticket, and client
   documentation (search_itglue / search_knowledge_base): exact UPN/email,
   display name, and for updates/disables the unambiguous existing user
   identifier. If the UPN cannot be established with certainty from these
   sources, stop and ask — a write against a guessed UPN can hit the wrong
   person or create a duplicate.
3. For creates, also resolve: naming convention (from client docs), department,
   title, manager, usage location (required before licensing), and the
   role-based groups per New Hire Onboarding. Check for collisions using PSA
   contact records and documentation, since Entra cannot be searched — state
   in the note that collision checking was PSA-side only.
4. Approval gate before EVERY write, no exceptions: post the exact intended
   change (action, target UPN, fields/values or groups) and get sign-off via
   send_approval or the client's documented channel. Batch-friendly: one
   approval may cover one ticket's coherent change set, but never carries over
   to a second user or a later ticket.
5. Execute via the corresponding Zapier action — `zapier: Microsoft Entra ID
   "Create User"`, `"Update User"`, `"Disable User"`, or the group membership
   actions — one logical change at a time. Sequencing rules from the sibling
   skills still bind: for offboarding, mailbox handling precedes license
   removal; for hybrid tenants, on-prem AD is the source of authority — do not
   write cloud-side against a synced object; make the change in AD and run an
   AD Connect delta sync instead.
6. Verify by effect, not by search (there is none): confirm through an
   observable outcome — the user appears in the licensing/billing view, sign-in
   behaves as expected, or a tech eyeballs the portal. Then post a plain-text
   note: action, target, approver, Zapier action used, verification method,
   and outcome. Log time (log_time_entry).

## Guardrails

- No Entra write without the approval gate — including "small" updates.
- Never guess a UPN. Unresolvable identity = no write.
- Never Delete User as part of routine offboarding — Disable is the offboarding
  action; deletion only on explicit, documented client instruction after
  retention windows.
- Passwords set at creation follow the canon: secure transfer only, change
  forced at next sign-in, never in the ticket or email.
- On any Zapier error or ambiguous result, do not retry blind: report the
  exact state as unknown and have it verified before a second attempt (a
  retried create can make duplicates).
- Do not write cloud-side attributes on objects mastered in on-prem AD.

## Unattended (Flows) variant

- Not eligible for unattended writes. If invoked from a Flow, produce only the
  proposed change specification as a plain-text note ending "PENDING APPROVAL —
  no changes made"; a human approves and triggers execution. When identity
  resolution fails, output nothing and stop.
