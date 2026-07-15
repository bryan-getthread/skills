---
name: Group Membership Request
description: Handle a security-group membership change — state what the group actually grants, get the right approver, set a review date. Use when a ticket asks to add or remove someone from a security group or role group.
category: Onboarding & Access
tools: [search_tickets, search_contacts, search_clients, search_knowledge_base, search_itglue, add_ticket_note, send_approval, log_time_entry]
---

# Group Membership Request

Processes security-group changes as access-control decisions, not list edits:
the note says in plain language what the group grants, who approved it, and
when it gets reviewed.

## When to use

- "Add <user> to the VPN users group."
- "<user> needs to be in the finance-apps security group."
- "Remove <user> from the admins group."
- A group add is requested with no explanation of why or what it unlocks.

## Steps

1. Identify the exact group and the user (search_contacts). Then determine what
   the group actually grants — check the group description, the client's
   documentation (search_knowledge_base / search_itglue), and what resources or
   policies reference it. If you cannot determine what a group grants, stop:
   that is a question for the client's admin, not a grant to make blind.
2. Classify sensitivity. Groups granting admin rights, security-tool access,
   financial systems, or conditional-access bypass are high-sensitivity: they
   require the client's documented high-privilege approver, not just a manager.
   Ordinary resource groups need the group owner or manager per policy.
3. Get approval before the change (send_approval or the client's channel),
   presenting the plain-language statement of what the group grants so the
   approver knows what they are signing.
4. Apply least privilege: if a narrower group satisfies the stated need,
   propose that instead.
5. Execute. For on-prem/synced groups, change in AD, run an AD Connect delta
   sync, and verify membership propagated before reporting done.
6. Set a review date: permanent grants get the client's standard access-review
   cycle noted; temporary grants get an expiry and a tracked revert (follow-up
   note or scheduled ticket).
7. Post a plain-text note: group name, what it grants (the plain-language
   statement — this is the point of the note), user, approver, rationale, and
   review/expiry date. Log time (log_time_entry).

## Guardrails

- Never add to a group whose effect you cannot state. "It fixes the error" is
  not a known effect.
- High-sensitivity groups require the documented high-privilege approver;
  manager approval is not sufficient for admin-grade groups.
- Every grant carries a review date or expiry in the note.
- Removals requested outside offboarding get confirmed with the group owner —
  removing someone can break their access mid-task.
- When in doubt about sensitivity or approver, escalate; do not grant.
