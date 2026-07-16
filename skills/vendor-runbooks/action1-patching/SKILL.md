---
name: Action1 Patching
description: An Action1 patch alert or report landed — separate patch failure from patch pending from reboot-pending, read compliance status across the estate, and hand the technician into the Action1 console for any deploy. Read/context only — no remote deployment.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_itglue, add_ticket_note, update_ticket]
---

# Action1 Patching

Vendor specialization of patch-management triage for Action1, the cloud patch-management platform. Action1 has no native Super Magic integration today, so this is a read/context-and-handoff skill: the agent interprets patch status and compliance from the alert/report, ticket history, and documentation, and directs the technician into the Action1 console to deploy. The agent cannot deploy patches, run scripts, or reboot devices. The core discipline is distinguishing the three states people conflate — failed vs pending vs reboot-pending — because each needs a different response. Verify status labels and features against Action1's current documentation.

## When to use

- An Action1 alert/report arrives: patch deployment failure, missing/pending patches, non-compliant endpoints, or a compliance summary
- A tech asks how to read Action1 patch status or which endpoints are actually at risk
- Patch-compliance context is needed for a security or audit conversation

## Steps

1. Parse the status precisely — the whole skill turns on this distinction:
   - Failed: the deployment ran and errored (download failure, install error code, dependency/reboot loop). Actionable now — read the error, don't just re-queue.
   - Pending / missing: the patch is approved but not yet installed (schedule not reached, device offline, maintenance window). Often normal; check the window before alarming.
   - Reboot-pending: installed but awaiting reboot to take effect — the endpoint is not yet protected but the fix is staged; a reboot (coordinated with the user) completes it.
   Copy Action1's exact status wording.
2. Route to the client and scope: is this one endpoint or an estate-wide compliance gap? Search ticket history (search_tickets, same device/KB, recent window) for recurrence — a patch that fails repeatedly across devices is a package/environment problem, not N separate tickets.
3. Prioritize by risk, not by count: a failed security-critical patch (actively-exploited CVE) on an exposed asset outranks a pile of pending optional updates. Cross-check severity against the client's exposure and any known-exploited advisories; don't treat all "non-compliant" equally.
4. Read failure causes from the error, and consult documentation (search_itglue) for the client's patch policy and maintenance windows; mark inferred meanings as inferred. If a Liongard inspector covers the estate, use it for corroborating OS/patch-level context (verify last run, state dataprint age; degrade if absent).
5. Hand off for deployment: approving, deploying, retrying, or rebooting happens in the Action1 console and is a technician action. Write the handoff with the specific endpoints, patches, and the reboot coordination needed; the agent directs and records.
6. Document the status breakdown (failed/pending/reboot-pending counts), risk prioritization, causes read, and the handoff. Client-facing wording per defensive-writing-standard.

## Guardrails

- The agent cannot deploy, approve, retry patches, or reboot devices — those are technician steps in the Action1 console the agent directs and records. Never imply the agent patched anything.
- Never conflate failed / pending / reboot-pending — each has a different meaning and response; a "pending" inside its window is not a failure, and a "reboot-pending" endpoint is not yet protected.
- Prioritize by exploited-risk and exposure, not by raw non-compliance count — don't bury a critical failure under optional-update noise.
- Recurrent failures across devices get one root-cause ticket (bad package/dependency/environment), not repeated re-queues.
- No native Action1 read integration: state that compliance figures are read from the report/docs and give their as-of time — don't present them as a live query.
- Degradation: without documentation, the client's patch policy and windows are unknown — say so before calling anything non-compliant.
