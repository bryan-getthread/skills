---
name: Teams Phone Admin
description: Configure Microsoft Teams Phone for a client — assign/reassign phone numbers, apply calling and caller-ID policies, and stand up basic auto-attendants and call queues. Configuration only, not live call control. Use when a client asks to give a user a phone number, change calling permissions, or set up a main-line menu / hunt group in Teams.
category: M365 Administration
tools: [search_tickets, search_clients, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, send_approval, log_time_entry, web_search]
---

# Teams Phone Admin

Handles the administrative side of Teams Phone: number assignment, calling
policies, and the building blocks of an auto-attendant / call queue. This is
tenant configuration prepared for a tech to execute — it is NOT telephony
control (no dialing, live routing, or number porting through the agent).

## When to use

- "Give <user> a phone number / assign a DID."
- "This user shouldn't be able to make international calls."
- "Set up a main line that answers with a menu."
- "Create a support hunt group / call queue."
- NOT for porting numbers between carriers (a carrier/telephony process), and
  NOT for anything requiring live call handling.

## Steps

1. Confirm the licensing and number supply first — this is where Teams Phone
   requests stall. A user needs a Teams Phone license (and, for PSTN calling,
   either Microsoft Calling Plans with available number inventory, Operator
   Connect, or a Direct Routing SBC). Verify which model the tenant uses and
   whether spare numbers exist before promising a DID. If the tenant is on
   Direct Routing, number provisioning happens on the SBC/carrier side — say
   so; the agent configures the Teams-side assignment, not the carrier.
2. Number assignment: identify the user and the specific number. For a
   reassignment, confirm the number is genuinely free (a recycled number can
   still ring for old contacts — note that). Emergency calling matters:
   confirm the emergency location/address associated with the number is
   correct, because a wrong civic address is a real safety issue.
3. Calling policies: scope permissions to what the role needs — internal-only,
   domestic, or international — using a calling policy assignment, not a
   per-user hack. Set caller-ID policy if the client wants numbers masked or a
   main line presented. Voicemail and call-forwarding defaults belong here too.
4. Auto-attendant / call queue basics (configuration): map the client's stated
   menu ("press 1 for sales") to auto-attendant options, business hours, and
   holiday handling; map a hunt group to a call queue with an agent list and
   overflow/timeout behavior. Each auto-attendant/queue needs a resource
   account (often with a number assigned) — flag that dependency. Keep this to
   the standard config surface; complex multi-level IVR trees are a design
   engagement, not a quick change.
5. Approval: number assignment, calling-permission changes, and a main-line
   menu are client-visible and business-impacting (a wrong menu sends
   customers to the void). Get client sign-off (send_approval) on the number,
   the calling scope, and the attendant/queue flow before the tech applies it.
6. Prepare execution for the tech (verify against current Teams admin center /
   PowerShell): Teams admin center (Voice > Phone numbers, Calling policies,
   Auto attendants, Call queues) or `Set-CsPhoneNumberAssignment`,
   `Grant-CsTeamsCallingPolicy`, and the resource-account cmdlets.
7. Verify with evidence: a test call reaches the assigned user; blocked call
   types are actually blocked; the auto-attendant menu routes each option
   where intended and after-hours behavior fires. Post a plain-text note:
   number assigned/changed, calling policy applied, attendant/queue flow,
   emergency address confirmed, approver, date, and rollback (unassign number
   / restore prior calling policy / disable the attendant or queue). Log time.

## Guardrails

- Configuration only — no live call control, routing during calls, or carrier
  number porting through the agent.
- Verify Teams Phone licensing and number supply (and which PSTN model)
  BEFORE promising a DID; Direct Routing number provisioning is a carrier/SBC
  task outside this skill.
- Emergency-location/civic-address accuracy is a safety item — confirm it on
  every number assignment.
- Recycled numbers can still ring for old contacts; call that out on reassign.
- Auto-attendant/queue changes are customer-facing — approve the flow before
  applying, and capture the prior config as rollback.
