---
name: Intune Compliance Policies
description: Handle requests to create or change Intune device compliance policies — what will mark devices noncompliant, grace periods, and the Conditional Access blast radius — piloted before broad enforcement. Use for "require encryption/min OS on devices", "why is this device noncompliant", or any compliance-rule change request.
category: M365 Administration
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, send_approval, schedule_ticket, web_search]
---

# Intune Compliance Policies

A compliance policy is not a setting — it is a promise wired to Conditional
Access. Every rule you add is a new way for a device to get blocked from mail
and apps. This skill sizes that blast radius before the change, not after the
tickets arrive.

## When to use

- "Require BitLocker / minimum OS / a password on all client devices."
- "Why is <device> showing noncompliant?" / user blocked by "device not compliant."
- A security-baseline or insurance requirement that maps to compliance rules.
- Reviewing a client's compliance setup during onboarding or a posture review.

## Steps

1. **Understand what compliance drives here.** Check (docs first:
   search_itglue / search_hudu / search_knowledge_base) whether the client has
   a Conditional Access policy granting access only to compliant devices. If
   yes, every compliance change is an access-control change and inherits that
   change process. If no, noncompliance is reporting-only — say so plainly in
   the plan so severity is judged right.
2. **For a noncompliant-device ticket:** have the tech read the failed setting
   from the device's compliance detail in Intune — the console names the rule.
   Common non-obvious causes: the device simply hasn't checked in (stale
   devices drift to noncompliant), the tenant-wide "mark devices with no
   compliance policy assigned as" setting, or a rule the device can never meet
   (e.g., TPM requirement on a VM). Fix the device against the rule; genuine
   business exceptions route to the conditional-access-exception skill, not a
   quiet policy weakening.
3. **For a new or changed rule:** draft the policy plan in plain language:
   the exact settings, which platforms, which groups, and — the key number —
   how many currently-enrolled devices would fail it today. Have the tech pull
   the current state (e.g., encryption status, OS version report) so the plan
   says "this will mark ~N devices noncompliant" instead of discovering it live.
4. **Grace period is the rollout lever.** Configure actions for noncompliance
   deliberately: mark noncompliant immediately vs after N days, email to user
   with remediation instructions, and only then (if the client wants it)
   retire/block stages. Default to a grace window long enough for the fleet to
   remediate (commonly 7–30 days), with user-facing comms explaining what to do.
5. **Pilot, then broaden.** Assign to a pilot group first; watch the compliance
   report and helpdesk volume through at least one full check-in cycle before
   broad assignment. Schedule the broaden step (schedule_ticket) rather than
   leaving it to memory.
6. **Approval gate.** Before any assignment that can block users (compliance +
   CA in force, or block-stage actions), send_approval to the client's
   documented authority with: the rule, the device count that will fail today,
   the grace period, the user impact when it bites, and the rollback (unassign
   policy / revert setting).
7. **Document.** Plain-text note: policy name, settings changed, groups,
   predicted-fail count, grace period, approver, pilot results, rollback steps.

## Guardrails

- Never enable a block-capable compliance rule broadly in one step — pilot
  group and grace period are mandatory, not stylistic.
- Never "fix" a noncompliant device by weakening the policy for everyone; one
  device's problem gets a device fix or a scoped, approved exception.
- State result-cap honesty in plans: predicted-fail counts come from console
  reports at a point in time — label them as estimates with the pull date.
- The compliance/CA interaction is the whole risk: any plan must state
  explicitly whether noncompliance blocks access in this tenant.
- Agent prepares the plan, counts, and comms; the technician executes in the
  Intune console. Rollback steps are written before the change is applied.
