---
name: Post-Resolution Verification
description: N days after a ticket closed, verify the fix actually stuck — search for recurrence tickets from the same user/client/system, and where an RMM is enabled, check the affected device is still healthy.
category: QA & Closure
tools: [search_tickets, search_contacts, add_ticket_note]
---

# Post-Resolution Verification

Closure says the fix worked that day; this skill asks whether it still works. Run against tickets closed N days ago, it looks for the two signals that a resolution did not hold — a recurrence ticket, or a device that is unhealthy again — and reports which fixes stuck.

## When to use

- "Verify last week's closures held" / "did the fix on <ticket> stick?"
- A scheduled sweep of tickets closed exactly N days ago (default 7).
- Following up high-stakes resolutions (server issues, VIP users, repeat offenders) before the client has to reopen.
- Quality evidence for a service review: "97% of resolutions held at 7 days."

## Steps

1. Scope: one ticket, or all tickets closed N days ago (default 7) via `search_tickets`, split per board; disclose result caps.
2. For each ticket, extract the resolution fingerprint from the thread: the affected user (`search_contacts` to normalize), client, system/device named in the notes, and the issue type/symptom in plain terms.
3. **Recurrence check:** `search_tickets` for tickets opened after the closure date matching the fingerprint — same contact or same client plus the same symptom or system. Read each candidate enough to judge: RECURRENCE (same issue back), RELATED (same system, different symptom), or UNRELATED. Wording similarity alone is not a recurrence — require the same underlying issue.
4. **Device health check (when an RMM integration such as NinjaOne is enabled and the ticket names a managed device):** look up the device, its recent activities, and open alerts. Signals against the fix holding: the device offline, the same alert class re-firing since closure, or repeated reboots/service restarts on the component that was "fixed." Where no RMM is enabled or no device is identifiable, skip this check and say so — its absence is a coverage note, not a failure.
5. Verdict per ticket, with evidence cited:
   - **HELD** — no recurrence found, device healthy (or not applicable).
   - **DID NOT HOLD** — recurrence ticket found (cite it) or device unhealthy on the fixed component (cite the alert/activity).
   - **INCONCLUSIVE** — related-but-different activity, or checks not possible; state what is missing.
6. For DID NOT HOLD, recommend the action: link the recurrence ticket to the original, route the pair to Reopen Forensics or a root-cause pass, and on approval `add_ticket_note` (plain text) on the recurrence ticket referencing the prior fix and what was tried — so the next tech does not repeat it.
7. Output: verdict table (ticket, client, issue, verdict, evidence), hold rate for the batch, and any repeat offenders (same client + issue failing verification more than once).

## Guardrails

- Verdicts cite evidence or they are INCONCLUSIVE — never assume a fix held because nothing turned up in a capped or partial search; disclose result caps explicitly.
- Never mark DID NOT HOLD on wording similarity alone; the recurrence must plausibly be the same issue, and when it is arguable, say RELATED and let a human judge.
- Do not invent ticket numbers, device names, or alert details in notes or the report.
- Read-only except the optional cross-reference note, which requires approval and is plain text.
- Do not reopen the original closed ticket — a recurrence is a new ticket linked back, not a resurrection; recommend, never perform, closure-state surgery.
- RMM checks are read-only lookups; remediation is a deep-link for the tech, never an unattended device action from this skill.
