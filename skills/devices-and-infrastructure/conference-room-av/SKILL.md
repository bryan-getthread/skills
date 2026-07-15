---
name: Conference Room AV
description: Keep meeting rooms working — room-system (Teams Rooms/Zoom Rooms) health, calendar/resource-mailbox integration checks, and the pre-meeting checklist for high-stakes meetings. Use for "the boardroom screen isn't working", recurring room complaints, or "make sure the room works before the board meeting".
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, list_ninjaone_alerts, get_ninjaone_device_activities, get_ninjaone_device_link, search_itglue, search_hudu, search_tickets, add_ticket_note, create_ticket, schedule_ticket]
---

# Conference Room AV

Treat each meeting room as a small system — room device, display, camera/mic, calendar identity, network — and check it as a system: health when it breaks, integration when bookings misbehave, and a proactive checklist when a meeting is too important to fail.

## When to use

- "<room> isn't joining meetings / the screen is black / nobody can hear us."
- Bookings behave oddly: room double-books, declines everything, or the console shows no meetings.
- "The board meets Thursday — make sure the room works."
- A fleet question: "which of <client>'s rooms are unhealthy?"

## Steps

1. Identify the room's stack from documentation (`search_itglue` / `search_hudu`): platform (Microsoft Teams Rooms, Zoom Rooms, or ad-hoc PC + peripherals), the room compute device's hostname, the resource-mailbox address the room books under, and the peripheral inventory. If the room is undocumented, that is finding one.
2. Device health: locate the room device in the RMM (`search_ninjaone_devices`, `get_ninjaone_device`) — last contact, pending reboot, disk, OS/app update state; `list_ninjaone_alerts` and `get_ninjaone_device_activities` for recent errors. Room systems are appliances that degrade after weeks of uptime — a pending reboot older than the vendor's guidance is a top suspect. Peripheral faults (camera/mic/display dropouts) usually need eyes on the room; hand off via `get_ninjaone_device_link` plus a physical checklist (cables, HDMI/USB, display input).
3. Calendar-integration check when bookings misbehave: verify the room's resource mailbox exists and is processing invites (auto-accept), the room device is signed into that account, and its password/certificate has not expired — sign-in expiry is the classic "console shows no meetings" cause. Mailbox-side fixes are an M365 admin handoff; specify exactly what to check. Cross-reference the onboarding-and-access mailbox skills for delegation/permission mechanics.
4. Pattern check: `search_tickets` for this room's history. Three tickets in a month is not three incidents — it is one unreliable room; recommend the systemic fix (reboot schedule, firmware, replacement) instead of a fourth restart.
5. Pre-meeting checklist (proactive mode): schedule it the business day before via `schedule_ticket`. Remote pass: device online, healthy, no pending reboot, signed in, meetings visible on the console, updates not about to auto-install mid-meeting. On-site pass (hand off as a checklist): start a test call, verify camera/mic/speaker/display and content sharing, check the booking shows on the panel. Report pass/fail per item — never "should be fine".
6. Output: room status, cause hypothesis with evidence, actions taken vs handed off, and for proactive checks the checklist results. Post plain-text notes via `add_ticket_note`; open `create_ticket` for systemic fixes (firmware cycle, hardware replacement).

## Guardrails

- Never reboot a room system during business hours without checking the room's calendar for in-progress or imminent meetings — interrupting a live meeting to fix a future one is a self-inflicted incident.
- Sign-in and mailbox changes touch identity: they are specified and handed to the M365 administrator, not performed here; never place account passwords in ticket notes.
- A remote "all green" is not a pre-meeting pass — the checklist's on-site items exist because AV fails physically. Report unverified items as unverified.
- If the room hardware is under an AV integrator's contract, package and route rather than opening the rack.
- Notes are plain text — no markdown, no emojis.

## See also

- `resource-mailbox-setup` (m365-administration) — room/equipment mailbox creation, booking policies, and delegate approval flows.
- `shared-mailbox-delegation` (onboarding-and-access) — the mailbox-permission mechanics behind room calendars.
- `device-health-check` — the general single-device workup this skill specializes for rooms.
