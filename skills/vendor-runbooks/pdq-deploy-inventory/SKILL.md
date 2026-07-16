---
name: PDQ Deploy & Inventory
description: A PDQ Deploy failure or PDQ Inventory finding landed — read the deployment error or inventory/collection gap, distinguish package problems from target problems, and hand the technician into PDQ for any action. Read/context only — no remote deployment.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_itglue, add_ticket_note, update_ticket]
---

# PDQ Deploy & Inventory

Vendor specialization of device/software-management triage for PDQ Deploy and PDQ Inventory (on-prem and PDQ Connect). No native Super Magic integration today, so this is a read/context-and-handoff skill: the agent interprets deployment failures and inventory findings from the alert/report, ticket history, and documentation, then directs the technician into PDQ to act. The agent cannot deploy packages, run steps, or push software. The central diagnostic is separating a *package* problem (the deployment itself is broken — bad installer, wrong switches, missing dependency) from a *target* problem (the endpoint is offline, unreachable, out of disk, or credential/permission blocked). Verify feature and error semantics against PDQ's current documentation.

## When to use

- A PDQ Deploy result arrives showing failures/errors across one or many targets
- A PDQ Inventory finding needs reading: stale collection, missing application, version drift, hardware/software report
- A tech asks whether a PDQ failure is the package or the machines, or wants an inventory/version read

## Steps

1. For a Deploy failure, read the error and classify package-vs-target:
   - Package problem — same error across many targets (non-zero installer exit code, wrong silent switches, missing prerequisite, wrong architecture). Fix the package/step once, not each target.
   - Target problem — error varies per machine or clusters on offline/unreachable/credential/disk conditions (target offline, access denied, insufficient space, service not running). Fix the endpoints or the schedule.
   Copy PDQ's exact step and error-code wording.
2. Scope from the result set and ticket history (search_tickets, same package/target, recent window): how many targets, which failed the same way, is this a first run or a recurring failure. A recurring package failure is one root-cause ticket.
3. For an Inventory finding, read the collection freshness first — a "missing" application on a stale collection may just be uncollected. State the last-scan time; distinguish genuine version drift / missing software from collection gaps. If a Liongard inspector covers the estate, use it to corroborate software/version state (verify last run, note dataprint age; degrade if absent).
4. Prioritize by risk where security-relevant: a failed deployment of a security patch or the absence of a required security agent outranks a cosmetic version drift — flag those and cross-reference the security runbooks.
5. Hand off for action: retrying a deployment, editing a package/step, adjusting credentials or targets, or forcing a scan happens in PDQ and is a technician action. Write the handoff with the specific package, the classified cause, and what to change; the agent directs and records.
6. Document the package-vs-target verdict (or the inventory read with its as-of time), scope, risk notes, and the handoff. Client-facing wording per defensive-writing-standard.

## Guardrails

- The agent cannot deploy, retry, edit packages, or push software — those are technician steps in PDQ the agent directs and records. Never imply the agent deployed anything.
- Always classify package-vs-target before recommending a fix — re-running a broken package against the same targets wastes a cycle; chasing targets when the package is wrong wastes several.
- Treat inventory "missing/absent" on a stale collection as unconfirmed — state the last-scan time; a stale scan is not evidence of absence.
- No native PDQ read integration: figures are read from the PDQ result/report or docs — give their as-of time, don't present them as a live query.
- Recurring package failures get one root-cause ticket, not per-target churn.
- Degradation: without documentation or a Liongard inspector, the client's standard package set and target groups may be unknown — say so.
