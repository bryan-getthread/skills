---
name: Facilities Tickets
description: Work the MSP's own office facilities tickets — door/badge systems, alarm, office ISP, printers, HVAC-adjacent vendor calls — with the same triage discipline the desk gives clients. Use when the broken thing is in the MSP's own building.
category: MSP Business Operations
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, create_ticket, update_ticket, add_ticket_note, schedule_ticket, send_approval, web_search]
---

# Facilities Tickets

Dogfoods the desk on the MSP's own office: facilities issues get real tickets, real triage, vendor contacts pulled from documentation, and a paper trail — instead of dying in a hallway conversation. Door systems and alarms are treated as security issues, the office ISP as a service-delivery dependency, and everything else as the routine work it is.

## When to use

- "The badge reader on the back door isn't working." / "Alarm panel is showing a fault."
- "Office internet is down/degraded" — which also means the desk's ability to serve clients is degraded.
- "The upstairs printer is jamming again" and other routine office equipment complaints.
- Standing up the practice itself: "our own office problems should be tickets."

## Steps

1. Capture it as a ticket on the internal/facilities board (create_ticket) if it arrived as chat or hallway lore — requester, location in the building, what's broken, since when. The MSP's office is a client record like any other; use it.
2. Triage by category:
   - **Door systems, locks, badge readers, alarm:** treat as physical-security priority. A door that won't lock or an alarm fault is not a convenience issue — escalate visibility to whoever owns office security the same day. A door that won't *open* during business hours is inconvenient; one that won't *close/lock* is an incident.
   - **Office ISP / network:** this is service-delivery infrastructure — when it degrades, client response degrades. Check whether the desk can fail over (secondary circuit, LTE, work-from-home posture), note the client-facing impact honestly, and open the carrier ticket with circuit ID from documentation.
   - **Printers, monitors, peripherals, meeting-room AV:** routine queue work. Batch recurring ones; a printer that jams weekly is a replacement conversation, not a seventh fix ticket.
3. Pull vendor and asset context from documentation (search_itglue, search_hudu, search_knowledge_base): who services the door system, the alarm-monitoring contract and account number, the ISP support line and circuit ID, printer lease/service coverage. If the office's own documentation is thinner than what the desk keeps for clients, flag that gap — it is the cobbler's-children finding.
4. Coordinate the fix: schedule vendor visits (schedule_ticket), route spend above trivial cost through approval (send_approval — a replacement badge controller is a purchase decision, not a desk decision), and keep the requester updated like a client would be.
5. Close with the same hygiene the desk demands on client tickets: what was wrong, what fixed it, vendor reference numbers, and any follow-up (parts on order, recurring-issue watch).

## Guardrails

- Safety and security first: anything touching doors-that-won't-lock, alarms, or life-safety systems gets same-day human escalation — never sits in a general queue, and never gets a DIY fix suggestion beyond the vendor's documented user procedures.
- Alarm codes, door master codes, and badge-system admin credentials never appear in ticket notes — vault only.
- Office ISP incidents: state client-facing impact factually in the ticket ("response capability degraded, failover active") but keep any client-facing communication about it to the outage-notification process — this skill doesn't message clients.
- Spend approval is a human's: recommend the vendor visit or replacement; do not commit money.
- Facilities tickets often name where specific people sit and building access details — internal board only, and keep physical-layout detail to what the fix needs.
- Vendor procedures change; verify support numbers and RMA paths against current documentation or the vendor's site (web_search) rather than trusting stale notes.

## Unattended (Flows) variant

- Trigger: new ticket on the internal board matching facilities intent.
- Do classification only: categorize (security-physical / office-network / routine), set priority accordingly (security-physical → high), and post one plain-text note naming the category and the documented vendor contact if found.
- Your entire reply is posted verbatim as the ticket note — plain text, no markdown, no narration.
- Never schedule vendors, never send approvals, never message anyone autonomously. Ambiguous category → tag routine and stop.
