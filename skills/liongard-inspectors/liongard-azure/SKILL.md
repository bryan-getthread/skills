---
name: Liongard Azure Read
description: Answer Azure subscription questions from the Liongard Azure/Microsoft Cloud inspector — resource inventory, spend signals, NSG rule changes, public exposure, and unattached resources — without portal access.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_metric, liongard_detection, liongard_timeline, liongard_query]
connectors: [Liongard]
---

# Liongard Azure Read

**When to use:** "What does <client> run in Azure?" / resource census, "did an NSG rule change / is anything exposed to the internet?", unattached-disk and orphaned-public-IP spend sweeps, or Azure change context on an outage or security ticket.

## Prompt

```
Answer an Azure question for CLIENT_NAME from the Liongard Azure / Microsoft Cloud inspector's dataprint. Read-only — no resource, NSG, or role changes originate here.

1. liongard_launchpoint filtered by the Azure / Microsoft Cloud systemType. Partners name and scope these differently (per-subscription vs. per-tenant) — enumerate ALL matching launchpoints, verify each last ran successfully, and date the dataprints. State scope honestly: "no resources found" means "none in inspected scope."
2. Route the question, verifying every JMESPath field path against the live dataprint (layouts shift with inspector versions):
   - Inventory → liongard_metric over resource sections: VMs (size, OS, power state), storage accounts, app services, databases, VNets. Angle to verify: `VirtualMachines[].{name: Name, size: HardwareProfile.VmSize, state: PowerState}`.
   - Exposure → NSG rules allowing inbound from any-source on management ports (3389, 22), public IP assignments, storage accounts with public blob access. Exposure findings are security leads — flag and route remediation to a technician.
   - Spend signals → unattached managed disks, unassociated public IPs, stopped-but-allocated VMs, oversized VMs vs. observed role. Liongard captures configuration, not billing — present these as "cost-review candidates," never as dollar amounts.
   - Changes → liongard_detection / liongard_timeline for NSG modifications, new/deleted resources, role-assignment changes, with timestamps. On an outage ticket, lead with changes in the incident window.
   - Open-ended → liongard_query, verify actionable answers with targeted metrics.
3. Cross-check surprising inventory against search_tickets: unclaimed new compute in a client subscription is a possible compromise (cryptomining is the classic) and escalates to security review.
4. Exposure and unexplained-resource findings surface even when unasked, flagged as security leads. Output: answer, evidence table, per-launchpoint dataprint as-of timestamps, explicit scope ("subscription X only — no inspector on their second subscription" when true). Degradation: no Azure inspector → docs and ticket history only, labeled "not instrumented"; recommend adding the inspector when Azure questions recur.
```
