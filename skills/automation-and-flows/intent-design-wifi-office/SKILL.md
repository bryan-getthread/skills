---
name: Office Wifi Intent Design
description: Build the office-connectivity intent — quick self-help for one user's wifi, site capture, and an everyone-down short-circuit to outage handling. Use when asked to "build a wifi intent", "deflect office network tickets", or when wifi/connectivity ranks high in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets, search_knowledge_base]
---

# Office Wifi Intent Design

Guide the admin through building an office-wifi intent that resolves the single-laptop cases with a short self-help ladder, always captures which site and area the user is in, and short-circuits to outage handling the moment more than one person is affected.

## When to use

- "Build an intent for office wifi problems."
- "Wifi tickets never say which office, let alone which corner of it."
- Intent Mining flagged office connectivity as recurring volume.

## Design

**Trigger phrases (adapt to real ticket language):** "wifi not working", "can't connect to wifi", "wifi keeps dropping", "no internet in the office", "wifi is slow", "laptop won't connect to the network", "network down in the office", "wireless not connecting", "internet keeps cutting out", "can't get on the network".
Near-miss watch: "guest wifi for a visitor" is the guest/visitor intent; "can't connect from home" is home-network/VPN territory; "wifi password?" for staff may be policy-sensitive — route per client policy, never answer from memory.

**Arguments to collect (site capture — always, even when self-help works):**
- Which office/site and where in it (floor, room, area — placeholder: <location>).
- One device or several people nearby (the fork that matters most).
- Device type (laptop/phone/desktop-with-dongle) and whether wired works.
- Slow vs. no-connection vs. drops (different fault classes).
- Since when.

**Reply flow:**
1. **Ask the scope question first.** If colleagues are affected too → skip all self-help, create a ticket flagged possible site outage with site/area/count captured; if the client has a report-outage intent, hand off to that flow.
2. Single user: **toggle wifi off/on** (or airplane mode cycle) and rejoin the network.
3. **Forget and rejoin** the office network (only if the client's setup lets users re-authenticate without help; certificate/802.1X networks skip this rung — rejoining may need IT).
4. **Reboot the device.**
After each rung: "connected now?" — stop and close as deflected on success.
5. Ladder exhausted → ticket with site capture, device, fault class, rungs tried.

**Handoff rule:** never instruct users to touch access points, routers, or switches ("turn the AP off and on" is not end-user self-help). Never disclose network passwords/PSKs; joining instructions per client policy only.

**Variation hooks (per client):** site list and how users name them, network names (corporate vs. guest), whether forget-and-rejoin is safe (802.1X/certificate networks: no), the wifi-password policy.

**Success metric:** deflection rate on single-user wifi conversations, plus site-capture completeness on tickets (a dispatchable wifi ticket names site and area). Multi-user reports converting quickly to outage tickets is success, not failure.

## Steps

1. `list_intents` — check for an existing wifi/connectivity intent (and a report-outage intent to hand off to); prefer updating over duplicating.
2. `search_tickets` for recent wifi tickets; mine phrasing, confirm which self-help rungs actually resolve on this desk, and collect how users refer to sites.
3. `search_knowledge_base` for network self-help articles; link where they exist.
4. Draft the full spec: triggers, site-capture arguments, scope-first branching, ladder, variations. Test plan: 5 should-match, 3–5 should-not (guest-wifi and work-from-home near-misses).
5. Show the draft; on explicit confirmation call `create_intent` then the variation tools.
6. Report what was created, restate the test plan, recommend activation after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, output the complete written spec for an admin to apply.
- Scope question comes first, always — self-helping one user through a site outage wastes everyone's time and hides the incident.
- No infrastructure instructions to end users, and no network credentials in replies.
- Forget-and-rejoin must be confirmed safe for the client's auth setup before it goes in a variation; on certificate-based networks it strands the user.
- Confirm before any write; never activate on the member's behalf. Site capture in plain text on the ticket.
