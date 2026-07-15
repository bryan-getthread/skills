---
name: Network Diagram Request
description: Produce the capture checklist for getting a useful network diagram out of a site visit — what to record, what to photograph, and exactly where the finished diagram gets stored — so the visit yields documentation, not a pile of photos.
category: Documentation
tools: [search_itglue, search_hudu, search_tickets, search_clients, create_ticket, add_ticket_note]
---

# Network Diagram Request

Turns "we need a network diagram for <client>" into a concrete site-visit capture checklist plus storage instructions — because a diagram is only as good as what got written down on site, and only findable if it lands in the agreed place.

## When to use

- "We need a network diagram for <client> — what should the tech capture on the visit?"
- A new client onboarding or site audit is scheduled and the diagram is a deliverable.
- An existing diagram is stale and a refresh visit is planned: "what do we re-verify?"

## Steps

1. Check what already exists: search_itglue / search_hudu (where enabled) for an existing diagram or network documentation for <client>, and search_tickets for recent network changes (new ISP, firewall swap, office move). An existing diagram converts the task from "capture everything" to "verify and capture deltas" — list the known-stale points from ticket evidence.
2. Build the **capture checklist**, scoped to the site(s) being visited:
   - **Edge:** ISP(s) and circuit type, modem/ONT, public IP assignment method, failover if any.
   - **Perimeter:** firewall/router make, model, and location; WAN/LAN port usage.
   - **Core:** switches (make/model/port count, uplink map — which port feeds what), VLANs in use and their purpose.
   - **Wireless:** APs (count, models, mount locations), SSIDs and which VLAN each lands on.
   - **Servers and appliances:** physical hosts, NAS, backup appliances, UPS — what's plugged into which switch.
   - **Addressing:** subnets in use, DHCP scope and where DHCP is served, static-assignment ranges, DNS servers.
   - **Interconnects:** site-to-site VPNs, point-to-point links, cloud connectivity.
   - **Photos:** rack front and back, patch panels with labels, the demarc, device labels/serials — photographed close enough to read (feed these to documentation/photo-to-documentation afterward).
   - **The "unknown" column:** the checklist explicitly instructs the tech to record "could not verify" per item rather than leaving blanks — a blank later reads as "not present."
3. Set the **storage discipline** lines on the checklist itself: the finished diagram goes to the agreed documentation platform under the client's network/infrastructure section (confirm the exact location convention with the requester or from how existing diagrams for other clients are filed); source/editable file stored alongside the export; photos attached to the same entry, not left in a phone or chat thread; the entry dated and marked with the visit date.
4. If asked, create the site-visit ticket via create_ticket with the checklist embedded as the work instruction (plain text per the Note Format Standard skill, documentation/note-format-standard), or attach the checklist to an existing visit ticket via add_ticket_note.
5. Output the checklist itself, the known-stale points to re-verify, and the storage instructions.

## Guardrails

- This skill produces the checklist and destination — it does not draw the diagram or invent topology. Anything not captured on site stays "unknown," never inferred.
- Storage location must be the desk's real convention — ask rather than inventing a folder structure; a diagram filed in a novel location is a lost diagram.
- Credentials are out of scope for the checklist: device passwords encountered on site go to the credential vault, never onto the diagram, the checklist, or the ticket (see documentation/credential-storage-audit).
- Notes and embedded checklists are plain text per the Note Format Standard — they may sync to a PSA.
- Degradation: without IT Glue/Hudu, run the existing-docs check against search_tickets and the requester's knowledge only, and confirm where the desk stores diagrams before promising a location.
