---
name: Network Documentation Standard
description: Define what "good" network documentation contains for a client and produce the capture template to get there — the fields a complete network doc must hold, so infrastructure is documented consistently rather than as scattered notes.
category: Documentation
tools: [search_itglue, search_hudu, search_tickets, search_clients, search_knowledge_base, notion-search, notion-create-pages, add_ticket_note]
---

# Network Documentation Standard

Sets the bar for what complete, useful network documentation looks like for a client, and emits the capture template that gets it there. Where documentation/network-diagram-request produces the on-site capture checklist for a single diagram, this skill defines the standing standard the whole client's network documentation is measured against and stored in.

## When to use

- "What should our network documentation for <client> actually contain?"
- Standardizing network docs across the client base so every client's docs hold the same fields.
- A network doc exists but is thin or inconsistent and needs to be brought up to standard.
- Onboarding a client and the network section of the doc pack needs its template (see documentation/client-onboarding-documentation-pack).

## Steps

1. Establish the client with `search_clients` and pull what exists: `search_itglue` / `search_hudu` (where enabled), `search_knowledge_base`, and `notion-search` (if connected) for current network docs; `search_tickets` for recent network changes that may have outdated them (ISP swap, firewall replacement, office move, VLAN changes).
2. Lay out the **network documentation standard** — the sections a complete record must contain, each scored present / partial / missing against what exists:
   - **Topology** — the network diagram itself (link to it; capture it via documentation/network-diagram-request if absent) plus a written summary of the shape.
   - **Internet / edge** — ISP(s), circuit type and speed, account/circuit IDs, public IP assignment, failover arrangement, demarc location.
   - **Perimeter** — firewall/router make, model, firmware, location, HA pairing; WAN/LAN interface map; VPN endpoints (site-to-site and remote-access) by purpose.
   - **Switching** — each switch: make/model, management IP, uplink/stacking map, PoE usage; the port-to-purpose map for key ports.
   - **Wireless** — controller/APs, models, SSIDs, VLAN per SSID, auth method, coverage notes.
   - **Addressing & services** — subnets and their purpose, VLAN list, DHCP scopes and server, DNS servers, static-assignment ranges, gateway addresses.
   - **Servers & appliances on the network** — hosts, NAS, backup appliances, UPS, and which switch/port each connects to.
   - **Remote access** — how staff and the desk reach the network (VPN, RMM, jump host) by method, not credential.
   - **Change history & review date** — when the doc was last verified and by whom.
3. For each missing/partial field, note the **source**: on-site capture, a Liongard inspector read (where the partner runs the relevant inspector — verify it exists and last ran successfully, and state dataprint age), tenant/appliance admin reads, or the client's IT contact. Unverified fields stay "unknown," never inferred from a similar client.
4. Set the **storage and freshness discipline**: the network doc lives in the desk's documentation platform under the client's network/infrastructure section, dated, with a review cadence (re-verify on major change and at least at a set interval). Confirm the exact location convention rather than inventing one.
5. Output the standard-vs-current gap table plus a paste-ready capture template with every field. If the desk keeps network docs in Notion and the destination is confirmed, create the template page with `notion-create-pages`; otherwise deliver paste-ready blocks. Attach the capture template to a site-visit or network-review ticket via `add_ticket_note` if requested.

## Guardrails

- Defines the standard and template — it does not draw topology or invent device details. Anything not verified from a source stays "unknown."
- **Credentials never live in network docs** — device and management passwords go to the vault; the doc references them by vault entry title only (see documentation/credential-storage-audit).
- Liongard-sourced fields require the inspector to exist and have run successfully; state the dataprint age and degrade to on-site/admin capture when the inspector is absent.
- Storage location must be the desk's real convention — ask rather than inventing folder structure.
- Notes and templates are plain text per the Note Format Standard (documentation/note-format-standard) — they may sync to a PSA.
- Docs tools are search-only; writes go to Notion only when the member has connected it and confirmed the destination, otherwise paste-ready output. Degradation: without IT Glue/Hudu, assess against ticket history and requester knowledge and note the narrower baseline.
