---
name: Shared Mailbox Creation
description: Create a new Exchange Online shared mailbox end-to-end — naming, license implications at the 50GB threshold, initial delegation, and the documentation note. Use when a ticket asks for a new shared mailbox, team inbox, or group email address that people need to work out of.
category: M365 Administration
tools: [search_tickets, search_contacts, search_clients, search_knowledge_base, search_itglue, add_ticket_note, send_approval, log_time_entry, web_search]
---

# Shared Mailbox Creation

Prepares a shared-mailbox creation the tech can execute in one pass: confirmed
name and address, the license question answered up front, the initial delegate
list approved, and a ticket note that makes the mailbox traceable forever.
The agent prepares and verifies; the tech drives the admin center or PowerShell.

## When to use

- "We need a shared mailbox for support@ / invoices@ / hr@<client-domain>."
- "Set up a team inbox the whole department can send from."
- Converting an ex-employee's mailbox into a shared mailbox (offboarding path).
- NOT for adding people to an existing shared mailbox — that is
  shared-mailbox-delegation.

## Steps

1. Pin down the request: display name, SMTP address, purpose, and the initial
   delegate list with the permission each person needs (Full Access vs Send As
   vs Send on Behalf — see shared-mailbox-delegation for the distinctions).
   Verify the address is not already in use as an alias, DL, or M365 group
   (ask the tech to check, or search_itglue for documented addresses).
2. Answer the license question before creation, not after:
   - A shared mailbox is free up to 50 GB with no license assigned.
   - Over 50 GB, or if it needs an online archive or a litigation hold, it
     requires an Exchange Online Plan 2 license (or Plan 1 + Archiving add-on).
   - If the expected volume is high (shared support queues grow fast), say so
     in the ticket and flag the future license cost to the client contact.
3. Get approval from the client's documented approver (send_approval) covering
   the address and the initial delegate list — a new sendable identity on the
   client's domain is user-visible and impersonation-adjacent.
4. Prepare the execution block for the tech (verify against current module
   versions):
   - `New-Mailbox -Shared -Name "<display name>" -PrimarySmtpAddress <address>`
   - Delegations per shared-mailbox-delegation (minimum permission per person).
   - Confirm sign-in stays blocked for the underlying account (shared mailboxes
     have a disabled account by default — it must stay that way; nobody logs
     in "as" the mailbox).
5. After the tech executes, verify via ticket evidence: a delegate confirms
   they can open the mailbox and, if granted, send from it; a test mail to the
   new address arrives. Allow up to an hour for permission propagation before
   calling it broken.
6. Post a plain-text note: mailbox name and address, purpose, delegates and
   exact permission types, approver, creation date, license status (unlicensed
   under 50GB / Plan 2 attached), and rollback (disable delegations, convert
   or remove mailbox). Log time (log_time_entry).

## Guardrails

- No creation without an approver on record — a new address people can send
  from is a phishing-surface change.
- Sign-in for the shared mailbox account stays blocked. If someone asks for
  "the password to the shared mailbox," redirect to delegation.
- Do not pre-grant Send As to everyone "to be safe"; grant per person, per
  need, per shared-mailbox-delegation.
- State the 50GB/license threshold in the ticket even when it doesn't apply
  yet; unbudgeted license surprises later land back on the desk.
- All PowerShell shown to the tech is labeled: verify against current module
  versions.
