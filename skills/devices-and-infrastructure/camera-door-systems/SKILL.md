---
name: Camera and Door Systems
description: Support a client's physical-security stack — camera/NVR health, recording-retention verification, and door-controller issues with vendor escalation — while enforcing that footage requests require documented client authorization. Use for "cameras are down", "pull footage from <date>", or badge/door-access malfunctions.
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, list_ninjaone_alerts, get_ninjaone_device_link, search_itglue, search_hudu, search_tickets, add_ticket_note, send_approval, create_ticket, update_ticket]
---

# Camera and Door Systems

Keep the physical-security layer honest: is the NVR recording, is retention actually meeting the client's stated window, are door controllers reachable — and when someone asks for footage, verify authorization before anyone exports a frame.

## When to use

- "The cameras at <site> are down / not recording."
- "Can you pull footage from <date/time>?" — any footage or access-log request.
- A door/badge reader stops working or lets the wrong state persist (stuck locked/unlocked).
- Periodic check: "is the NVR healthy and is retention where it should be?"

## Steps

1. Map the estate from documentation (`search_itglue` / `search_hudu`): NVR/VMS platform, camera count per site, door-controller vendor and model, who the servicing vendor is, and — critically — where the support boundary sits. Many MSPs support the server/network layer only, with a security integrator owning cameras, licenses, and door hardware. State the boundary in your output.
2. NVR health: if the NVR is an agent-managed or monitored device, pull `get_ninjaone_device` (disk health above all — NVRs die by disk), `list_ninjaone_alerts`, and last-contact. Cameras themselves are rarely in the RMM; camera-down determination usually needs the VMS console — a deep-link/hands-on handoff (`get_ninjaone_device_link` for the NVR).
3. Retention check: compare the client's required retention window (from documentation or compliance requirements) against reality — oldest available footage on the NVR (hands-on check via the VMS). Retention silently shrinks when cameras are added or resolution increases; a "30-day" system recording 12 days is a finding, not a footnote. If required retention is undocumented, flag that as its own gap.
4. Footage or access-log requests — authorization gate FIRST: verify the requester is on the client's documented authorized-requester list (or is the client's own management), capture the request in writing (what timeframe, which cameras, why), and route for explicit approval via `send_approval` before any export happens. If law enforcement is the requester, route to the client's management and the MSP's own leadership — the desk does not hand footage to third parties. Record the approval and fulfillment trail in the ticket.
5. Door-controller issues: check controller reachability/power at the network layer (is its switch port up, is PoE budget exceeded — cross-check recent changes via ticket history). A door stuck unlocked is a physical-security incident: advise the client immediately and recommend on-site interim measures; do not leave it as a queued ticket. Beyond the network layer, escalate to the door-system vendor with a tight package: site, door, controller model, symptom, network findings, and the client's service-contract reference — track the vendor engagement like a carrier ticket (reference number, ETR, follow-up scheduled).
6. Output: estate health summary (NVR status, disk, retention verdict, controllers), any authorization-gated requests and their status, vendor escalations opened, and next actions. Post plain-text notes via `add_ticket_note`; keep the ticket status honest (`update_ticket`) when waiting on the security vendor.

## Guardrails

- No footage, screenshots, or access logs are exported or shared without documented client authorization — no exceptions for urgency, and law-enforcement requests go through client management and MSP leadership.
- Never state a camera or door is "fine" from network reachability alone — reachable is not recording, and reachable is not locking. Physical verification claims require the VMS or on-site confirmation.
- A door failing unsecure (stuck unlocked) or a fire-egress-related fault is urgent and safety-adjacent: escalate immediately and recommend the client involve their security integrator; never advise disabling door hardware remotely.
- Respect the support boundary: work outside the MSP's documented scope is routed to the security vendor, packaged, and tracked — not attempted.
- Retention findings state observed facts ("oldest footage found: N days") — never certify compliance; that judgment belongs to the client and their compliance advisor.
- Notes are plain text — no markdown, no emojis.

## See also

- `vendor-escalation-package` (escalation) — building the dossier when the door/camera vendor stalls.
- `network-device-inventory` — the switching/PoE layer these systems ride on.
