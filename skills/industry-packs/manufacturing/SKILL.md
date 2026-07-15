---
name: Supporting Manufacturing Clients
description: Vertical pack for manufacturing and industrial clients — the OT/IT boundary (never touch PLCs/SCADA without the OT owner), ERP/MES stack, shift patterns and line-down urgency, and shop-floor devices. Load when the client is a manufacturer or the ticket mentions the shop floor, a production line, machine controllers, or industrial equipment.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Manufacturing Clients

Manufacturing IT has a border, and crossing it uninvited can stop a production line or hurt someone. On one side: the office network, ERP, email — normal MSP territory. On the other: PLCs, SCADA/HMIs, CNC controllers, and the machines they drive — operational technology (OT) with its own owner, its own rules, and consequences measured in scrap, downtime, and safety. This pack layers manufacturing knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework), starting with that border.

## When to use

- The client is a manufacturer, machine shop, processor, or industrial operation, or the ticket names an ERP/MES (Epicor, SYSPRO, Global Shop, NetSuite, JobBOSS-class), shop-floor terminals, label printers, or "the machine's computer"
- Anything that might touch production equipment, its controllers, or its network segment
- Line-down tickets, shift-change timing questions, or maintenance scheduling at a plant
- Discovery scans, patching policy, or security-agent rollout scoping at a manufacturing client

## The OT/IT boundary — rule zero

- **OT is not the desk's to touch.** PLCs, SCADA servers, HMIs, robot controllers, CNC/machine PCs, and anything on the process/machine network belong to the OT owner (plant engineering, maintenance, or the machine vendor/integrator). The desk does not reboot them, patch them, scan them, install agents on them, or "just check" them — even when they run Windows and look like ordinary PCs. An ill-timed reboot of an HMI can stop a line; an unexpected network scan can fault decades-old controllers.
- **Legacy is deliberate.** That Windows XP box running the CNC is not negligence to fix — it is the only OS the machine's control software supports, kept alive by isolation, not patching. The correct posture is segmentation and documented exceptions, decided *with* the OT owner, never a surprise patch or AV push.
- **Know the border before working.** Every manufacturing client should have the OT owner and the OT network segments documented. Before any plant-wide action (patch cycles, agent deployments, network changes, vulnerability scans), confirm OT scope exclusion explicitly. When you cannot tell which side a device is on, stop and ask the OT owner — "when in doubt, do nothing" is literal here.
- Where the sides meet (an ERP pulling from an MES database, a historian feeding reports, file drops from machines), changes are coordinated with both owners — that interface is usually the plant's most fragile dependency.

## The stack you'll meet (IT side)

- **ERP:** Epicor Kinetic, SYSPRO, Global Shop, JobBOSS, Infor-class, NetSuite, or Dynamics — orders, inventory, job costing, purchasing. Classic LOB: database server, client versions, and month-end close sensitivity.
- **MES / shop-floor data collection:** job-tracking and machine-monitoring apps, often vendor-specific, straddling the boundary — treat their machine-side components as OT-adjacent.
- **Shop-floor IT devices (the desk's, usually):** shared kiosk terminals for job clock-in, barcode scanners, Zebra-class label printers (label failures stop shipping — more urgent than they sound), time clocks, quality stations, and break-room-adjacent PCs living in dust, heat, and vibration. Hardware death is routine; documented standard builds and spares are the fix.
- **CAD/CAM engineering workstations:** SolidWorks/Mastercam-class — high-spec machines, license-server dependencies (a down license server idles the whole engineering department; treat as multi-user outage).

## The rhythm and what's sacred

- **Shifts, not office hours.** Plants run 2–3 shifts, sometimes 24/7. "After hours" may be peak production; the real maintenance windows are planned downtime — weekends, shutdown weeks, between-shift gaps — and they belong to the plant's schedule, not the desk's convenience. Get the shift pattern and sanctioned windows documented per client.
- **Line-down is the P1.** Anything stopping production or *shipping* (label printers, scanners feeding the ERP, the ERP itself during order entry/shipping cutoffs) is a revenue event by the minute. Triage question: "is production or shipping stopped right now?"
- **Sacred:** the OT environment (by exclusion — sacred means *don't touch*), the ERP database and its backups, the license server, shipping-label path at cutoff times, and month-end close in the ERP.

## Steps

1. Context: search_tickets for this client + system history, and search_itglue / search_hudu for the plant documentation: OT owner and segments, shift pattern, sanctioned maintenance windows, ERP/MES inventory and vendor contracts, shop-floor standard builds.
2. Classify the ticket's side of the border first. OT or ambiguous → coordinate with the OT owner before any action; the desk's contribution is evidence (what the IT side sees) and scheduling, not hands on controllers. IT side → proceed.
3. Triage by production impact and shift clock: line/shipping stopped → top severity any hour; single kiosk with a spare → swap and move on. Confirm which shift is affected and when the next sanctioned window opens for anything disruptive.
4. Run the LOB framework for IT-side failures: exact versions (ERP client vs server mismatch after partial updates is the classic), change correlation (Windows patches breaking label-printer drivers and scanner wedges is a recurring pattern), verbatim error, vendor known-issue search.
5. Branch: environment-side (network, workstation, printer/scanner, security-agent quarantining a shop-floor app component) is the desk's; ERP/MES-internal faults are vendor territory — full vendor-escalation package with case number, and coordinate any vendor remote session that could touch OT-adjacent components with the OT owner present.
6. Plan disruptive work only inside sanctioned windows, with the plant's sign-off recorded in the ticket; verify after the window with the shift actually using the system (clock into a job, print a label, ship an order).
7. Note in plain text: system, which side of the boundary, production impact, shift/window context, error verbatim, approvals, branch, vendor case, verification.

## Guardrails

- Never touch PLCs, SCADA, HMIs, machine controllers, or the OT network segment — no reboots, patches, agents, or scans — without the OT owner's explicit direction and presence in the loop. Ambiguity = stop and ask.
- Plant-wide automations (patching, agent rollouts, discovery/vulnerability scans) must have OT segments explicitly excluded, and the exclusion documented, before they run.
- Legacy machine PCs are managed by isolation and documented exception with the OT owner — never by surprise patching or AV installation.
- Disruptive work happens in the plant's sanctioned windows with recorded sign-off; a quiet office at 6 PM does not mean the plant is idle.
- Never operate on the ERP/MES database outside vendor procedure; month-end close periods get change freezes on the ERP.
- Safety trumps tickets: anything suggesting a safety-relevant malfunction goes to the plant's OT/safety owner immediately, without tinkering.
- Docs tools vary per tenant — state what you could not verify; a manufacturing client with no documented OT owner or boundary is itself a top-priority flag for the account owner.
