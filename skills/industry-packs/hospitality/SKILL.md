---
name: Supporting Hospitality Clients
description: Vertical pack for hotel, resort, and lodging clients — the PMS (Opera, Cloudbeds-class), POS and OTA/channel-manager interfaces, key-card and phone integrations, night audit, and the 24/7 front-desk urgency model. Load when the client is a hotel/lodging property or the ticket names a PMS, key encoder, channel manager, or front-desk system.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Hospitality Clients

A hotel never closes, and its PMS is the property: who's in which room, who owes what, whose key opens which door. Around it hangs a ring of interfaces — POS, OTAs, key encoders, phones — and most hospitality tickets are really interface tickets. This pack layers hospitality knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a hotel, motel, resort, or property-management operation, or the ticket names Opera (Oracle Hospitality), Cloudbeds, Mews, RoomKeyPMS, WebRezPro, Micros/Simphony, a channel manager (SiteMinder-class), or a key-card system (ASSA ABLOY/VingCard, dormakaba/Saflok, Salto-class)
- "Keys won't cut," "rooms are double-booking online," "the restaurant can't post charges to rooms"
- Night-audit failures or anything scheduled during the audit window
- Front-desk device/printer/phone tickets at a 24/7 property

## The stack you'll meet

- **PMS:** Opera (on-prem or Opera Cloud) at branded/full-service properties; Cloudbeds, Mews, RoomKeyPMS, WebRezPro-class at independents. Reservations, folios, housekeeping status, and guest profiles live here. On-prem Opera means a database server on property; cloud PMS means the property's internet line *is* the PMS.
- **The interface ring — where tickets actually live:**
  - **POS → PMS** (Micros/Simphony, Toast-class): restaurant/bar charges post to room folios. When the interface drops, charges queue or go missing — a revenue-leak event the property must know about immediately.
  - **Channel manager / OTA sync** (SiteMinder-class, or PMS-native): pushes availability/rates to Booking.com, Expedia, etc. A stalled sync sells rooms that don't exist — **double-bookings are among the most expensive failures in this vertical**; a suspected sync stall means telling the property at once so they can pull inventory manually while you diagnose.
  - **Key-card encoders** (VingCard, Saflok, Salto-class): the PMS tells the encoder what to cut. Failures split into encoder hardware/connection (front desk can't cut any key) vs PMS-interface vs the lock itself (one door). No keys at check-in time is a lobby full of tired guests — top severity.
  - **PBX/phone**: guest-room phones, wake-up calls, call accounting, and — critically — location-accurate 911 from guest rooms. Treat anything touching emergency calling as change-controlled and verified.
- **Also on property:** guest Wi-Fi (usually a separate managed network/vendor — know whose it is before owning the ticket), payment terminals (PCI — never handle card data; follow the processor's documented procedures), digital signage, and back-office accounting exports.

## The rhythm and what's sacred

- **24/7 with pulse peaks:** checkout surge 7–11 AM, check-in surge 3–6 PM. An outage inside a surge is worth more urgency than the same outage at 2 PM. There is no "after hours" — but there *is* a nightly sacred window:
- **Night audit** (typically ~2–4 AM): the PMS closes and rolls the business day — posting room-and-tax, reconciling, resetting. **Never reboot the PMS server, restart its services, or schedule maintenance during the property's night-audit window**, and treat "night audit won't complete" as an urgent, front-desk-blocking failure (many PMSs limit function until audit completes). Learn and document each property's audit window.
- **Sacred systems:** the PMS and its database, key-cutting at check-in time, the OTA sync (inventory truth), 911/phone from guest rooms, and payment processing. Guest data (identities, stay history, card tokens) gets confidentiality-grade ticket hygiene.

## Steps

1. Context: search_tickets for this property + system (interface drops recur with known restart procedures), and search_itglue / search_hudu for the property documentation: PMS type/version, interface inventory and vendors, night-audit window, key-system vendor, whose network the guest Wi-Fi is, and support contracts.
2. Triage on the property clock: keys-down or PMS-down during a check-in/checkout surge, or night audit stuck → top severity, wake whoever needs waking; single-workstation or back-office issues → normal flow. Always ask the front desk "can you check guests in right now?"
3. Localize to the right layer before diagnosing: PMS itself, an interface (POS posting, channel manager, key encoder), the device (encoder, terminal, printer), or the network path. Interface failures: check the interface service/queue status per vendor documentation — most have a documented restart that front-desk-facing staff cannot see.
4. For suspected OTA-sync stalls: notify the property immediately with the plain risk ("rooms may oversell online — consider closing out inventory manually until sync is confirmed"), then diagnose. Confirm sync recovery with the property against the extranets, not just service status.
5. Run the LOB framework for the failing component: exact versions, change correlation (PMS patches, interface-server Windows updates, certificate expiries are classic), verbatim error, vendor known-issue search — then the vendor-escalation package for PMS-internal, key-system, or channel-manager faults, with the property's support identifiers and case number logged. Interface vendors love to point at each other; a package with evidence from *both* sides of the interface breaks the loop.
6. Note in plain text, guest-data-scrubbed: system/interface, property-clock impact, error verbatim, actions and restarts performed, vendor case, and verification by front-desk staff performing the real workflow (cut a key, post a charge, complete a test booking sync per vendor procedure).

## Guardrails

- Never reboot or maintain the PMS server, interface services, or their host during the property's night-audit window; when the window is undocumented, ask before touching anything at night.
- Suspected OTA/channel-sync failure = tell the property first, diagnose second — double-bookings compound by the minute.
- No guest data in tickets: no names-plus-stay details, no folio contents, no card data ever (PCI: follow the processor's documented procedures; the desk never handles or stores card numbers).
- Anything touching guest-room phones/911 routing is change-controlled and verified with a live test coordinated with the property.
- Key-system changes beyond the documented encoder/interface procedures (lock programming, master-key scope) route to the key-system vendor and property management — the desk does not improvise on physical security.
- Never operate on the PMS database outside vendor procedure; interface "fixes" follow the vendor's documented restart/repost sequences only — reposting charges twice is as bad as losing them.
- Docs tools vary per tenant — state what you could not verify; a property with no documented audit window or interface inventory is itself a flag worth raising.
