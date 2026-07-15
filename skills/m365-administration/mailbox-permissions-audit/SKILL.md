---
name: Mailbox Permissions Audit
description: Inventory who can access what across mailboxes — Full Access, Send As, Send on Behalf, and folder-level grants — and flag unexpected grants. Use when a ticket asks "who has access to this mailbox," a security review needs a delegation inventory, or offboarding needs a permission sweep.
category: M365 Administration
tools: [search_tickets, search_contacts, search_clients, search_itglue, add_ticket_note, update_ticket, log_time_entry, web_search]
---

# Mailbox Permissions Audit

Produces a complete, honest map of mailbox access for one mailbox or a whole
tenant: every grant type enumerated, each grant classified as expected or
unexpected, and the findings documented so someone can act on them.

## When to use

- "Who can read/send from <mailbox>?"
- Periodic client security review needing a delegation inventory.
- Offboarding sweep: what did the departing user have access to, and who has
  access to theirs?
- A user reports "someone read/sent my email" — inventory first, then hand
  proven-misuse questions to delegate-access-forensics.

## Steps

1. Scope with the requester: one mailbox, a department, or tenant-wide.
   Tenant-wide takes longer and produces a report, not a quick answer — say so.
2. Prepare the collection block for the tech (verify against current module
   versions). All three grant types must be collected — auditing only
   Full Access misses the impersonation-grade grants:
   - Full Access: `Get-MailboxPermission <mbx> | Where {$_.User -notlike
     "NT AUTHORITY\SELF"}`
   - Send As: `Get-RecipientPermission <mbx>`
   - Send on Behalf: `Get-Mailbox <mbx> | Select GrantSendOnBehalfTo`
   - If the ticket concerns calendar/folder access, add
     `Get-MailboxFolderPermission <mbx>:\Calendar` (see calendar-permissions).
   For tenant-wide, the same cmdlets piped over `Get-Mailbox -ResultSize
   Unlimited`, exported to CSV.
3. Classify every grant against documentation (search_itglue,
   search_knowledge_base, prior tickets via search_tickets). Expected =
   traceable to a ticket, a documented role, or a shared-mailbox design.
   Flag as unexpected:
   - Any grant with no ticket or documentation trail.
   - Full Access or Send As on a personal mailbox held by a peer (not an
     assistant/manager arrangement on record).
   - Grants held by disabled or departed accounts.
   - Send As granted where the documented need was read-only.
   - Broad grants (groups with FullAccess on individual mailboxes).
4. Do not remediate inside the audit. Unexpected grants get listed with a
   recommended action (revoke / confirm with owner / document); revocation is
   its own approved change via shared-mailbox-delegation, because "unexpected"
   sometimes means "undocumented but load-bearing."
5. If a flagged grant smells like compromise (grant appeared recently, grantee
   irrelevant to the mailbox, paired with forwarding), escalate to the
   security playbooks (compromised-account-containment) instead of treating it
   as hygiene.
6. Post a plain-text note: scope, collection date, counts per grant type, the
   full inventory (or CSV reference for tenant-wide), each unexpected grant
   with why it's flagged and the recommended action, and explicit statement of
   anything not covered (e.g., folder-level grants not swept). Log time.

## Guardrails

- Inventory, classify, recommend — never revoke within the audit itself.
  Every removal is a separate approved change with its own rollback.
- Report what was NOT checked as clearly as what was; a partial audit
  presented as complete is worse than no audit.
- Departed-user grants and Send As anomalies always get flagged, even when
  the requester only asked about Full Access.
- Neutral language in findings: "grant not traceable to documentation," not
  "so-and-so has been reading the CEO's mail."
- PowerShell labeled: verify against current module versions.
