---
name: Supporting Construction and Field Services
description: Vertical pack for construction, trades, and field-service clients — plan-room and field apps (Procore, Bluebeam, ServiceTitan-class), rugged tablets and jobsite connectivity, equipment tracking, and the crack-of-dawn crew clock. Load when the client is a contractor/field-service firm or the ticket involves a jobsite, field device, or plan-room app.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Construction and Field Services

The office is the easy half. A construction or field-service client's real IT lives in trucks, trailers, and pockets: tablets covered in drywall dust, hotspots at the edge of coverage, and a foreman who needs today's drawing revision before the crew pours concrete. This pack layers field-vertical knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a general contractor, subcontractor, trade (electrical/mechanical/plumbing), or field-service company, or the ticket names Procore, Autodesk Construction Cloud/PlanGrid/Build, Bluebeam Revu, ServiceTitan, BuildOps, FieldEdge, Sage 300 CRE, Foundation, or Viewpoint-class software
- Field-device tickets: tablets/phones not syncing, hotspot/LTE problems, "the app works in the office but not on site"
- Jobsite connectivity setup or failure (trailer internet, Starlink/LTE routers, cameras)
- Lost/damaged device replacement and equipment/asset tracking questions

## The stack you'll meet

- **Project management / plan room:** Procore and Autodesk Construction Cloud (PlanGrid/Build) dominate; drawings, RFIs, submittals, daily logs live there. The field pain point is *sync*: huge sheet sets cached to tablets over weak links, and version currency — a crew building from a stale drawing revision is a real-money defect, so "sync isn't finishing" is more urgent than it sounds.
- **Markup:** Bluebeam Revu on estimator/PM workstations — heavy PDFs, license-server or subscription sign-in issues, and performance tickets that are really "a 400-sheet set on a 8 GB machine."
- **Field service dispatch:** ServiceTitan, BuildOps, FieldEdge-class — office dispatch board + technician mobile app. When the mobile app fails, techs can't see jobs, capture payments, or close work orders; treat a fleet-wide mobile failure like an office-wide outage.
- **Back office:** construction accounting (Sage 300 CRE, Foundation, Viewpoint/Vista, QuickBooks in smaller shops) — classic on-prem LOB with job-costing databases; payroll deadlines make it sacred on payroll-run days.
- **Field hardware:** rugged or ruggedized tablets (iPads in cases, Samsung Tab Active-class), hotspots and LTE/5G routers (Cradlepoint-class), jobsite trailers with Starlink or LTE WAN, site cameras, GPS/telematics on vehicles and equipment (Samsara, Verizon Connect-class), and Toughbook-class laptops in trucks.

## The rhythm and realities

- **The clock starts at 6 AM.** Crews mobilize at or before dawn; a dispatch-app or plan-sync failure at 6:30 AM idles paid labor by the hour. The urgency window is shifted earlier than office verticals — and Friday-afternoon changes break Monday-5 AM mobilizations.
- **Connectivity is probabilistic.** Field apps are built offline-first for a reason. Diagnose in the right order: does it work on office Wi-Fi? On the hotspot at the office? Only on site does it fail? That sequence separates app/account problems from coverage problems — don't debug the app when the answer is "that site is a dead zone; here's the offline workflow."
- **Devices churn hard.** Drops, dust, water, theft from trucks, and crew turnover mean field devices are consumables. What matters is fast, repeatable replacement: MDM enrollment, app set, and sign-ins documented so a spare goes out same-day. Lost devices are also a security event — remote-wipe via MDM and check what was signed in.
- **Equipment tracking:** GPS trackers and telematics tickets are usually power (unplugged/tampered), cellular coverage, or platform account issues — and "the tracker shows the excavator in the wrong place" occasionally means theft; flag anomalies to the client promptly rather than assuming sensor error.

## What's sacred

The dispatch platform during working hours, the plan room's version integrity (never let a crew unknowingly work from stale drawings — if sync state is uncertain, say so explicitly to the client), payroll/job-cost accounting on payroll days, and estimating workstations during bid deadlines (bid day is this vertical's filing deadline — ask "is there a bid due?" when an estimator calls urgent).

## Steps

1. Context: search_tickets for this client + app/device history (field-sync and hotspot tickets recur with known fixes), and search_itglue / search_hudu for the field-stack documentation: MDM tenant, device/app standard build, hotspot carrier accounts, jobsite WAN inventory, vendor contracts.
2. Triage by crew impact and clock: crews idle or fleet-wide mobile failure → top severity regardless of hour; single device with a spare available → swap first, diagnose later. Ask "is a crew waiting on this right now?" and "is there a bid or pour scheduled today?"
3. Localize the failure with the location ladder: office Wi-Fi → cellular in a known-good area → the actual site. App/account issues follow the LOB framework (exact app versions, change correlation, verbatim error, vendor known-issue search); coverage issues become connectivity work (carrier, router placement, external antenna, Starlink) with an honest interim ("cache your sheets before leaving the office").
4. For device loss/damage: MDM remote-wipe and account sign-out for lost units, record serial/asset tag, issue the documented spare build, and note the security actions taken. Flag repeated same-site losses to the client.
5. Vendor territory (platform sync engine faults, database issues in accounting, telematics platform errors) → complete vendor-escalation package per the framework with case number and follow-up cadence.
6. Note in plain text: app/device + versions, site vs office localization result, crew/bid impact, error verbatim, actions (including any wipe), vendor case, and verification — the field user performs the real workflow on site or the interim workaround is stated plainly.

## Guardrails

- Never leave drawing/plan sync state ambiguous — if you cannot confirm the field device has current revisions, say so explicitly in the ticket and to the user; stale-plan risk is a physical-world defect.
- Lost or stolen devices are security events, not just hardware replacements: wipe via MDM, sign out sessions, and note it — before ordering the replacement.
- Field workarounds must respect the offline-first design — instruct caching/offline mode rather than telling crews to "wait for signal."
- Telematics/GPS anomalies get flagged to the client as possible theft/tampering rather than silently dismissed as glitches.
- Payroll-day and bid-day changes to accounting/estimating systems are deferred unless the client explicitly accepts the risk.
- No remote execution on field devices beyond documented MDM actions; credentials and carrier account details stay in the documentation system, referenced by location.
- Docs tools vary per tenant — state what you could not verify; a field fleet with no documented standard build or MDM is itself a flag worth raising.
