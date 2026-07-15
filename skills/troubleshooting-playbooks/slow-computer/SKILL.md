---
name: Slow Computer
description: Work a "computer is slow" ticket through a measured triage ladder — resource hogs, disk health and pressure, startup load, profile weight — ending with honest when-to-reimage or when-to-replace criteria.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, get_ninjaone_device, get_ninjaone_device_activities, get_ninjaone_device_link, add_ticket_note, web_search]
---

# Slow Computer

"Slow" is a measurement problem before it is a fix problem. This ladder turns the complaint into numbers (what resource, when, doing what), climbs the likely causes in order of cheapness, and is honest when the answer is reimage or replace rather than another hour of tuning.

## When to use

- "<user>'s computer is slow" / "everything takes forever"
- Slow only at boot/login, or only in one application
- Recurring slowness complaints on the same device
- Deciding whether a machine is worth more tuning vs reimage/replacement

## Steps

1. **History first.** search_tickets for this device and user. Third slowness ticket on the same machine → skip to step 6's decision criteria; do not re-run the ladder ritually.
2. **Docs/RMM second.** search_itglue / search_hudu / search_knowledge_base for the device's spec and age. When NinjaOne is enabled: get_ninjaone_device for spec, OS, disk, and health; get_ninjaone_device_activities for recent alerts, patches, or software installs that correlate with onset.
3. **Pin the shape of "slow" — never accept the adjective.** When did it start (correlate with changes)? Always, or at boot/login, or in one app, or on certain actions (opening files on a share → network, not the PC)? Get the numbers: Task Manager's CPU/memory/disk percentages during the slowness.
4. **Identify versions.** OS version/build and the slow application's version — a known-slow release of an app (verify with web_search) beats any endpoint tuning.
5. **Climb the ladder — cheapest first, one rung at a time:**
   1. **Resource hogs** — Task Manager sorted by CPU/memory/disk during the pain. A single process pinned high names the fix (runaway process, AV scan schedule, browser with 60 tabs, sync client re-indexing). Two security agents scanning each other is a classic — check for double-installed AV/EDR.
   2. **Disk health and pressure** — two separate checks: health (SMART status, and whether the disk is HDD — a spinning system disk on a modern OS is itself the diagnosis; the honest fix is an SSD, not tuning) and pressure (near-full system drive degrades everything: check free space, clean per documented practice). Escalate when: SMART indicates pre-failure — stop tuning, start the backup/replace path immediately and say why.
   3. **Startup load** — slow boot/login specifically: startup apps list, autostart services, and login-time policy/script/drive-mapping delays (domain machines: check what group policy and mapped drives are doing at login — timeouts to a dead server stall everyone).
   4. **Profile weight** — huge local profiles, overfilled desktop/Documents with sync churn, or an oversized browser profile. Pair with the profile-corruption playbook when the profile is also error-prone.
6. **When-to-reimage / when-to-replace — the honest exit.** Recommend reimage when: no single cause found after the ladder, malware history, or OS-level corruption symptoms — and the reimage effort (with data preservation) is below cumulative tuning effort, which by ticket three it always is. Recommend replace instead when: hardware floor is the cause (HDD, insufficient RAM for the client's standard workload, aged CPU) or the device exceeds the client's documented refresh age. Quantify: "third ticket, N hours spent" is the business case; write it.
7. **Verify and note.** Re-measure the original pain point after the fix (boot time, app-open time — a number, not a vibe). Plain-text note: shape of slow, rung findings, action, before/after measurement, reimage/replace recommendation if made.

## Guardrails

- No script execution exists — all diagnostics/remediation are guidance for the tech or user; when NinjaOne is enabled, get_ninjaone_device_link hands the tech into the device for hands-on work.
- Never bulk-disable services or apply "optimizer" tools; changes are targeted at the measured cause only.
- Deleting anything (temp cleanup included) checks first what it is — no destructive cleanup guidance without naming exactly what is removed.
- Be honest at the exit: more tuning on failing hardware or a repeat-offender machine wastes client money — recommend the reimage/replace with the numbers.
- NinjaOne tools exist only when that integration is enabled for the tenant; otherwise the tech gathers spec/health manually — note the gap.
- Notes for PSA sync: plain text, no markdown or emojis.
