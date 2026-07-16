---
name: Teams Governance
description: Get Microsoft Teams sprawl under control — a naming policy, an expiration policy for dead teams, a guest-access review, and a plan the tech executes. Use when a client says "Teams is a mess," "too many teams/channels," "who are all these guests," or asks for a Teams cleanup or naming standard.
category: M365 Administration
tools: [search_tickets, search_clients, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, send_approval, log_time_entry, web_search]
---

# Teams Governance

Turns "Teams is chaos" into a scoped governance change: a naming convention,
an expiration/lifecycle policy for abandoned teams, and a guest-access review —
prepared by the agent, executed by the tech, every change reversible.

## When to use

- "We have hundreds of teams and nobody knows what's real."
- "Set a naming standard for new teams."
- "Clean up old/empty teams and channels."
- "Review who has guest access across Teams."
- NOT for creating a single team/channel (routine provisioning), and NOT for
  Teams Phone number/calling config — that is teams-phone-admin.

## Steps

1. Establish the current state before proposing policy. Because a team is
   backed by an M365 Group, governance here overlaps m365-group-lifecycle —
   coordinate so the two policies do not fight. Gather (via the tech or admin
   center export): team count, teams with no owner, teams with no recent
   activity, and guest count. If Liongard's M365 inspector is present, use it
   to read tenant group/guest state and state the dataprint age; otherwise
   note the numbers came from a point-in-time admin export.
2. Naming policy: propose a prefix/suffix convention (e.g. department or
   client code) plus a blocked-words list. Say plainly that a naming policy
   requires Entra ID P1 for every affected user and applies to ALL M365
   Groups tenant-wide, not just Teams — so it will reshape group names in
   Outlook and SharePoint too. Existing teams are not renamed automatically.
3. Expiration/lifecycle policy: propose a group-expiration period (owners get
   renewal prompts; unrenewed teams are soft-deleted and recoverable for 30
   days). This also needs Entra ID P1. Call out that a team with no owner
   cannot be renewed and will expire — so orphaned-team ownership must be
   fixed first (assign owners, minimum two) or those teams are the ones that
   silently die.
4. Guest-access review: list external guests, the teams they sit in, and
   (where visible) last sign-in. Flag guests from free-mail domains, guests
   with no recent activity, and any guest in a sensitive team. Recommend
   removals as a proposal for client sign-off — never auto-remove; a guest
   might be an active vendor. Tie into guest-access-audit / b2b-collaboration-setup
   for the deeper identity-side review.
5. Approval: naming and expiration policies are tenant-wide and user-visible
   (renewal emails, blocked names on creation). Get explicit client approval
   (send_approval) with the exact convention, expiration period, and guest
   removals listed. Never apply tenant-wide policy on a hunch.
6. Prepare execution for the tech (verify against current module/portal
   versions): Teams admin center + Entra admin center for naming/expiration
   policies; `Set-AzureADDirectorySetting` / Microsoft Graph group settings
   for the naming policy; guest removal via the Teams/Entra admin center.
7. Verify: new team creation is blocked when it violates the convention; the
   expiration policy shows applied with the agreed period; removed guests no
   longer appear. Post a plain-text note: policies set (convention,
   expiration period, licensing prerequisite), orphaned teams fixed, guests
   removed with reason, approver, date, and rollback (remove naming/expiration
   policy; soft-deleted teams recoverable within 30 days; re-invite a guest
   removed in error). Log time.

## Guardrails

- Naming and expiration policies need Entra ID P1 for all users and hit every
  M365 Group tenant-wide — verify licensing and scope before promising them.
- Never auto-delete teams or auto-remove guests; propose, get approval, then
  the tech executes. Expiration soft-deletes with a 30-day recovery window —
  say so.
- Fix orphaned ownership (two owners minimum) before turning on expiration, or
  ownerless teams expire silently.
- A team is an M365 Group — keep this policy consistent with
  m365-group-lifecycle rather than setting conflicting rules.
- Capture current policy state and the guest list before changing anything;
  that is the rollback evidence.
