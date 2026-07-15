---
name: Supporting Dental Practices
description: Vertical pack for dental-practice clients — the PMS and imaging stack (Dentrix, Eaglesoft, Open Dental, Dexis-class sensors), HIPAA handling inside tickets, and the morning-huddle downtime clock. Load when the client is a dental office or the ticket names dental software or x-ray sensors.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Dental Practices

A dental office is a small business where one application runs everything — schedule, charts, imaging, billing — and the whole team stands around it at 7:50 AM planning the day. This pack layers dental-specific knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework): what the apps are, how HIPAA changes ticket hygiene, and why a "minor" outage at opening time is a P1.

## When to use

- The client is a dental (or orthodontic/oral-surgery) practice, or the ticket names Dentrix, Eaglesoft, Open Dental, Curve, Denticon, Dexis, Sidexis, Romexis, Apteryx/XVWeb, Carestream, or an "x-ray sensor"
- "The schedule won't open," "images won't come up in the operatory," "the sensor isn't capturing"
- Triaging priority for a dental client — the practice-day clock changes severity
- Any dental ticket where patient information might land in a note

## The stack you'll meet

- **Practice management (PMS):** Dentrix, Eaglesoft, and Open Dental are the classic on-prem trio — a database server in a closet, workstation clients in every operatory and at front desk. Curve Dental and Denticon are cloud-class equivalents (browser problems, not database problems). The PMS is the practice: schedule, charting, billing, recall all live in it.
- **Imaging:** Dexis, Schick/Sidexis, Carestream, Planmeca Romexis, Apteryx/XVWeb-class. Imaging is usually a *separate* application "bridged" to the PMS — the bridge (patient-context handoff) is its own failure point. Sensors are USB devices with TWAIN/vendor drivers; a sensor that stopped capturing is most often a driver, USB port/hub, or cable problem — and the sensor itself is a multi-thousand-dollar part, so never declare one dead without swap-testing against a known-good operatory.
- **Adjacent:** eClaims/clearinghouse modules inside the PMS, intraoral cameras, panoramic/CBCT units (often on their own aging PC the vendor controls), patient-communication add-ons (reminder texts/emails).

Diagnostic habits that pay off: client/server version mismatch after a partial PMS update is the top repeat offender; imaging failures split cleanly into "sensor/driver" (one operatory) vs "imaging server/bridge" (everywhere); and the imaging database is frequently *not* inside the PMS backup — confirm both are covered rather than assuming.

## HIPAA inside the ticket

Dental practices are HIPAA covered entities and the MSP is a business associate — assume a BAA exists and behave accordingly.

- **Minimum necessary in every artifact.** Tickets, notes, and escalations should identify patients only when unavoidable, and then minimally (chart number over name; never full DOB + name + treatment together). "The schedule module errors when opening any patient" beats naming the patient it was discovered on.
- **No PHI screenshots.** Screenshots of the PMS almost always contain the schedule or a chart. Ask for the error dialog cropped, or the error text typed out.
- **Suspected exposure is a flag, not a fix.** If a ticket suggests PHI went somewhere it shouldn't (misdirected email, stolen laptop, open share), capture facts and flag the practice's compliance/privacy owner and your internal escalation path — do not offer breach-notification opinions.

## The rhythm and what's sacred

- **The morning huddle:** most practices open 7–8 AM and start with a huddle around the day's schedule. A PMS or imaging outage between 7:00 and 8:30 AM stops the huddle, the first patients, and hygiene checks — treat opening-time outages as top severity regardless of how small the technical cause looks.
- **Compressed weeks:** many practices run Mon–Thu and use Fridays for admin. Friday is the maintenance window; Monday 7 AM is the worst possible time to discover Friday's change broke something — verify Friday work before the weekend ends.
- **Sacred systems:** the PMS database server, the imaging server, and their backups. A dental practice that loses its PMS database loses its schedule, its charts, and its receivables at once. Backup-failure alerts for these servers are never routine noise.

## Steps

1. Confirm the vertical context: search_tickets for this client's history with the named app (dental desks generate repeat tickets with known local fixes — a bridge re-link, a service restart), and search_itglue / search_hudu for the PMS/imaging documentation: server names, versions, vendor support contract and portal-credential *location*.
2. Set severity by the practice clock: whole-office PMS or imaging failure during patient hours (especially before 8:30 AM) → highest priority and immediate human dispatch; single-operatory sensor issue with other operatories working → normal priority with an honest workaround note ("use op 3 for x-rays meanwhile").
3. Run the LOB framework sequence (exact versions client and server, change correlation, verbatim error, scope) with the dental splits above: imaging problems get triaged as sensor/driver vs bridge vs imaging server *before* deep-diving; PMS problems check client/server version mismatch first.
4. Branch: environment-side (network to server, workstation drivers, USB, security-agent quarantining an unsigned sensor driver — a classic) is the desk's; anything inside the PMS or imaging *database* is vendor territory — build the vendor-escalation package per the framework, using the practice's support contract from docs, and log the case number.
5. Note in plain text: app + exact versions, scope, practice-clock impact, error verbatim (PHI-scrubbed), branch taken, vendor case number if any, and verification — the *user* re-runs the real workflow (open the schedule, capture an image), not just app-opens.

## Guardrails

- Never operate directly on the PMS or imaging database (repair, compact, SQL surgery) outside vendor-documented procedure — patient charts and support standing are both at stake; the vendor package is the deliverable.
- No PHI in tickets beyond minimum necessary; no PMS screenshots; suspected exposure → facts only, flag the compliance owner. No legal or breach-notification advice.
- Never pronounce a sensor dead without a swap test — it is an expensive accusation.
- Confirm the imaging database is in backup scope any time you touch backup config for a dental client; do not assume the PMS backup covers it.
- Sensor/imaging drivers are vendor-specific: install per vendor documentation, and route security-agent exclusion requests to the security policy owner rather than adding them ad hoc.
- Docs tools vary per tenant — state what you could not verify, and flag missing PMS/imaging documentation (no versions, no vendor contact) as its own follow-up.
