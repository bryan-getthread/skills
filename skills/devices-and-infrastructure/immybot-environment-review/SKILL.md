---
name: ImmyBot Environment Review
description: Review an ImmyBot tenant's computers and maintenance-session history — what ran, what failed, and which machines keep failing — read-only. Use for "did last night's ImmyBot maintenance succeed", "why didn't <software> deploy to <device>", or an ImmyBot coverage check.
category: Devices & Infrastructure
tools: [list_immybot_tenants, search_immybot_computers, get_immybot_computer, list_immybot_maintenance_sessions, add_ticket_note]
---

# ImmyBot Environment Review

Reads the ImmyBot side of a client's world: which computers are enrolled, what maintenance sessions ran, what failed and why — so deployment questions get answered from session history instead of guesswork.

## When to use

- "Did the ImmyBot maintenance run for <client> last night? Anything fail?"
- "Why didn't <application> install on <device>?" when ImmyBot handles deployments.
- Checking which of a client's machines are enrolled in ImmyBot at all.
- A cross-tool reconciliation flagged ImmyBot coverage gaps and someone wants the detail.

## Steps

1. Resolve the client to an ImmyBot tenant via `list_immybot_tenants`. Rank name matches and state your pick; only ask when genuinely ambiguous.
2. Scope the question: whole-tenant review, one computer, or one maintenance window.
3. Tenant review: `search_immybot_computers` for the enrollment list (flag result caps — report floors). Then `list_immybot_maintenance_sessions` over the window (default: last 7 days) and bucket sessions: succeeded / failed / partial / not-run. Group failures by what failed (a specific software's detection or install step, a pending-reboot precondition, machine offline at session time) and by machine — a machine failing every session is a different problem than a package failing on every machine.
4. Single computer: `get_immybot_computer` for its identity and state, then its sessions from `list_immybot_maintenance_sessions` — what ran, what failed, and the failure detail per step. Correlate "offline at session time" failures with the RMM view if a health check has been run.
5. Read the failure texture before concluding: installer exit codes and detection-script failures point at the package; timeout/unreachable points at the machine or network; a pending reboot blocking everything points at basic hygiene. Report what the session record actually says, and quote the failing step's status rather than paraphrasing into certainty.
6. Output: coverage count, session outcomes over the window, failure clusters (by package and by machine) with the evidence, and recommended next actions — retry after reboot, fix the package, enroll the missing machines. Offer a plain-text summary via `add_ticket_note`.

## Guardrails

- Read-only today: this integration cannot start, retry, or modify maintenance sessions, and cannot deploy software. Every "retry" or "fix" in the output is a recommendation for a tech to action in ImmyBot itself — never claim a remediation was performed.
- Do not infer success from absence: a machine with no session in the window "did not run", which is not "passed".
- Failure causes come from the session record; where the record is ambiguous, say so instead of picking a cause.
- Result-cap honesty on computer and session listings.
- If ImmyBot is not enabled for the tenant, say so and stop — do not substitute RMM data as if it were deployment history.
- Plain-text notes only.
