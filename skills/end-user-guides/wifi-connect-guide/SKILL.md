---
name: WiFi Connect Guide
description: Draft reply-ready instructions for an end user connecting a work device to wifi — office network, home network, and captive-portal awareness — "send the user wifi connection steps."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# WiFi Connect Guide

Produces a client-ready instruction block for getting a work device online — the correct office network by name, the home-wifi case, and the hotel/café captive-portal trap that generates "internet is broken" tickets from travelers.

## When to use

- "Send <user> steps to get on the office wifi."
- New device or new hire tickets where wifi is the first step.
- "User travels next week — send the hotel-wifi survival note."

## Steps

1. **Verify the client's wireless setup first** (search_itglue / search_hudu / search_knowledge_base / search_tickets): the office network's exact name (SSID) users should join, how it authenticates — certificate/auto-join on managed devices (user does nothing but pick it, or it joins itself), sign-in with work credentials, or a password — and whether a guest network exists that users should NOT put work devices on. If the network name or auth style is unknown, ask the technician ONE question. **Never put a wifi password in the draft** — if the office network is password-based, the draft says where to get it per the client's documented practice, or the tech delivers it out-of-band.
2. **Draft the office branch** to end-user rules, one action per step with what-you'll-see cues:
   - Open the wifi list (cue per platform in plain words), pick the exact documented name — "spelling matters; if you see two similar names, the right one is exactly <documented SSID>."
   - The auth cue that applies: "it may just connect — that's our setup working" (certificate), or "a sign-in box appears; use your normal work login" (credentials).
   - The guest-network warning where one exists: "the network called <guest SSID> is for visitors' phones — work laptops don't belong on it; things like printers and shared drives won't work there."
   - Off-ramp: "If it asks for anything these steps don't mention, or refuses your work login twice, stop and reply — retrying can lock your account."
3. **Home branch:** join their own network normally (their router password, which the desk never needs or wants — say so), then the one work-specific note that applies per client docs (e.g., "once online, the VPN handles the rest — see the VPN steps").
4. **Captive-portal awareness (travel):** the plain-language explanation — "hotel and café wifi shows a welcome/agree page before the internet works; your laptop may look connected but nothing loads until that page is accepted." Steps: connect, open the browser, try any plain website to trigger the page, accept, then connect the VPN. Honest caveat: "some public networks block work tools entirely — if the VPN won't connect after the welcome page, use your phone's hotspot instead," including only if the client permits hotspot use per docs; otherwise the off-ramp is "reply and we'll advise."
5. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Never transmit a wifi password in the draft**, and never invent an SSID — both come from documentation or the tech, or the draft doesn't ship.
- Include only the branches the ticket needs (office / home / travel) — an unrequested three-part guide dilutes the one answer the user wanted.
- Work credentials on wifi sign-in can lock the account when mistyped repeatedly — the stop-after-two off-ramp stays in.
- No admin steps (access-point config, RADIUS, PSK rotation) in the user block — that's troubleshooting-playbooks/wifi-network-troubleshooting.
- The guest-network warning only appears when docs confirm a guest SSID exists; never speculate one into being.
- Localizable; degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled.

## Cross-references

- end-user-guides/vpn-connect-guide — the step after wifi for remote/travel users.
- troubleshooting-playbooks/wifi-network-troubleshooting — tech-facing when the network itself is the problem.
- communication/email-baseline-standard.
