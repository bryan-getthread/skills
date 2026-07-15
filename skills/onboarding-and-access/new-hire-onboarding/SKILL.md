---
name: New Hire Onboarding
description: Run a new-employee onboarding request end to end — role-based accounts, licenses, groups, hardware, and MFA against the client's own checklist. Use when a ticket asks to set up, onboard, provision, or "create accounts for" a new starter.
category: Onboarding & Access
tools: [search_tickets, search_contacts, search_clients, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, send_approval, log_time_entry]
---

# New Hire Onboarding

Takes an onboarding request from intake to a tracked, role-appropriate provisioning
checklist: account, licenses, groups, hardware, and enforced MFA, with every pending
item owned and the requester told what to expect and when.

## When to use

- "New hire starting Monday at <client> — set them up like the rest of the sales team."
- "Onboard <user>, role <role>, needs laptop and M365."
- "Set up a new user with the same access as <user>."
- An onboarding ticket landed on the queue and needs the full checklist built and worked.

## Steps

1. Gather from the ticket (and the client's intake form, if one exists — see the
   Onboarding Form Intake skill): full name, start date, role/title, department,
   manager, location, and requested apps or groups. Use search_contacts and
   search_clients to anchor the client and manager records. List anything missing
   and ask the requester for it in one message, not piecemeal.
2. Check the start date. If it is within 48 hours, raise the ticket priority
   (update_ticket, using the board's real priority names from list_ticket_priorities)
   and flag it to the dispatcher in a note so it cannot sit in the queue.
3. Pull the client's onboarding SOP from search_knowledge_base / search_itglue /
   search_hudu. Build the checklist in three categories:
   - Required — every hire at this client gets these (account, mailbox, MFA, baseline groups).
   - Conditional — driven by role or department (role license bundle, department groups, line-of-business apps).
   - Optional — requester-specified extras that need explicit confirmation.
4. Map role to access. Use the client's documented role profile for licenses and
   security groups. If no profile exists, propose one from the closest documented
   role and have the requester confirm it — do not invent a default.
5. If the request is "mirror <user>" / "same access as <user>": ask exactly whose
   access to mirror if ambiguous, enumerate that user's groups, licenses, and
   delegations, then apply a least-privilege review — copy only what the new role
   needs. Never silently copy admin roles, elevated groups, or delegated mailbox
   access; list those separately and require explicit approval for each.
6. Confirm the full plan with the requester, including a timeline (account ready
   date, hardware ETA). For licenses that carry cost, get sign-off first
   (send_approval, or the client's documented approval channel).
7. Provision, or hand off provisioning. If Entra writes are available via Zapier,
   use the Entra User Lifecycle (Zapier) skill. If the client is hybrid on-prem AD,
   after account creation run an AD Connect delta sync and verify the object has
   synced before assigning cloud licenses or groups.
8. Credentials: enforce MFA enrollment and a password change at first sign-in.
   Deliver the temporary password only through the client's secure transfer method
   — never plain email, never pasted into the ticket.
9. Post a structured checklist note on the ticket (plain text, no markdown or
   emojis — it may sync to the PSA): each item under its Required / Conditional /
   Optional heading with status Done, Pending, or Blocked and an owner. Confirm
   completion and remaining timeline to the requester, and log time
   (log_time_entry).

## Guardrails

- Base access on the role profile and the client's licensed solutions, never a
  generic default. If confidence in the role mapping is low, ask — do not provision.
- Mirror-a-user requests always get a least-privilege review; elevated access is
  never copied without explicit approval.
- Confirm before creating accounts or assigning anything that carries license cost.
- Passwords travel only by secure transfer, with change forced at next sign-in.
- Do not invent SOP steps, group names, or license SKUs; if documentation is
  missing, say so in the note rather than guessing.
- If IT Glue / Hudu are not enabled for this tenant, fall back to
  search_knowledge_base and note that external documentation was not checked.

## Consolidates

New hire onboarding, new user onboarding planner, onboarding orchestrator,
role-based provisioning, and mirror-a-user setup skills.
