---
name: Mobile Fleet Review
description: Review a client's phones and tablets under management — enrollment state, OS version spread, compliance flags, and lost-device readiness (can we lock/wipe if one goes missing). Use for "review the mobile devices", "are the phones compliant", or preparing for a lost/stolen-device scenario.
category: Devices & Infrastructure
tools: [liongard_launchpoint, liongard_query, liongard_metric, search_ninjaone_devices, search_itglue, search_hudu, add_ticket_note, create_ticket, send_approval]
---

# Mobile Fleet Review

Posture review of a client's managed phones and tablets: who is enrolled, what OS versions are in the field, which devices are non-compliant or stale, and whether the desk could actually execute a remote lock/wipe if a device were reported lost today.

## When to use

- "Review <client>'s mobile fleet" / "are their phones and tablets compliant?"
- Before enforcing a new mobile policy (passcode, encryption, app protection).
- A lost or stolen phone ticket — establish what remote actions are even possible.
- An executive asks "what happens if someone loses a company phone?"

## Steps

1. Establish the MDM source of truth. Mobile devices rarely live in the RMM: pull enrollment data through Liongard when enabled — verify the Microsoft 365 / Intune inspector exists and last ran successfully (`liongard_launchpoint`) before trusting it, then `liongard_query` / `liongard_metric` for the enrollment data, stating the dataprint age; check `search_ninjaone_devices` only to catch stragglers with a mobile agent; otherwise ask the requester for an MDM export. State which source you used and its as-of time.
2. Enrollment state: bucket devices into enrolled-and-compliant, enrolled-but-noncompliant, enrolled-but-stale (no check-in for ~30+ days), and known-but-unenrolled (in docs or a user assignment list but absent from MDM). Stale devices are the silent risk — they will not receive a wipe command promptly.
3. OS version spread: tabulate iOS/iPadOS and Android versions. Flag versions no longer receiving vendor security updates — verify current support windows against vendor documentation rather than asserting from memory (Android support varies heavily by manufacturer).
4. Ownership model: note corporate-owned vs BYOD where the MDM or documentation distinguishes them. Wipe expectations differ — full wipe for corporate, account/container wipe for BYOD. Never assume BYOD devices can be fully wiped.
5. Lost-device readiness: for a sample of devices (or the specific device in a lost-phone ticket), verify the preconditions for remote lock/wipe — enrolled, recently checked in, supervised/corporate where full wipe is expected. Report readiness honestly: "a wipe command would reach N of M devices within an hour based on check-in recency."
6. Output: fleet summary table, non-compliance and staleness lists with recommended actions, and lost-device readiness verdict. Remote lock/wipe is not executable through these tools — hand off to the MDM console, and for an actual lost device recommend the desk get written client authorization first (`send_approval`). Offer `create_ticket` per remediation stream or a plain-text `add_ticket_note`.

## Guardrails

- Never present the fleet as complete unless it came from the MDM (via Liongard or export). Documentation-only device lists are candidates, not inventory.
- Wipe/lock/retire actions are never executed by this skill — they are MDM console actions requiring authorization, especially on BYOD where a full wipe may destroy personal data and create liability.
- For a lost/stolen device, confirm the reporter's identity through normal desk verification before recommending any destructive action; when in doubt, lock rather than wipe.
- If no MDM data source is available, say the mobile fleet cannot be verified — do not infer it from phone numbers in contact records.
- Notes posted to tickets are plain text — no markdown, no emojis.

## See also

- `intune-rmm-reconciliation` — the laptop/desktop equivalent of the enrollment-gap question.
