---
name: Access Request Handling
description: Work a request for access to a folder, calendar, distribution list, or shared mailbox — approval checked, least privilege applied, expiry set on anything temporary. Use when a ticket asks to "give <user> access to" a resource.
category: Onboarding & Access
tools: [search_tickets, search_contacts, search_clients, search_knowledge_base, search_itglue, add_ticket_note, update_ticket, send_approval, log_time_entry]
---

# Access Request Handling

Turns a "please give <user> access to <resource>" ticket into an approved,
least-privilege, documented grant — with an expiry and revert plan whenever the
need is temporary.

## When to use

- "Give <user> access to the finance shared folder."
- "<user> needs to see the team calendar / the projects distribution list."
- "Add me to the support shared mailbox."
- Any grant-access ticket where the approver and access level are not yet pinned down.

## Steps

1. Identify precisely: who needs access (search_contacts), which resource
   (exact folder path, calendar, DL, or mailbox name), what level (read,
   contribute, full), and for how long. If the request says only "access,"
   ask — do not assume full control.
2. Check the client's access policy in search_knowledge_base / search_itglue:
   who owns or approves this resource? If an approver is defined, get their
   sign-off before granting (send_approval, or the client's documented
   channel). If the requester is the resource owner themselves, that counts.
3. Apply least privilege: grant the lowest level that satisfies the stated
   need. Reading files does not require contribute; viewing a calendar does
   not require editor.
4. If the need is temporary (project, covering leave, audit), set an explicit
   expiry date. Create the revert as a real follow-up: a note with the removal
   date and owner, or a scheduled follow-up ticket, so the grant does not
   become permanent by neglect.
5. Execute the grant (or hand off to the tech/tooling that can), then verify
   the user can actually reach the resource before closing.
6. Post a plain-text note: who was granted what level on which resource, who
   approved, why, and any expiry with its revert plan. Log time
   (log_time_entry) and confirm to the requester.

## Guardrails

- No grant without an identified approver or documented standing policy that
  covers it. "The user asked" is not approval.
- Least privilege is the default; escalate any request for full control of a
  resource the user does not own.
- Every temporary grant gets an expiry and a tracked revert — never grant
  "temporary" access with no removal plan.
- If the resource or its owner cannot be identified with confidence, do not
  grant; ask.
- Requests for security-group membership route to the Group Membership Request
  skill; mailbox delegation specifics route to Shared Mailbox Delegation.
