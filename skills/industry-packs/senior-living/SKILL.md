---
name: Supporting Senior Living Communities
description: Vertical pack for senior-living, assisted-living, and skilled-nursing clients — EHR/eMAR platforms (PointClickCare, MatrixCare-class) and the med-pass clock, nurse-call and life-safety adjacency, resident wifi vs clinical network separation, HIPAA, and night-shift coverage realities. Load when the client is a senior-living community or the ticket names an eMAR, nurse call, or resident network.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Senior Living Communities

A senior-living community is a 24/7 clinical operation wrapped in a hospitality business: medication passes run on a clock, nurse-call buttons must always work, and the overnight shift is a skeleton crew with no on-site IT. The severity model here is clinical first — an eMAR outage during med pass or anything touching nurse call outranks everything else on the board. This pack layers senior-living knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is an assisted-living, independent-living, memory-care, skilled-nursing (SNF), or CCRC community, or the ticket names PointClickCare, MatrixCare, ALIS, Eldermark, Yardi Senior Living-class platforms, or an eMAR
- "The med cart can't connect," "nurses can't chart," nurse-call or wander-management system tickets
- Resident wifi complaints, or anything proposing to join resident devices to community networks
- After-hours calls from a charge nurse or overnight staff
- Any senior-living ticket where resident information could land in a note

## The stack you'll meet

- **EHR/eMAR:** PointClickCare and MatrixCare dominate skilled and assisted care; ALIS, Eldermark, Yardi Senior Living-class serve AL/IL. Care plans, charting, and the electronic medication administration record (eMAR) live here — mostly cloud, accessed from med carts (laptops/tablets on wheels) over wifi. The classic failure chain is wifi → cart → eMAR, in that order of likelihood.
- **Med carts and clinical devices:** carts roam hallways at the edge of AP coverage; dead zones, roaming drops, and battery-dead carts generate steady tickets with clinical urgency. Vitals devices and med-dispensing units may bridge to the EHR.
- **Nurse call and life-safety adjacency:** nurse-call systems (and wander-management/door systems in memory care) are life-safety-adjacent, usually vendor-installed and vendor-maintained, increasingly riding the IP network. The desk supports the network under them but does not modify the systems themselves — a nurse-call or wander-management fault gets the vendor and the community's leadership immediately, and any network change that could affect their VLANs/paths is a planned change with the vendor consulted.
- **Adjacent:** pharmacy interfaces (orders flow from a contracted LTC pharmacy into the eMAR — interface failures mean manual med reconciliation), dining/POS, staff scheduling, family-communication portals, and door-access systems.

## Resident wifi vs clinical network

Two populations share the building: residents (personal TVs, tablets, medical alert devices, dozens of consumer gadgets) and clinical operations. The workable posture is hard separation — resident wifi is an amenity network isolated from clinical VLANs, and resident devices never join clinical networks no matter how insistent the request. Resident-wifi complaints are real tickets (for many residents it's their main family link) but they never outrank clinical issues, and the support boundary for personal devices should follow the community's documented scope. Flag any discovered cross-connection between resident and clinical networks as a security finding, not a convenience.

## HIPAA inside the ticket

Communities providing care are covered entities; the MSP is a business associate. Minimum necessary in every artifact — room or unit numbers over resident names where identification is unavoidable, never name + condition + medication together; no charting or eMAR screenshots. Suspected exposure → facts only, flag the community's compliance/privacy owner and your internal escalation path; no breach-notification opinions.

## The rhythm and what's sacred

- **Med-pass windows:** medication passes cluster around early morning, midday, evening, and bedtime. An eMAR, wifi, or cart failure during a pass forces nurses onto paper fallback and manual back-entry — treat as top severity; ask "are you in a med pass right now?"
- **Night shift:** overnight staffing is minimal, non-technical, and often agency staff unfamiliar with the systems. After-hours support must assume low technical context — spoken-word, step-by-step help, and a bias toward dispatching rather than long remote debugging when clinical systems are down.
- **Shift changes** (roughly 6–7 AM/PM) are the worst moments for planned work — charting and handoff load peaks.
- **Sacred:** nurse call and wander management (life-safety — vendor + leadership immediately), the eMAR path during med passes, the pharmacy interface, and door-access systems in memory care.

## Steps

1. Context: search_tickets for this community's history (cart and AP dead-zone fixes repeat), and search_itglue / search_hudu for the EHR/eMAR platform, med-cart fleet, wifi layout, nurse-call and wander-management vendors and contracts, pharmacy-interface details, and the after-hours escalation chain.
2. Set severity on the clinical clock: nurse-call or wander-management fault → top severity, vendor engaged, community leadership notified, immediately. eMAR/wifi failure during a med pass → top severity. Resident-wifi or admin-side issues → normal queue.
3. Split the eMAR failure chain in order: platform status (cloud-side outage — check the vendor status page early) → internet/firewall → wifi (one hallway? one AP?) → the specific cart (battery, NIC, sleep settings). Scope by asking whether other carts and wired stations work.
4. For nurse-call/wander-management: verify the network layer beneath it (switch, VLAN, PoE) only; everything in the system itself goes to the vendor with the community's leadership in the loop. Log the vendor case number.
5. Run the LOB framework for application failures: versions, change correlation (cart OS updates and certificate expiries are classics), verbatim error, scope, vendor-escalation package with case number. Pharmacy-interface failures get flagged to nursing leadership because manual reconciliation has clinical risk.
6. Note in plain text: system, scope, clinical-clock impact (med pass yes/no), error verbatim (resident-info-scrubbed), branch taken, vendor case, and verification — a nurse charts and passes meds on the real cart, or the nurse-call vendor confirms restoration.

## Guardrails

- Never modify nurse-call, wander-management, or door-access systems themselves — network-layer support only; faults go to the vendor and community leadership immediately.
- Network changes that could touch life-safety VLANs are planned changes with the vendor consulted — never ad hoc.
- Resident devices never join clinical networks; cross-connections found are security findings to flag, not conveniences to extend.
- Minimum-necessary resident information; no eMAR/charting screenshots; suspected exposure → facts only, flag the compliance owner.
- Med-pass-window outages are never queued — clinical severity first, honest paper-fallback acknowledgment to nursing staff.
- After-hours callers are assumed non-technical: spoken steps, patience, and dispatch bias when clinical systems are down.
- Docs tools vary per tenant — state what you could not verify; an undocumented nurse-call vendor or missing after-hours chain is itself a flag worth raising.
