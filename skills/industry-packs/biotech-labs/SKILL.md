---
name: Supporting Biotech and Lab Clients
description: Vertical pack for biotech, life-science, and laboratory clients — instrument-control PCs vendor-locked on old OSes (the OT-boundary cousin), LIMS/ELN platforms, GxP and validated-system awareness (changes need change control), and sample-data sanctity including freezer/environmental monitoring. Load when the client is a lab or biotech firm, or the ticket names an instrument PC, LIMS, ELN, or freezer alarm.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Biotech and Lab Clients

A lab is manufacturing's cousin: alongside the office network sits a bench network of instruments driven by vendor-locked PCs, producing data that may be irreplaceable (months of experiments) or regulated (GxP records with audit trails). The instinct that gets desks in trouble here is treating an instrument PC like a normal endpoint — patching it, installing an agent, rebooting it mid-run — and silently invalidating a validated system or destroying a run. This pack layers lab knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework), starting with that boundary.

## When to use

- The client is a biotech, pharma-services, diagnostics, research, environmental-testing, or CRO lab, or the ticket names a LIMS (LabWare, LabVantage, Sample Manager-class), an ELN (Benchling-class), instrument software (Chromeleon, Empower, ChemStation-class), or "the instrument computer"
- Anything touching instrument-control PCs, their network segment, or data transfer off instruments
- Freezer, incubator, or environmental-monitoring alerts and their alerting paths
- Patching policy, security-agent rollout, or discovery-scan scoping at a lab client
- Tickets mentioning GxP, GLP/GMP, validation, or 21 CFR Part 11

## The instrument boundary — rule zero

- **Instrument-control PCs are the vendor's domain.** They are frequently pinned to old OS versions the instrument software is validated against, excluded from patching, and sometimes under a vendor service contract that voids if third parties modify them. No OS updates, agent installs, reboots, or "quick cleanups" on an instrument PC without the lab manager's and (where applicable) vendor's explicit sign-off — mid-run interruption can destroy an experiment worth months.
- **Runs happen unattended.** Instruments run overnight and over weekends; a reboot or network change at 6 PM Friday can kill a 48-hour run nobody mentions until Monday. Always ask "is anything running on this instrument, and when is the next idle window?"
- **Isolation over integration:** the sustainable posture is instrument segments isolated from the office network with controlled data-transfer paths (shared drop folder, one-way transfer), not full management. Discovery scans and vulnerability scans do not touch instrument segments without the lab manager scoping them — aggressive scans can crash old instrument PCs.

## GxP and validated systems

Where the lab operates under GxP (GLP/GMP/GCP) or supports regulated submissions, key systems are *validated*: their configuration is documented and controlled, and 21 CFR Part 11-class audit trails govern electronic records.

- Changes to validated systems (LIMS, instrument software, sometimes even the OS beneath them) go through the lab's change-control process — the desk proposes and schedules, the quality owner approves. An undocumented "helpful fix" on a validated system creates an audit finding.
- Never edit, delete, back-date, or reconstruct data in LIMS/ELN/instrument records; audit-trail integrity is the product. Data corrections are lab workflows with their own trail.
- Not every lab is GxP — research-stage labs may be informal. Establish which regime applies from docs or the lab manager before assuming either way.

## The rhythm and what's sacred

- **Sacred above all: sample integrity.** Freezer (-80s, LN2), incubator, and environmental-monitoring alarms are top severity at any hour — a failed -80 freezer can destroy years of samples in hours. The monitoring/alerting path (sensors, gateway, notification chain) is life-safety-grade infrastructure for the science; test it after any change to networks, power, or notification routes.
- **Sacred:** instrument data on acquisition PCs before it transfers (often the only copy — confirm transfer/backup coverage rather than assuming), the LIMS database, and the monitoring chain.
- **Rhythm:** unattended overnight/weekend runs; maintenance windows negotiated per instrument with the lab manager; grant/audit/submission deadlines create crunch periods worth asking about.

## Steps

1. Context: search_tickets for this lab's history with the named system/instrument, and search_itglue / search_hudu for the instrument inventory (vendor, service contract, validated-or-not, OS pinning), LIMS/ELN details, network segmentation, monitoring vendor and alert chain, and the change-control contact.
2. Classify the ticket's zone first: office IT (normal MSP work) vs instrument boundary (lab manager + vendor coordination) vs validated system (change control) vs monitoring/sample-safety (top severity, immediate). State the zone in the ticket.
3. Monitoring alarms: verify whether it's a real environmental excursion or a monitoring-path failure — both are urgent (a dead monitoring path means the next real excursion is silent). Confirm someone on the lab side has physical eyes on the asset before treating it as IT-only.
4. Instrument-PC issues: confirm nothing is running, get the lab manager's window, check the vendor contract terms in docs, and prefer the vendor-escalation package (per the LOB framework) for anything inside the instrument software. Data on the acquisition PC gets copied off (per the lab's transfer procedure) before risky steps.
5. LIMS/ELN issues: run the LOB framework — exact versions, change correlation, verbatim error, scope; database-side problems are vendor territory with change-control awareness; record the case number.
6. Note in plain text: system/instrument + versions, zone classification, run-status confirmation, lab-clock impact, error verbatim, approvals obtained (lab manager / quality owner / vendor), vendor case, and verification — the scientist runs the real workflow (acquire, transfer, log a sample).

## Guardrails

- Never patch, reboot, scan, or install agents on instrument-control PCs without lab-manager sign-off and a confirmed idle window; vendor-contract terms checked first.
- Never modify a validated system outside the lab's change-control process; never touch data or audit trails in LIMS/ELN/instrument records.
- Freezer/environmental alarms are never noise — top severity, physical-eyes confirmation, and monitoring-path verification every time.
- Assume instrument-PC data may be the only copy until transfer/backup coverage is confirmed in docs — preserve before acting.
- Discovery and vulnerability scans exclude instrument segments unless explicitly scoped with the lab manager.
- Suspected regulated-data exposure or integrity problem → facts only, flag the lab's quality/compliance owner; no regulatory advice from the desk.
- Docs tools vary per tenant — state what you could not verify; an undocumented instrument inventory or unclear GxP status is a follow-up worth flagging.
