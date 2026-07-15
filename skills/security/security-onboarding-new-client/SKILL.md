---
name: Security Onboarding New Client
description: A new client is being onboarded and their security baseline is unknown — run the intake: MFA coverage, admin inventory, backup posture, EDR presence — and produce the day-one risk list before the first incident finds the gaps for you.
category: Security
tools: [search_clients, search_contacts, search_tickets, search_ninjaone_devices, connectwise_rmm_search_devices, search_itglue, liongard_environment, liongard_identity, liongard_metric, add_ticket_note, create_ticket]
---

# Security Onboarding New Client

Every new client arrives with an inherited security posture nobody has honestly assessed. This skill runs the structured intake across the four load-bearing controls — MFA, admin access, backups, endpoint protection — and outputs a day-one risk list: what could hurt this client this week, ranked, with owners.

## When to use

- A new client signed and onboarding is underway (pair with the client-onboarding-runbook — this is its security chapter)
- Management asks "what are we inheriting?" about an incoming environment
- A client came over from another provider and their real posture needs verifying against what was claimed

## Steps

1. Establish sources: what the previous provider or client handed over (documentation exports, admin credentials via secure handoff), what tooling is connectable now (RMM agents, Liongard where deployed), and what must be answered by the client's staff. Record the as-of date and source for every fact gathered — inherited documentation is routinely stale, so verify rather than transcribe (liongard_environment / liongard_identity for live reads where available).
2. MFA coverage: percentage of user accounts with MFA enrolled and enforced (not just "capable"), the enforcement mechanism, and the exception list — service accounts, legacy-auth dependencies, and any "the owner doesn't like prompts" exemptions. Every exception is a day-one-risk-list candidate. Feed detailed follow-up to the identity-mfa-health-check skill.
3. Admin inventory: every privileged account across identity platform, devices, firewall, backup console, and line-of-business admin panels. Flag: admin accounts used for daily work, shared admin credentials, ex-employee or previous-provider access still live, and vendor accounts with standing access. Previous-provider access gets a dated transition plan — cutting it off mid-transition breaks things; leaving it indefinitely is an open door. Feed to global-admin-audit for the deep pass.
4. Backup posture: what is backed up (and — the real question — what ISN'T: cloud mailboxes and file shares are commonly assumed-covered and aren't), where copies live (any offsite/immutable copy?), and the last successful restore TEST, not the last successful backup job. "Never tested" goes on the risk list at high rank; unverified backups are hope, not posture.
5. EDR/endpoint protection presence: what product, on what percentage of the fleet (reconcile agent count against RMM inventory via search_ninjaone_devices / connectwise_rmm_search_devices — result-cap honesty on counts), who watches its alerts today (an unmonitored EDR console is a recorder, not a control), and unprotected classes (servers, Macs, that machine in the warehouse).
6. Quick environmental sweep for the classics: end-of-life OS in production, exposed remote-access (RDP/VPN with no MFA), credential spreadsheets (flag location, never contents — password-manager-rollout is the fix), and email-security basics (route the SPF/DMARC read to dmarc-spf-failure-triage territory).
7. Produce the day-one risk list — the deliverable: each entry as risk / evidence / exposure in plain terms / recommended action / suggested owner, ranked by exploitability-now rather than audit-category. Cap it at the ten that matter; a forty-line list is a document, not a plan. Create remediation tickets (create_ticket) for the top items the client approves, and hand the list to the account manager for the roadmap conversation (it-roadmap-builder / cyber-risk-posture-review for the ongoing cadence).

## Guardrails

- Verify, don't transcribe: inherited docs and previous-provider claims get checked against live reads or client confirmation; every finding carries its source and as-of date.
- Never present partial visibility as a clean bill — "no EDR gaps found among the 60% of devices reporting" is the honest sentence.
- Findings are stated neutrally (this is an intake, not an indictment of the previous provider) and factually per the defensive-writing-standard; the client may share this document.
- Credentials received in handoff go straight into the desk's credential store — never into tickets or the intake notes; credential spreadsheets found are flagged by location only.
- The risk list recommends; the client decides — remediation beyond the agreed onboarding scope is quoted work, flagged to the account manager, never silently absorbed or silently skipped.
- Degradation: without RMM/Liongard connectivity in early onboarding, the intake runs on exports and client interviews — mark every unverified item as unverified, and re-run the checks once tooling lands.
