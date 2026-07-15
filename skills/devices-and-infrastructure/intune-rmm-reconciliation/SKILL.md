---
name: Intune vs RMM Reconciliation
description: Reconcile a client's Intune-enrolled device list against the RMM agent inventory — find machines missing an RMM agent, machines missing Intune enrollment, and double-managed conflicts. Use when someone asks "are all Intune devices in the RMM", "why does this machine have no agent", or before an MDM/RMM policy rollout.
category: Devices & Infrastructure
tools: [search_ninjaone_devices, list_ninjaone_organizations, get_ninjaone_device, connectwise_rmm_search_devices, connectwise_rmm_list_companies, liongard_launchpoint, liongard_query, liongard_metric, search_itglue, search_hudu, add_ticket_note, create_ticket]
---

# Intune vs RMM Reconciliation

Cross-check the Intune enrollment list against the RMM agent inventory for one client and produce three actionable buckets: missing RMM agent, missing Intune enrollment, and double-managed conflict candidates.

## When to use

- "Do all of <client>'s Intune devices have the RMM agent?"
- A machine shows in Intune but is invisible to monitoring, or vice versa.
- Before rolling out a policy (patching, encryption, compliance) that assumes one tool covers the whole fleet.
- Periodic hygiene: "reconcile <client>'s Intune and RMM inventories."

## Steps

1. Resolve the client organization in the RMM (`list_ninjaone_organizations` or `connectwise_rmm_list_companies`) and pull its full device list (`search_ninjaone_devices` / `connectwise_rmm_search_devices`). If a listing may have hit a result cap, report the count as "at least N".
2. Get the Intune side. There is no direct Intune tool: pull Intune device data through Liongard when enabled — first verify the Microsoft 365 / Azure / Intune inspector exists and last ran successfully (`liongard_launchpoint`), then `liongard_query` / `liongard_metric` for the device list, stating the dataprint age alongside the count. If the inspector is absent or its last run failed, ask the requester to export the Intune device list (Devices → All devices → Export) and paste or attach it. Never guess the Intune population.
3. Normalize before matching: lowercase hostnames, strip domain suffixes, and match on serial number where both sides expose it — hostname alone misses renamed machines. Flag rows you matched on hostname only as lower confidence.
4. Bucket the results: (a) in Intune, not in RMM → missing agent (candidate for agent deployment); (b) in RMM, not in Intune → enrollment gap (candidate for MDM enrollment, or a server/appliance where Intune is not expected — separate those out); (c) in both → check for double-management conflicts: both tools trying to own patching, or both pushing conflicting policies. Note which tool is the intended source of truth per function if documentation (`search_itglue` / `search_hudu`) says.
5. Age-filter the noise: a device last seen by either side more than ~30 days ago is likely retired, not a gap. List stale devices separately as "verify then retire" rather than as action items.
6. Output a reconciliation table per bucket (device, last seen in each tool, match confidence, recommended action). Deploying agents or enrolling devices is not something this skill can do — hand off with the exact device list and, for RMM-side work, `get_ninjaone_device` context. Offer to open a remediation ticket (`create_ticket`) per bucket or post a plain-text summary note (`add_ticket_note`).

## Guardrails

- Never claim the Intune list is complete unless it came from Liongard or a user-supplied export; say explicitly which source you used and its as-of time.
- Serial-number matches are authoritative; hostname-only matches are stated as probable, never certain.
- Do not recommend removing either management agent from a double-managed device without the client's documented management model — flag the conflict, do not resolve it unilaterally.
- This skill is read-only. No agent installs, no enrollments, no policy pushes — the RMM MCP cannot run scripts or deploy software.
- If neither Liongard nor an export is available, stop after the RMM inventory and say the Intune side could not be verified — do not fabricate enrollment data.
- Notes posted to tickets are plain text — no markdown, no emojis.

## See also

- `rmm-cross-tool-reconciliation` — the generic pattern for reconciling any two device inventories (RMM vs RMM, RMM vs docs); this skill is the Intune-specific case with enrollment/conflict semantics.
