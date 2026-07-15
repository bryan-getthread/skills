---
name: Entra PIM Requests
description: Handle privileged-role requests through Privileged Identity Management — eligible vs active assignments, activation justification, and time-boxing — instead of handing out standing admin. Use when a ticket asks to "make <user> an admin", grant a directory role, or activate/extend a PIM assignment.
category: M365 Administration
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, send_approval, schedule_ticket, web_search]
---

# Entra PIM Requests

The correct answer to "make me an admin" is almost never a permanent role
assignment. This skill converts admin requests into the least role, the
shortest window, and — where the tenant has PIM — an eligible assignment the
user activates with justification.

## When to use

- "Make <user> a Global Admin / Exchange Admin / <role>."
- "<user> needs to activate their PIM role" or activation is failing.
- "Extend <user>'s admin access" / a temporary project needs elevated rights.
- Converting standing admins to eligible assignments after a global-admin-audit.

## Steps

1. **Interrogate the need, not the role name.** What task, on what, for how
   long? Map the task to the least directory role that does it (e.g., password
   resets → Helpdesk Administrator or Password Administrator, not User
   Administrator; Exchange work → Exchange Administrator, not Global Admin).
   Look up current role capabilities (web_search / Microsoft docs) rather than
   assuming — role definitions change. Global Administrator requests get
   special scrutiny: name the specific action that lesser roles cannot do; if
   none, propose the lesser role.
2. **Check licensing reality.** PIM requires Entra ID P2 (or Governance) for
   the users involved. If the tenant lacks it, the fallback pattern is a
   time-boxed direct assignment with a scheduled removal ticket — weaker, but
   honest; say which pattern applies in the plan.
3. **Eligible over active, active over permanent.** Default proposal:
   eligible assignment, so the user activates only when working. Active
   time-bound assignments only for roles used constantly during a defined
   period. Permanent active assignments are exceptions requiring explicit
   client sign-off and a named reason (break-glass accounts are the classic
   legitimate case — see break-glass-account-audit).
4. **Set the activation rules deliberately** (or record the existing ones):
   maximum activation duration (default to hours, not days), MFA on
   activation, justification required, and approval required for high roles
   (Global Admin activations should require an approver). Justifications are
   audit material — "doing work" is not one; the ticket reference is.
5. **Approval gate.** Privileged access is a security-posture change:
   send_approval to the client's documented security/IT authority with the
   role, assignment type (eligible/active), duration, and why the lesser
   alternatives were rejected. The requester's manager approving their own
   team's admin access does not clear the bar unless the client's policy says
   so.
6. **Time-box and track the revert.** Every assignment gets an end date. For
   direct (non-PIM) assignments, create the scheduled removal
   (schedule_ticket) before the assignment is applied — the revert exists
   first. For eligible assignments, schedule the periodic re-justification
   review per the client's cadence.
7. **Document.** Plain-text note: user, role, assignment type, duration,
   activation settings, approver, justification, and the removal/review
   reference. For activation-failure tickets: the usual causes are missing
   P2 license, MFA/CA blocking the activation flow, or an approval sitting
   unactioned — check in that order.

## Guardrails

- Never grant Global Administrator when a scoped role does the task; the note
  must record why alternatives were rejected if GA is granted anyway.
- No permanent standing assignment without explicit approval naming it as
  permanent — "temporary" access with no end date is permanent access.
- The revert (scheduled removal or review) is created before the grant is
  applied, not after.
- Do not approve-and-execute in one breath: the agent prepares the proposal
  and approval; a technician executes the assignment in Entra after approval
  lands.
- Elevation requests arriving mid-incident from an unverified requester get
  identity verification first (callback to a number on file) — urgency plus
  admin rights is the social-engineering signature.
