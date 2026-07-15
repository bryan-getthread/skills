---
name: Internal IT Onboarding
description: Onboard the MSP's own new hire — technician, dispatcher, or back-office — with accounts, PSA/RMM/docs tool licenses, role-scoped client access, and a shadowing schedule. Use when the new starter works FOR the MSP, not for a client.
category: MSP Business Operations
tools: [search_tickets, search_members, search_knowledge_base, search_itglue, search_hudu, create_ticket, update_ticket, add_ticket_note, send_approval, schedule_ticket, log_time_entry]
---

# Internal IT Onboarding

Runs the MSP's own new-hire setup as a tracked ticket: core accounts, the tool-stack seats the role actually needs, client-access scoped by role from day one, and a shadowing plan so the new hire learns the desk on real work. This is the internal twin of the client-facing new-hire-onboarding skill — same discipline, different tenant: the "client" is the MSP itself.

## When to use

- "New tech starts Monday — set up their onboarding ticket."
- "What does a new dispatcher need access to?"
- An HR/internal-ops ticket lands on the desk: "please provision <user>, starting <date>, role <role>."
- Building or refreshing the internal onboarding checklist itself.

## Steps

1. Confirm the essentials from the requester: name, start date, role and level (L1/L2/L3, dispatcher, back-office, sales/CSM), reporting manager, and location/remote. Do not proceed on a role you had to guess.
2. Pull the internal onboarding checklist if one exists (search_knowledge_base, search_itglue, search_hudu — look for "internal onboarding", "new employee", "staff setup"). If none exists, propose a role-based checklist and flag that it should be documented for next time.
3. Build the provisioning list in three tiers and post it as the ticket's work plan (add_ticket_note, plain text):
   - **Core accounts:** email/identity, MFA enrollment on day one, password manager vault membership, chat/collaboration, phone/softphone.
   - **Tool-stack seats by role:** PSA/Thread member account with role-appropriate permissions, RMM console access, documentation platform (IT Glue/Hudu) access, remote-access tooling, monitoring/alerting views. A back-office hire may need none of the technical consoles — do not default everyone to full stack; seats cost money and widen the audit surface.
   - **Client-access scoping:** which client environments, credential folders, and documentation the role may see. Default to least privilege: an L1 gets the clients on their assigned board, not the whole vault; escalation and admin credentials come later with tenure and manager sign-off. Record the scoping decision in the ticket so it is auditable.
4. Route anything permission-granting through approval (send_approval to the hiring manager): vault access tiers, admin-console roles, client credential folders. The manager approves scope; you track it.
5. Set up shadowing: identify 1–2 experienced members in the same role (search_members), propose a first-two-weeks shadowing plan (sit-ins on live tickets, then reverse-shadowing where the new hire drives and the mentor watches), and schedule the checkpoints (schedule_ticket / calendar entries via the requester). Cross-reference the new-hire-onboarding-coach and onboarding-guide-checkin skills in training-and-enablement for the ramp-up coaching that follows.
6. Track completion on the ticket: each provisioning item checked off with a note, MFA verified, first login confirmed. Close only when every item is done or explicitly deferred with an owner and a date.

## Guardrails

- Least privilege is the default, always. Broad client-credential access for a day-one hire requires explicit manager approval on the ticket — never grant it because "it's easier".
- You coordinate and track; you do not create identity accounts, assign vault permissions, or modify security groups yourself. Console work is done by an authorized admin — your job is the checklist, the approvals, and the audit trail.
- No credentials, temporary passwords, or MFA secrets in ticket notes, ever. "Credentials delivered via the password manager" is the correct note.
- Start-date pressure does not skip the approval step. If the manager is unreachable, provision core accounts only and hold the client-access tier.
- If the desk supports the MSP's own staff through a dedicated internal board, keep the ticket there; internal staff details do not belong on client boards.
- If IT Glue/Hudu are not connected for this tenant, work from the knowledge base and say the checklist source was limited.
