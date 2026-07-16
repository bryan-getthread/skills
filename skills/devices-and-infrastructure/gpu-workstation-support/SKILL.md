---
name: GPU Workstation Support
description: Diagnose and support high-end GPU workstations (CAD, render, GIS, video) — driver channel choice, thermal/load behavior, and multi-monitor/display issues — through the RMM with a deep-link handoff. Use when a designer/engineer's workstation is slow, crashing in an app, or having display problems.
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, get_ninjaone_device_activities, list_ninjaone_alerts, get_ninjaone_device_link, search_itglue, search_knowledge_base, add_ticket_note]
---

# GPU Workstation Support

Support the workstations that professional apps punish — CAD/BIM, rendering, GIS, video — where the GPU, its driver channel, thermals, and multi-monitor setup are usually the story. Read the machine, isolate the likely cause, and hand the tech a deep link to act.

## When to use

- "<user>'s workstation crashes in <CAD/render/video app>."
- Display/multi-monitor problems on a high-end workstation.
- A render/CAD box is thermal-throttling, slow, or unstable under load.
- Standardizing driver channel across a design/engineering fleet — cross-reference the architecture-engineering industry pack.

## Steps

1. Resolve the workstation in the RMM: `search_ninjaone_devices` for the hostname within the client, then `get_ninjaone_device` for spec (GPU, RAM, CPU), OS build, disk, last contact, and pending reboot. Pull the app/driver standard for this fleet from documentation (`search_itglue`) and any KB (`search_knowledge_base`).
2. Establish the driver channel expectation. Professional GPU workflows usually want the vendor's Studio / Enterprise / certified driver (NVIDIA Studio/RTX Enterprise, AMD Radeon Pro) rather than the gaming/Game-Ready channel, because the ISV certifies against it — confirm against the client standard and the app vendor's certified-driver matrix rather than assuming latest-is-best. State the current driver vs. the recommended channel.
3. Look at load behavior and stability: `list_ninjaone_alerts` for GPU/thermal/memory/disk alerts, and `get_ninjaone_device_activities` for crash patterns, driver-timeout events, or repeated app faults. Note thermal or resource alerts that correlate with the reported crashes.
4. Multi-monitor/display angle: check for display-related errors and confirm the setup matches the GPU's supported output/resolution capability; mismatched refresh rates, mixed DisplayPort/HDMI, or docks are common culprits — capture the display topology from documentation where recorded.
5. Rule out the ordinary before blaming the GPU: disk pressure (flag under ~15% or under the app's scratch/cache need), pending reboot age, and RAM headroom for the workload. A render box out of scratch space looks like a "GPU problem" but isn't.
6. Output a findings summary: spec, current vs. recommended driver channel, thermal/load evidence, display topology note, and a ranked remediation proposal. Provide `get_ninjaone_device_link` for any hands-on step. Offer to post the summary to the ticket as a plain-text note (`add_ticket_note`).

## Guardrails

- Driver channel is a workload/certification decision — recommend the certified channel per the app vendor and client standard; verify against current vendor docs rather than defaulting to the newest driver.
- Role/spec inference from hostname is a hint, not a fact — confirm the actual GPU and spec from device details.
- This skill is read-only: it diagnoses and proposes. Driver installs, BIOS/thermal changes, and reboots are performed by the tech via the deep link, with the user's confirmation for anything that interrupts their session.
- If NinjaOne is not enabled, degrade to documentation and ticket history and say device state could not be read.
- Notes posted to tickets are plain text — no markdown, no emojis.
