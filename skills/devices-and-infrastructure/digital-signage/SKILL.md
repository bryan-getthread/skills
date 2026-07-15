---
name: Digital Signage Support
description: Support digital-signage players — detect offline/frozen screens, route content-update requests to the content owner (not the desk), and keep the infrastructure-vs-content ownership line clear. Use for "the lobby screen is blank/frozen", "update what's on the screens", or a signage fleet check.
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, list_ninjaone_alerts, get_ninjaone_device_activities, get_ninjaone_device_link, reboot_ninjaone_device, search_itglue, search_hudu, search_tickets, add_ticket_note, update_ticket, create_ticket]
---

# Digital Signage Support

Signage tickets split cleanly in two: the screen/player (infrastructure — the desk's problem) and what plays on it (content — the client's content owner's problem). This skill fixes the first, routes the second, and never confuses the two.

## When to use

- "The lobby / menu / warehouse screen is blank, frozen, or showing an error."
- "Can you change what's showing on the screens?" — content-update requests.
- A signage player shows offline in monitoring.
- "How many signage players does <client> have and are they healthy?"

## Steps

1. Establish ownership from documentation (`search_itglue` / `search_hudu`): the signage platform (CMS), the player hardware type, which players exist per site, and — the load-bearing fact — who owns content vs who owns infrastructure. If the split is undocumented, propose the default (MSP: player hardware, OS, network, CMS agent; client: content, scheduling, playlists) and get it documented.
2. Classify the request. Content requests ("change the menu", "add the holiday notice") are routed to the client's documented content owner with the CMS as the tool — the desk does not edit content unless the contract explicitly says so. Set the ticket accordingly (`update_ticket`) and tell the requester who owns it. Everything else is infrastructure and continues below.
3. Offline/frozen detection: find the player in the RMM (`search_ninjaone_devices`, `get_ninjaone_device` — last contact, uptime, disk) and `list_ninjaone_alerts`. Distinguish the three failure layers: player offline (no RMM contact — power/network), player up but CMS agent dead (RMM sees it, CMS shows it disconnected — needs CMS-side confirmation from the content owner or console), and player fine but content stale (a CMS/publishing issue → content owner). Say which layer the evidence supports.
4. Remediate what the tools allow: for a frozen player that is RMM-reachable, `reboot_ninjaone_device` is usually safe — signage players have no logged-in user to disrupt, but confirm from documentation that the player is not also driving something interactive (kiosk with a customer mid-transaction) before rebooting. Anything deeper (CMS agent reinstall, display/HDMI/power faults) is a hands-on handoff via `get_ninjaone_device_link` plus physical checks (display power, input source, cabling).
5. Fleet mode: sweep all players for the client, bucket by last-contact, and check `search_tickets` for repeat offenders — a player that needs monthly reboots wants a scheduled reboot or replacement, not a ticket ritual. Report capped counts as "at least N".
6. Output: player status, failure layer with evidence, action taken or handoff, and the routing note for any content-side component. Post plain-text notes via `add_ticket_note`; open `create_ticket` for chronic-player replacement or documentation gaps.

## Guardrails

- Never modify signage content, playlists, or schedules unless the contract explicitly assigns content to the desk — a wrong screen in a public lobby is a client-reputation incident. Route to the content owner by name/role from documentation.
- Reboot is low-risk for pure signage but verify the player is not an interactive kiosk or shared with another function first; when unsure, ask.
- Do not declare a player "fixed" from RMM contact alone — the screen showing the right content is the acceptance test; get eyes-on confirmation (requester or on-site contact).
- If signage players are not in the RMM at all, say monitoring is blind, work from the CMS's own status (via the content owner) and physical checks, and recommend agent coverage where the platform supports it.
- Notes are plain text — no markdown, no emojis.

## See also

- `device-offline-runbook` — the generic offline-device workflow this skill specializes.
- `reboot-request-workflow` — the consent discipline for reboots on devices that do have users.
