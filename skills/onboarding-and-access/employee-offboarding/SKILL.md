---
name: Employee Offboarding
description: Securely disable a departing employee in the right order — sign-in and sessions first, mailbox before licenses — and reclaim access and assets. Use when a ticket asks to offboard, terminate, disable, or "remove access for" a user.
category: Onboarding & Access
tools: [search_tickets, search_contacts, search_clients, search_itglue, search_knowledge_base, add_ticket_note, update_ticket, send_approval, log_time_entry]
---

# Employee Offboarding

Runs a departure as a security event: access is cut in the correct order, data is
preserved before licenses disappear, every destructive step is confirmed, and the
ticket ends with a sign-off summary for HR or the manager.

## When to use

- "<user> is leaving Friday — disable everything on their last day."
- "Terminated effective immediately, cut access now."
- "Offboard <user>, manager needs their mailbox and files."
- An offboarding ticket needs the checklist built, sequenced, and worked.

## Steps

1. Confirm the target user (search_contacts), the client (search_clients), the
   effective date/time, and that the requester is authorized to order an
   offboarding for this client (manager, HR, or a documented authorized contact
   — not the departing user themselves). Pull the client's offboarding SOP from
   search_knowledge_base / search_itglue.
2. At the effective time, cut interactive access first, in this order:
   disable sign-in, revoke all active sessions and refresh tokens, remove or
   reset MFA methods, and reset the password. An enabled session outlives a
   password reset — session revocation is not optional.
3. Handle the mailbox BEFORE touching licenses: convert to shared or set
   forwarding per client policy, set the out-of-office, and grant the manager
   access if policy allows. Removing the license first can destroy the mailbox
   — never reverse this order.
4. Only then remove licenses, security groups, app assignments, distribution
   lists, and shared-mailbox delegations. Transfer ownership of anything the
   user owned (files, groups, DLs) to the manager or a named successor.
5. Hardware and devices: list assigned hardware for return, disable or retire
   the managed device enrollment, and record serial/asset details in the note.
   If a device is remote, note the return-logistics step as Pending with an owner.
6. Stamp the trail: write the ticket number into the disabled AD/Entra object's
   description field so any future admin can trace why the account is disabled.
   If hybrid, run an AD Connect delta sync and verify the disable propagated
   before reporting the account as secured.
7. Post a plain-text checklist note (no markdown or emojis — PSA sync) with each
   step Done / Pending / Blocked, then send a sign-off summary to HR or the
   manager covering: access cut time, mailbox disposition, data handoff,
   hardware status, and anything awaiting their decision. Log time
   (log_time_entry).

## Guardrails

- Confirm before every destructive action — disable, wipe, license removal,
  delegation removal. Never run the whole sequence unattended.
- Mailbox conversion always precedes license removal. If unsure whether the
  mailbox is preserved, stop and verify before removing the license.
- Do not act on an offboarding requested only by the departing user or an
  unverified sender; verify authorization through a contact on file.
- Follow the client's retention and data-handoff policy; if none is documented,
  ask rather than defaulting to deletion.
- Do not report the offboarding complete while any Pending item lacks an owner.

## Consolidates

Employee offboarding, offboarding checklist, termination access-cut, and
mailbox-conversion sequence skills.
