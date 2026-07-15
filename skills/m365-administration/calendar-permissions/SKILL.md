---
name: Calendar Permissions
description: Grant, change, or review calendar sharing and delegation with scope discipline — the minimum folder role that satisfies the ask, owner consent, and private-items handling stated. Use when a ticket asks for calendar access, an assistant managing a calendar, or "why can't I see their calendar."
category: M365 Administration
tools: [search_tickets, search_contacts, search_clients, search_knowledge_base, search_itglue, add_ticket_note, send_approval, log_time_entry, web_search]
---

# Calendar Permissions

Delivers calendar access at exactly the level requested — free/busy, details,
edit, or full delegate — with the calendar owner's consent and a note that
records who can see and do what.

## When to use

- "<user> needs to see <other user>'s calendar."
- "Make the assistant a delegate for the manager's calendar."
- "Everyone should see full details on the ops calendar."
- Reviewing or removing existing calendar grants.
- NOT for whole-mailbox access — that is shared-mailbox-delegation.

## Steps

1. Translate the ask into a folder role before proposing anything, and confirm
   the minimum that satisfies it:
   - AvailabilityOnly — free/busy times only.
   - LimitedDetails — free/busy plus subject and location.
   - Reviewer — read full details.
   - Editor — read, create, and modify items.
   - Delegate (Editor + meeting-request handling) — the assistant scenario;
     meeting requests and responses route to the delegate.
   "Can they see my calendar?" is almost always LimitedDetails or Reviewer,
   not Editor. Ask when ambiguous; do not default upward.
2. Consent: the calendar owner (or their manager per client policy) approves
   the grant — a calendar exposes travel, medical, and personnel meetings.
   Use send_approval or capture the owner's reply in the ticket.
3. Private items are a separate decision. By default a delegate does NOT see
   items marked private; the "delegate can see private items" flag is an
   explicit, owner-approved extra. Never bundle it silently.
4. Prepare execution for the tech (verify against current module versions):
   - Grant: `Add-MailboxFolderPermission -Identity "<owner>:\Calendar"
     -User <delegate> -AccessRights <Role>`
   - Change existing: `Set-MailboxFolderPermission` (Add fails if a grant
     already exists — check first with `Get-MailboxFolderPermission`).
   - Full delegate flows (meeting forwarding, private items) are cleanest via
     Outlook's Delegate Access UI by the owner or the tech with the owner.
   Note: non-English mailboxes may name the folder differently — resolve the
   folder name from the mailbox, don't hardcode "Calendar".
5. Default-calendar exposure check: if the ask is "everyone can see details,"
   that's the Default user's permission on the calendar — changing it affects
   the entire organization. Restate that scope to the approver explicitly
   before touching it.
6. Verify via evidence: the grantee opens the calendar and sees exactly the
   granted level (and cannot edit if Reviewer). Allow propagation time before
   retesting. Post a plain-text note: owner, grantee, exact role, private
   items yes/no, approver, date, expiry if temporary, and rollback
   (`Remove-MailboxFolderPermission`). Log time.

## Guardrails

- Minimum role always; Editor and delegate rights only when creating or
  managing items was explicitly requested and approved.
- Private-items visibility is never included implicitly.
- Changing the Default permission is an org-wide change — separate, explicit
  approval naming that scope.
- Temporary coverage (leave, project) gets an expiry and a tracked revert.
- Owner consent on record before any grant; when the owner is unavailable
  and the request is urgent, escalate per client policy rather than granting.
