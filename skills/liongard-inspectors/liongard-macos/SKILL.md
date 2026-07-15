---
name: Liongard macOS Fleet Read
description: Answer Mac fleet questions from the Liongard macOS inspector — OS version spread, FileVault coverage, and local admin accounts — for the Apple slice of a client's endpoints.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_metric, liongard_device, liongard_detection, liongard_timeline, liongard_query, search_tickets, add_ticket_note]
---

# Liongard macOS Fleet Read

The Mac slice of a client's fleet is routinely under-instrumented — MDM coverage is partial, the RMM agent is Windows-first, and "the designers' Macs" live outside every report. Where the partner runs Liongard's macOS inspector, this skill reads what is actually deployed: macOS versions, FileVault status, and who is an admin on which machine.

## When to use

- "What macOS versions is <client> running?" / "any Macs on unsupported macOS?"
- "Is FileVault on across their Macs?" — encryption evidence for compliance or insurance
- "Who has admin on the Macs?" — local-admin audit for the Apple fleet
- Completing a fleet or QBR picture that Windows-only reads leave half-blind

## Steps

1. Base pattern (skills/liongard-inspectors/liongard-access-pattern/SKILL.md): liongard_launchpoint filtered by the macOS systemType — per-machine launchpoints via the agent. State coverage explicitly (inspected Macs vs. the client's believed Mac count from MDM/docs/tickets); Mac coverage gaps are the norm, so the coverage line matters even more than on Windows. Report per-machine dataprint staleness; long-silent inspectors on laptops often just mean the lid was shut — distinguish "stale" from "missing."
2. Route the question — liongard_device for the reconciled view, liongard_metric for depth:
   - OS spread → group by macOS version. Apple's practical support window is the current major version plus roughly two prior; verify the currently supported set against Apple's published releases rather than memory. Output: version → count → named machines, unsupported-first. Hardware too old for a supported macOS is a refresh finding — route to devices-and-infrastructure/mac-fleet-management and warranty-eol-report.
   - FileVault → per-machine encryption status. Unencrypted portables lead the findings; pair with devices-and-infrastructure/endpoint-encryption-audit for the remediation program. Note escrow: FileVault "on" without a recoverable key (MDM/docs) is a future lockout — flag where key escrow status is captured or documented.
   - Local admins → per-machine admin accounts. On Macs the primary user being an admin is common default reality, so the findings are relative: accounts admin on machines that aren't theirs, unexplained shared admin accounts, and drift from the client's documented standard (search_itglue/docs where available). Angle shape: `Users[?Admin==\`true\`].{user: Name}` — verify field paths against the live dataprint.
   - Changes → liongard_detection / liongard_timeline for new admin accounts, OS upgrades, FileVault state changes.
   - Open-ended → liongard_query, verified before reporting.
3. Cross-check unexplained admin accounts against tickets before flagging (the AV-install ticket, the migration ticket), same discipline as the Windows read.
4. Output: coverage statement, answer table, findings ranked (unencrypted portables and unexplained admins first), dataprint dates. Plain-text note on request.

## Guardrails

- Coverage honesty is the headline: "of the N Macs Liongard sees" — never let a partial read masquerade as the fleet.
- macOS support-window claims cite Apple's current release list, not remembered version numbers.
- Local-admin findings on Macs are calibrated to the client's standard, not to a Windows-style zero-admin ideal — flag drift and sprawl, not the platform's defaults.
- Read-only: FileVault enablement, admin demotion, and OS upgrades are technician/MDM work.
- Degradation: no macOS inspectors → MDM exports, docs, and tickets only, labeled "not instrumented"; a client with a real Mac population and no instrumentation is itself a finding for the QBR.
