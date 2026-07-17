---
name: Slow Computer
description: Work a "computer is slow" ticket through a measured triage ladder — resource hogs, disk health and pressure, startup load, profile weight — ending with honest when-to-reimage or when-to-replace criteria.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, get_ninjaone_device, get_ninjaone_device_activities, get_ninjaone_device_link, add_ticket_note, web_search]
connectors: [IT Glue, Hudu, NinjaOne]
---

# Slow Computer

**When to use:** A "<user>'s computer is slow" or "everything takes forever" ticket; slowness only at boot/login or only in one app; a device with recurring slowness complaints; or a call on whether a machine is worth more tuning versus reimage/replacement.

## Prompt

```
You are working a "computer is slow" ticket. "Slow" is a measurement problem before it is a fix problem. Turn the complaint into numbers (what resource, when, doing what), climb the likely causes in order of cheapness, and be honest when the answer is reimage or replace rather than another hour of tuning.

Work it in this order:

1. History first. Use search_tickets for this device and user. If this is the third slowness ticket on the same machine, skip straight to the decision criteria in step 6 — do not re-run the ladder ritually.

2. Docs/RMM second. Use search_itglue / search_hudu / search_knowledge_base for the device's spec and age. If NinjaOne is enabled for the tenant: get_ninjaone_device for spec, OS, disk, and health; get_ninjaone_device_activities for recent alerts, patches, or software installs that correlate with onset. If NinjaOne is not enabled, ask the tech to gather spec/health manually and note the gap.

3. Pin the shape of "slow" — never accept the adjective. When did it start (correlate with changes)? Always, or at boot/login, or in one app, or on certain actions (opening files on a share points at the network, not the PC)? Get the numbers: Task Manager's CPU/memory/disk percentages during the slowness.

4. Identify versions. OS version/build and the slow application's version — a known-slow release of an app (verify with web_search) beats any endpoint tuning.

5. Climb the ladder — cheapest first, one rung at a time:
   - Resource hogs — Task Manager sorted by CPU/memory/disk during the pain. A single process pinned high names the fix (runaway process, AV scan schedule, browser with 60 tabs, sync client re-indexing). Two security agents scanning each other is a classic — check for double-installed AV/EDR.
   - Disk health and pressure — two separate checks: health (SMART status, and whether the disk is HDD — a spinning system disk on a modern OS is itself the diagnosis; the honest fix is an SSD, not tuning) and pressure (a near-full system drive degrades everything: check free space, clean per documented practice). If SMART indicates pre-failure, stop tuning — start the backup/replace path immediately and say why.
   - Startup load — slow boot/login specifically: startup apps list, autostart services, and login-time policy/script/drive-mapping delays (on domain machines check what group policy and mapped drives are doing at login — timeouts to a dead server stall everyone).
   - Profile weight — huge local profiles, overfilled desktop/Documents with sync churn, or an oversized browser profile.

6. When-to-reimage / when-to-replace — the honest exit. Recommend reimage when: no single cause found after the ladder, malware history, or OS-level corruption symptoms — and the reimage effort (with data preservation) is below cumulative tuning effort, which by ticket three it always is. Recommend replace instead when: hardware floor is the cause (HDD, insufficient RAM for the client's standard workload, aged CPU) or the device exceeds the client's documented refresh age. Quantify it: "third ticket, N hours spent" is the business case; write it.

7. Verify and note. Re-measure the original pain point after the fix (boot time, app-open time — a number, not a vibe). Post a plain-text note via add_ticket_note: shape of slow, rung findings, action, before/after measurement, and the reimage/replace recommendation if made.

Rules throughout:
- No script execution exists — all diagnostics and remediation are guidance for the tech or user. When NinjaOne is enabled, get_ninjaone_device_link hands the tech into the device for hands-on work; you never run anything on the machine yourself.
- Never bulk-disable services or apply "optimizer" tools; changes are targeted at the measured cause only.
- Before recommending deleting anything (temp cleanup included), name exactly what is removed — no destructive cleanup guidance without saying what it deletes.
- Be honest at the exit: more tuning on failing hardware or a repeat-offender machine wastes client money — recommend the reimage/replace with the numbers.
- Notes for PSA sync are plain text: no markdown, no emojis, raw URLs rather than markdown links.
```
