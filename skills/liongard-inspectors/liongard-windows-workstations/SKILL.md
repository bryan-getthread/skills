---
name: Liongard Windows Workstation Fleet Read
description: Answer Windows fleet questions from the Liongard Windows inspector dataprints — OS build spread, local-admin sprawl, BitLocker status, and end-of-life flags — across a client's workstations without touching the RMM.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_metric, liongard_device, liongard_detection, liongard_timeline, liongard_query, search_tickets, add_ticket_note]
---

# Liongard Windows Workstation Fleet Read

Fleet-level reads over the Liongard Windows inspector: which OS builds are actually deployed, who is a local administrator where, which drives are encrypted, and which machines are past end-of-life. The RMM sees the same machines, but Liongard's dataprint history answers "when did that change" and works even when the partner's RMM isn't one Super Magic integrates with.

## When to use

- "What Windows versions are deployed at <client>?" / "EOL machines for this customer" — Windows 10 is past end-of-support, so this question is now a standing one
- "Which machines have extra local admins?" — local-admin sprawl audit
- "Is BitLocker on everywhere?" / encryption evidence for compliance or insurance
- Build-spread evidence for patch-compliance conversations or a zero-day exposure check

## Steps

1. Base pattern (skills/liongard-inspectors/liongard-access-pattern/SKILL.md): liongard_launchpoint filtered by the Windows systemType for the client — workstation inspectors run per-machine via the agent, so expect many launchpoints. Count them and compare against the client's known endpoint count (RMM or docs): inspecting 40 of 120 machines is a coverage statement that must lead the output. Note per-machine last-run times; report the staleness range, and treat machines whose inspector hasn't run in weeks as their own finding (agent dead, machine dead, or machine gone).
2. Route the question — liongard_device for the reconciled device view, liongard_metric per dataprint for depth:
   - OS build spread → group by OS caption and build number. Anything on Windows 10 or older is EOL exposure (verify current end-of-support dates against Microsoft's published lifecycle rather than memory); Windows 11 builds past their servicing window count too. Output: build → machine count → named machines. Feed the roadmap side to devices-and-infrastructure/win11-readiness-assessment and warranty-eol-report; client-facing EOL messaging goes through communication/eol-product-notice.
   - Local admins → per-machine local Administrators group membership. Expected members: the MSP's management account and documented exceptions. Findings, ranked: domain users as local admin on their own machine (sprawl), the same unexplained account admin on MANY machines (lateral-movement toolkit — escalate to security review), and machines drifting from the documented baseline. Angle shape: `LocalGroups[?Name=='Administrators'].Members[]` — verify field paths against the live dataprint.
   - BitLocker → per-volume protection status; unencrypted OS volumes on portable machines lead the findings. Pair with devices-and-infrastructure/endpoint-encryption-audit for the remediation program; recovery-key retrieval is m365-administration/bitlocker-key-retrieval.
   - Change context → liongard_detection / liongard_timeline: new local admin appeared <date>, build changed, encryption turned off — dated changes turn audits into narratives.
   - Open-ended → liongard_query, verified with targeted reads.
3. Cross-check surprising local admins against search_tickets (the install ticket that needed temp admin and never got cleaned up is the usual boring explanation) before flagging as unexplained.
4. Output: coverage statement first (inspected/total, staleness range), then the answer table, findings ranked, dataprint dates. Plain-text ticket note on request.

## Guardrails

- Never present the inspected subset as the fleet — the coverage statement is mandatory, every time.
- EOL determinations cite the OS build and the lifecycle date source; do not declare EOL from memory of dates.
- A recurring unexplained local admin across machines escalates to security rather than sitting in a hygiene report.
- Read-only: admin removal, encryption enablement, and upgrades are technician/RMM work — hand off with the machine list.
- Degradation: no Windows inspectors → use RMM reads where that integration exists (NinjaOne/ConnectWise RMM tools), else tickets/docs, labeled accordingly.
