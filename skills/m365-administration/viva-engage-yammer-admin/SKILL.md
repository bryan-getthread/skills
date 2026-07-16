---
name: Viva Engage / Yammer Admin
description: Light administration of Viva Engage (formerly Yammer) — network configuration, community/usage policies, and basic moderation/access posture. Use when a client asks to set up or configure Viva Engage/Yammer, control who can post or create communities, or apply a usage policy to their internal social network.
category: M365 Administration
tools: [search_tickets, search_clients, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, send_approval, log_time_entry, web_search]
---

# Viva Engage / Yammer Admin

Handles the light-touch admin most clients actually need for Viva Engage (the
product formerly called Yammer): who's in the network, who can create
communities, and a basic usage/moderation posture. This is a low-frequency
skill — keep it proportional; most requests are a few settings, not a program.

## When to use

- "Set up / configure Viva Engage (Yammer) for us."
- "Control who can create communities / who can post."
- "Apply a usage policy to our internal social network."
- NOT for a full adoption/change-management program (that's an engagement),
  and NOT for Viva Connections/Insights/Learning (different Viva modules).

## Steps

1. Confirm scope and licensing: Viva Engage rides on Exchange/M365 and its
   communities are M365 Group-backed — so anything you do here overlaps
   m365-group-lifecycle. Confirm it's Viva Engage (not another Viva module)
   and that it's enabled for the tenant.
2. Network configuration: confirm the network is in Native Mode (M365-managed
   identity/security, the current standard) rather than legacy standalone
   Yammer — this affects how identity, compliance, and eDiscovery apply. Set
   the network's usage guidelines/terms-of-use text and the org's display
   basics.
3. Community creation & posting policy: decide whether any user can create
   communities or only approved people — because each community spins up an
   M365 Group, uncontrolled creation is group sprawl (defer the actual
   restriction mechanics to m365-group-lifecycle so it's one policy).
   Configure who can post to all-company/leadership feeds if the client wants
   broadcast controlled.
4. Moderation & access posture: set the usage policy (acceptable use,
   guidelines prompt on first post), keyword monitoring/reporting if the
   client wants it, and external-network/guest posture — default to no
   external participation unless the client explicitly asks and approves it.
5. Approval: usage policy, community-creation restrictions, and any external
   participation are org-visible and culture-relevant. Get client sign-off
   (send_approval) on the guidelines text, creation policy, and external
   posture before applying.
6. Prepare execution for the tech (verify against current portals): Viva
   Engage admin center for network settings, usage policy, and community
   controls; group-creation control via Entra (see m365-group-lifecycle).
7. Verify: settings show applied; community creation obeys the policy; the
   usage guidelines appear as intended. Post a plain-text note: network mode
   confirmed, usage policy set, community-creation/posting policy, external
   posture, approver, date, and rollback (restore prior settings / relax the
   creation policy). Log time.

## Guardrails

- Viva Engage communities are M365 Groups — keep creation/naming policy
  aligned with m365-group-lifecycle rather than setting a conflicting one.
- Confirm Native Mode vs. legacy standalone Yammer up front; it changes how
  compliance/eDiscovery apply.
- Default to no external/guest participation unless the client explicitly
  approves it.
- Keep this proportional — it's light admin, not a full adoption program;
  don't over-build.
- Usage policy and creation restrictions are org-visible — approve before
  applying, and capture prior settings as rollback.
