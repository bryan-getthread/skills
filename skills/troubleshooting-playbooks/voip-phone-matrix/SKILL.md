---
name: VoIP Phone Matrix
description: Diagnose VoIP problems — inbound calls failing, ring groups misbehaving, one-way audio, dead phones, provisioning failures — by first splitting one-phone vs site vs trunk, then working the right layer.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# VoIP Phone Matrix

Voice tickets collapse quickly once you know which layer is sick: the handset, the LAN path, the PBX/platform config, or the trunk/carrier. The matrix splits on scope first, and treats one-way audio as what it almost always is — a NAT/firewall problem, not a phone problem.

## When to use

- "Callers can't reach us" / inbound dies while outbound works (or vice versa)
- "The ring group isn't ringing <user>" or calls route to the wrong place
- One-way audio, choppy audio, or calls dropping mid-conversation
- A phone shows no registration, or a new/replacement phone won't provision

## Steps

1. **History first.** search_tickets for this client's phones — voice issues recur with network changes; a firewall or ISP ticket in the same window is probably the cause, and prior one-way-audio tickets document what fixed the path last time.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the voice environment: platform (hosted/cloud vs on-prem PBX) and vendor, trunk/carrier, phone models, VLAN/QoS design, and any documented SIP ALG/firewall settings.
3. **Identify versions — never assume.** Phone model and firmware, and the platform/app version for softphones; carrier-side and vendor-side changes land as "it broke overnight."
4. **Scope split — the first diagnostic.** One phone/user → device branch. One group/queue → routing branch. Whole site → trunk/network branch. Inbound-only vs outbound-only vs both → different suspects (inbound-only failures live at the carrier/DID/routing layer; outbound-only points at trunk auth or dial plans).
5. **Get the evidence before theorizing.** The platform's call logs/SIP traces for a failed call example (time, caller, callee), the phone's registration state, and for quality issues the platform's quality metrics (MOS/jitter/loss) if exposed. A specific failed-call example is worth an hour of generalities.
6. **Branch:**
   1. **Inbound faults** — calls in fail, outbound fine. Ladder: does the DID reach the platform at all (platform logs show the attempt or silence — silence means carrier/DID routing: open the carrier ticket and say plainly only the carrier can act on their routing); if it arrives, follow the inbound route/IVR/hours rules to where it dies. After-hours/holiday schedules misfiring is a boring, common answer — check the calendar rules and the timezone on them.
   2. **Ring groups / routing** — a member not ringing: their registration state, DND/forwarding flags, simultaneous vs sequential strategy and timeouts, and whether their client (deskphone vs app) is the one actually registered. Verify against intended design in docs — sometimes the config is doing exactly what it was (wrongly) told.
   3. **Trunk-level** — site-wide failure: trunk registration/peering state on the platform or PBX, carrier status page/ticket, recent firewall or ISP changes (a new public IP breaks IP-authenticated trunks — check that first after ISP work). Escalate when: trunk is down at the carrier — carrier-only fix; capture the failure evidence and reference number.
   4. **Provisioning** — new/replacement phone dead: is the device registered in the platform's provisioning with the right MAC; can it reach the provisioning server (VLAN placement, DHCP options for phones, filtered outbound); firmware too old to support current provisioning/TLS (verify the vendor's minimum with web_search). A phone on the wrong VLAN explains "no config and no calls" at once.
   5. **One-way audio / quality — the network-path branch.** One-way audio is NAT/firewall asymmetry until proven otherwise: signaling works (call connects) but RTP flows one direction. Check the documented firewall settings — SIP ALG on consumer/branch routers is the number-one culprit (disable per platform vendor guidance), then correct RTP port ranges, keep-alives, and NAT settings per the platform's published firewall guide. Quality issues (choppy, robotic): jitter/loss on the path — QoS, saturated uplink, Wi-Fi softphones; recommend measurement (the platform's quality stats per call) over anecdotes. Escalate when: firewall/router changes are needed — network owner, with the vendor's firewall guide attached; when the path is the ISP's — carrier/ISP case, say so.
7. **Verify and note.** Place the actual failing call type end-to-end (inbound from an outside line, the specific ring group, two-way audio held for a minute). Plain-text note: scope split, branch, evidence (call example, trace/log findings), fix or handoff with reference numbers, verification.

## Guardrails

- No remote execution and no PBX/firewall changes by the agent — configuration steps are evidence-backed guidance for the platform admin or network owner.
- Carrier-layer problems (DID routing, trunk outages, number ports) can only be fixed by the carrier — say so immediately, open/track the case, and stop burning endpoint time.
- Do not recite platform-specific firewall/port requirements from memory — pull the vendor's current guide with web_search or from client docs.
- Never disable SIP ALG or change firewall settings "to test" without the network owner — one-way-audio fixes are changes, and changes get owners.
- Emergency-calling implications: any change touching trunks or outbound routing gets a post-change 933/test-call verification per the platform's guidance — flag it explicitly.
- Docs tools vary per tenant — note what you could not check.
