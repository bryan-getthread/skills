---
name: Flow Bulk Editor
description: List the desk's flows, present the target set, then bulk enable / disable / rename them via one confirmed pass — no editing each flow by hand. Answers the commonly requested "turn these flows off / rename this batch" workflow; runs manually on demand.
category: Automation & Flows
tools: [list_flows, get_flow, update_flow]
---

# Flow Bulk Editor

When you need to flip a batch of flows on or off (a maintenance window, a client offboarding, a cleanup) or rename a group to a new convention, doing it one at a time is tedious and easy to get wrong. Select the set, confirm once, apply to all.

## When to use

- "Disable all the flows for <client>" / "turn these back on"
- "Rename this batch of flows to the new naming scheme"
- "Bulk enable/disable the flows matching <pattern>"
- Prepping for a maintenance window or client change that touches many flows

## Steps

1. List all flows with list_flows. Identify the target set from the member's criteria (client, name pattern, current state) and get_flow for detail where needed to confirm each is the right one.
2. Present the target set as a table: flow name, current state (enabled/disabled), and the proposed change (enable / disable / rename → new name). This is the review gate — the member sees exactly which flows are affected before anything happens.
3. For renames, show current → proposed name per flow and check the new names don't collide.
4. Wait for explicit confirmation. Let the member drop individual flows from the batch first.
5. On confirm, apply the change flow by flow with update_flow, tracking each result.
6. Report a summary: which flows changed, which were skipped, and any failures. Never report the batch as done if one didn't take.

## Guardrails

- Confirm before writing. Bulk-toggling automation can silence real alerting or unleash it — the target-set review and an explicit yes are mandatory.
- Enumerate the exact affected flows before acting; never act on a vague "all the X flows" without showing the resolved list first.
- Flag high-impact toggles (disabling a flow that handles P1 routing or SLA escalation) so the member knows what goes quiet.
- Report partial success honestly.
- No fabrication — act only on flows list_flows/get_flow actually return; don't invent names or states.
- Member-invoked only; no unattended variant (a flow bulk-editing flows is not a supported trigger, and this should stay human-confirmed).
