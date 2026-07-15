---
name: Liongard Azure Read
description: Answer Azure subscription questions from the Liongard Azure/Microsoft Cloud inspector — resource inventory, spend signals, NSG rule changes, public exposure, and unattached resources — without portal access.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_metric, liongard_detection, liongard_timeline, liongard_query, search_tickets, add_ticket_note]
---

# Liongard Azure Read

Interrogate a client's Azure footprint through Liongard's Azure inspector dataprint: what resources exist, what is exposed, what changed, and what is quietly burning money. Most "what do they have in Azure" questions during triage, quoting, or QBR prep need a read, not a portal session.

## When to use

- "What does <client> run in Azure?" / resource census for scoping or onboarding
- "Did an NSG rule change?" / "is anything exposed to the internet?"
- "Any unattached disks or orphaned public IPs at <client>?" — spend-leak sweep
- Azure change context on an outage or security ticket ("what changed in their subscription this week?")

## Steps

1. Base pattern (skills/liongard-inspectors/liongard-access-pattern/SKILL.md): liongard_launchpoint filtered by the Azure / Microsoft Cloud systemType for the client. Note that partners name and scope these inspectors differently (per-subscription vs. per-tenant) — enumerate all matching launchpoints, verify each last ran successfully, and date the dataprints.
2. Route the question:
   - Inventory → liongard_metric over the resource sections: VMs (size, OS, power state), storage accounts, app services, databases, VNets. Angle shape: `VirtualMachines[].{name: Name, size: HardwareProfile.VmSize, state: PowerState}` — verify field paths against the live dataprint; Azure dataprint layouts shift with inspector versions.
   - Exposure → NSG rules allowing inbound from any-source on management ports (3389, 22), public IP assignments, storage accounts with public blob access where captured. Exposure findings are security leads: flag them and route remediation to a technician, referencing devices-and-infrastructure/firewall-rule-change discipline for the change itself.
   - Spend signals → unattached managed disks, unassociated public IPs, stopped-but-allocated VMs, oversized VMs vs. observed role. Liongard captures configuration, not billing — present these as "cost-review candidates," never as dollar amounts; actual spend belongs to the client's cost tooling.
   - Changes → liongard_detection / liongard_timeline for NSG modifications, new resources, deleted resources, role assignment changes, with timestamps. On an outage ticket, lead with changes in the incident window.
   - Open-ended → liongard_query, verify actionable answers with targeted metrics.
3. Cross-check surprising inventory against tickets: a VM nobody recognizes gets the same treatment as an unexplained admin — search_tickets for the provisioning change; unclaimed new compute in a client subscription is a possible compromise (cryptomining is the classic) and escalates to security/security-alert-response.
4. Output: answer, evidence table, per-launchpoint dataprint as-of timestamps, and explicit scope ("subscription X only — no inspector on their second subscription" when true).

## Guardrails

- State scope honestly: Liongard sees only the subscriptions/tenants it inspects; "no resources found" means "none in inspected scope."
- Never quote costs from configuration data — flag candidates, let billing tooling price them.
- Exposure and unexplained-resource findings surface even when unasked, flagged as security leads.
- Read-only: no resource, NSG, or role changes originate here.
- Degradation: no Azure inspector → docs and ticket history only, labeled "not instrumented"; recommend adding the inspector when Azure questions recur for the client.
