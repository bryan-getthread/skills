---
name: BSOD Analysis
description: Triage Windows blue screens from the stop code and faulting module — never from folklore — correlating with recent changes (patches, drivers, new hardware) to split driver vs hardware vs storage causes.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, get_ninjaone_device, get_ninjaone_device_activities, get_ninjaone_device_link, add_ticket_note, web_search]
---

# BSOD Analysis

A blue screen hands you its own diagnosis: the stop code and, usually, a faulting driver/module. This playbook insists on collecting those before any theory, then correlates with what changed on the machine to land in the driver, hardware, or storage branch.

## When to use

- "<user>'s PC blue-screened" — once, or repeatedly
- Random reboots that turn out to be crashes (machine restarts overnight)
- BSODs starting after a patch cycle, driver update, or new peripheral
- Multiple machines at one client crashing the same way

## Steps

1. **History first.** search_tickets for this device (recurring crash pattern → this is a repeat, read the prior work) and for the same stop code across the client. Multiple machines, same code, same window → shared cause (a patch, a deployed agent/driver) — treat as one incident, and web_search the code + the suspected update for a known issue before touching endpoints one by one.
2. **Get the code before theorizing — non-negotiable.** The exact stop code (e.g. the name and/or hex) and any named file/module from: the BSOD screen photo, Event Viewer (System log, BugCheck event), or the minidump. If the machine rebooted past it, guide the tech to the event log — never proceed on "it just blue-screened" alone.
3. **Correlate with recent change.** When NinjaOne is enabled: get_ninjaone_device_activities for patches, software installs, and driver updates in the days before onset; get_ninjaone_device for OS build and hardware model. Otherwise ask/check manually: Windows updates, new hardware/peripherals, new security agents, vendor driver updates. Onset-after-change is the strongest signal you will get.
4. **Decode the specific code.** web_search the exact stop code against Microsoft's documentation — codes have specific meanings (memory corruption, driver IRQL faults, storage/boot device, watchdog timeouts) and the named module narrows further (a vendor driver file names its owner). Do not decode from memory or apply generic "run sfc" folklore before the code is understood.
5. **Branch:**
   1. **Driver-implicated** (code/module names a third-party driver, or onset follows a driver update) — roll back or update that specific driver per the vendor's guidance (web_search the vendor + model + version). If the vendor's current driver is itself the defect, only the vendor can fix it: say so, and the interim is the last-known-good version. GPU, network, storage-controller, and security-agent drivers are the usual suspects.
   2. **Patch-correlated** — a Windows update ships in the correlation window: check for a documented known issue and its prescribed remediation (Microsoft's known-issue rollback or an out-of-band fix). Uninstalling the patch is an interim with a security cost — say so and flag for re-patching when fixed.
   3. **Memory/hardware-implicated** (memory-corruption codes, or crashes with varying codes — the classic bad-RAM signature) — guide a memory diagnostic pass and reseat/hardware inspection; pair with the hardware-diagnostics playbook. Varying stop codes across crashes = hardware until proven otherwise.
   4. **Storage/boot-implicated** (storage stack codes, inaccessible boot device) — check disk SMART health first (failing disk causes this) and storage driver changes; onset after imaging or storage-mode/firmware changes points at configuration, not the disk.
6. **Frequency discipline.** A single BSOD with no recurrence after evidence is collected: document the code and monitor — do not launch remediation on one event. Recurrence or any hardware signal → work the branch fully.
7. **Verify and note.** Stability over an agreed window (no BugCheck events for N days) is the verification, not a single clean boot. Plain-text note: stop code, module, change correlation, branch, action, monitoring window.

## Guardrails

- No stop code, no theory. Refuse to speculate until the code/module is in hand; the event log always has it.
- No script or remote execution — dump collection and remediation are guidance for the tech; use get_ninjaone_device_link for the hands-on handoff when NinjaOne is enabled.
- Do not recite stop-code meanings or vendor fixes from memory — verify with web_search against current documentation.
- If the defect is in a vendor's driver/firmware or a Microsoft patch, state plainly that the vendor owns the fix; offer only the documented interim.
- One crash ≠ a remediation campaign; gate action on recurrence or hard evidence.
- NinjaOne/docs tools exist only when enabled for the tenant — note what you could not check.
