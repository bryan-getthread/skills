---
name: Distribution vs M365 Groups
description: Choose the right group type for the ask — distribution list, M365 Group, mail-enabled security group, or dynamic — and handle DL-to-M365-Group upgrades with the blockers checked first. Use when a ticket asks for "a group email," "a team," or wants to modernize an old distribution list.
category: M365 Administration
tools: [search_tickets, search_contacts, search_clients, search_knowledge_base, search_itglue, add_ticket_note, send_approval, log_time_entry, web_search]
---

# Distribution vs M365 Groups

Maps a vague "we need a group" request onto the group type that actually fits,
and prevents the two classic mistakes: creating a full M365 Group (with a
SharePoint site and Planner) when the client wanted an email alias, and trying
to "upgrade" a DL that can't be upgraded.

## When to use

- "Create a group email for the sales team."
- "Should this be a distribution list or a Team?"
- "Upgrade our old distribution lists to Microsoft 365 Groups."
- "We need a group we can also use for permissions."
- NOT for adding/removing members of an existing list — that is
  distribution-list-management.

## Steps

1. Ask what the group is FOR, not what it should be called:
   - Just an address that fans out email → Distribution List. Lightest
     footprint, supports nesting, no extra workloads created.
   - Email plus a shared workspace (files, shared calendar, Teams, Planner)
     → M365 Group. Creating one provisions a mailbox, SharePoint site, and
     calendar — say that out loud; clients asking for "a group email" are
     often surprised by the workspace sprawl.
   - Granting permissions to resources AND receiving email → Mail-enabled
     security group.
   - Permissions only, no email → plain security group (Entra, not Exchange).
   - Membership by attribute (all users in department X) → dynamic
     membership, which requires Entra ID P1 licensing — check before
     promising it.
2. Ask the external-mail question for anything that receives email: should
   outside senders be able to mail it? DLs and M365 Groups both default to
   internal-only (`RequireSenderAuthenticationEnabled $true`); a support@ or
   sales@ that clients email needs that flipped deliberately — and that makes
   it spoofing/phishing surface worth noting.
3. If the group receives email people must WORK from (reply, triage, own),
   steer to a shared mailbox instead (shared-mailbox-creation) — a DL that
   fans out to five inboxes creates five uncoordinated copies; that's the
   root of "two people answered the same customer" tickets.
4. For DL → M365 Group upgrade requests, check the hard blockers before
   promising anything. A DL cannot be upgraded if it is: synced from
   on-premises AD, nested (contains groups or is a member of one), a
   mail-enabled security group, has send-on-behalf settings, is moderated,
   or is hidden from the GAL. Have the tech verify against the current
   eligibility list in Microsoft's docs — the list shifts. Ineligible DLs
   get recreated-and-cutover instead, which changes membership management
   and needs a comms plan.
5. Approval and naming: group creation is user-visible (GAL entry, possible
   Teams/SharePoint provisioning) — confirm name, address, owners (at least
   two), and privacy (public/private for M365 Groups) with the client
   (send_approval). Check the tenant's group-naming policy and creation
   restrictions before the tech hits create.
6. Prepare execution for the tech (verify against current module versions):
   `New-DistributionGroup` (add `-Type Security` for mail-enabled security),
   `New-UnifiedGroup` for M365 Groups, or the admin-center flow. For
   upgrades: the EAC upgrade action or `Upgrade-DistributionGroup`.
7. Verify via evidence: mail to the address fans out / lands; for M365
   Groups the workspace provisioned; owners can manage membership. Post a
   plain-text note: group type chosen and WHY (the decision from step 1),
   name/address, owners, external-mail setting, privacy, approver, date,
   and rollback (remove group; for upgrades note the DL is deleted by the
   upgrade and rollback means recreating it). Log time.

## Guardrails

- Never create an M365 Group when the stated need is only an email alias;
  the workspace sprawl is real cost and confusion.
- Two owners minimum on any group; single-owner groups orphan.
- External-mail enablement is an explicit, approved decision, never a
  default.
- DL upgrades: check eligibility blockers first and tell the client the DL
  is consumed by the upgrade; do not promise reversibility.
- Dynamic membership needs Entra ID P1 — verify licensing before designing
  around it.
