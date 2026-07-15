---
name: Wi-Fi & Network Troubleshooting
description: Diagnose wireless and LAN problems — slow/dropping Wi-Fi, can't connect, dead zones, "the internet is down" — by laddering scope from single user to AP to site before touching anything.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# Wi-Fi & Network Troubleshooting

The first diagnostic act is scoping: one user, one AP/area, or the whole site. Everything downstream — and who should be working the ticket — follows from that answer. This playbook ladders the scope, then branches.

## When to use

- "<user>'s Wi-Fi keeps dropping / is slow"
- "The conference room has terrible signal" / dead zones
- "Nobody in the office can get online" or new devices can't join
- Guests stuck at a captive portal, or devices connect but get no address

## Steps

1. **History first.** search_tickets for this client's network. Recurring drops at the same location or time are a pattern ticket — correlate before treating today's instance as new.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the network: AP vendor/controller, SSID design (corp/guest/IoT), DHCP scopes and who serves them, ISP and circuit details.
3. **Ladder the scope — this is the diagnosis.** Ask/verify in order: Does it affect (a) one user/device, (b) everyone near one AP or area, (c) one SSID everywhere, or (d) the whole site including wired?
   - (a) → endpoint branch. (b) → AP/RF branch. (c) → SSID/policy/DHCP branch. (d) → uplink/core branch — treat as an incident, not a per-user ticket.
4. **Get evidence before theorizing.** For the endpoint: signal strength, which AP/band it's on, IP config (valid lease vs self-assigned 169.254.x). For infrastructure: controller/AP status, DHCP scope utilization, ISP circuit state. Never assume — ask for the numbers.
5. **Branch:**
   1. **Single user/device** — driver version (verify current with web_search — never assume it's updated), power-save on the wireless NIC, forgotten/stale saved network profile (forget and rejoin), band issues (device pinned to congested 2.4GHz). If the device works elsewhere on the same SSID, it's positional → branch 2.
   2. **Roaming / sticky client** — drops when moving between areas: the device clings to a distant AP. Check minimum-RSSI/roaming settings on the infrastructure and the NIC's roaming aggressiveness. Escalate when: fixing requires controller changes — route to the network owner with the evidence (which APs, signal readings).
   3. **AP-wide** — everyone near one AP: check the AP's status (up? recently rebooted? flapping uplink?), channel utilization/interference, and whether it changed channel or power recently. Physical/RF causes (new equipment, moved furniture, neighbor networks) are real — say when a site survey is the honest next step.
   4. **DHCP exhaustion** — devices connect but no valid IP, or "connected, no internet" bursts at busy times. Check scope utilization and lease time versus device turnover (guest networks with long leases exhaust silently). Fix guidance: shorten leases or widen the scope — a change for the network owner, with the utilization numbers attached.
   5. **Captive portal** — guests stuck: portal page not appearing (device can't reach the portal — DNS interception vs HTTPS), certificate errors on the portal, or authentication backend down. Distinguish "portal broken for all guests" (infrastructure) from one device (its captive-portal detection — toggle Wi-Fi, try http:// site).
   6. **Site-wide** — wired affected too → ISP circuit, edge firewall/router, or core switch. Verify the demarc: can the edge device reach its gateway? If the ISP is down, say only the ISP can act, log the outage reference, and set honest ETRs.
6. **Verify and note.** Confirm from the affected location/device, not just dashboard green. Plain-text note: scope determined, branch, evidence (signal/scope numbers), action or handoff, verification.

## Guardrails

- No remote execution and no config pushes — infrastructure changes are evidence-backed recommendations for whoever owns the network.
- Never skip the scope ladder; a site-wide outage worked as a single-user ticket wastes the response window. If scope is (d), flag it as an incident immediately.
- Do not guess channel plans, scope sizes, or ISP details — pull from docs or ask; do not invent vendor documentation.
- Be honest when the fix is physical (more APs, cabling, site survey) — no settings tweak fixes RF physics.
- Docs tools vary per tenant — note what you could not check.
