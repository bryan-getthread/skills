---
name: Liongard QBR Evidence Pack
description: Assemble QBR-grade posture evidence for one client from multiple Liongard inspectors — identity risks, EOL exposure, cert and domain hygiene, and config drift — as a dated, sourced evidence pack the QBR deck can stand on.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_identity, liongard_domain, liongard_metric, liongard_detection, liongard_timeline, liongard_cyber_risk_dashboard, liongard_query, search_tickets, add_ticket_note]
---

# Liongard QBR Evidence Pack

QBRs die when the slides say "you should upgrade" and the client asks "based on what?" This skill builds the based-on-what: a per-client evidence pack pulled from every Liongard inspector the client has, organized into the four stories a QBR tells — identity risk, EOL exposure, domain/cert hygiene, and what changed this quarter. It feeds the Liongard section of account-management/qbr-and-sbr-prep; the meeting, commercial framing, and roadmap live there.

## When to use

- "Pull the Liongard evidence for <client>'s QBR" / prep for a quarterly or strategic business review
- account-management/qbr-and-sbr-prep or account-management/it-roadmap-builder needs instrumented posture data
- A vCIO wants defensible numbers behind a recommendation ("show me the EOL machines, not the claim")

## Steps

1. Inventory what evidence is even possible: liongard_launchpoint for ALL of the client's inspectors, applying the base trust rules (skills/liongard-inspectors/liongard-access-pattern/SKILL.md) to each. The inventory itself is slide one — what is instrumented, what last ran when, and what isn't watched at all (monitoring gaps are roadmap items). Every downstream number carries its dataprint date; anything older than the quarter gets marked stale.
2. Assemble the four evidence sections, each delegating depth to its specialist read and taking only headline findings:
   - Identity risk → liongard_identity plus the M365/AD/Google/Okta/Duo reads as applicable (liongard-m365-tenant, liongard-active-directory, liongard-google-workspace, liongard-okta, liongard-duo): admin counts vs. benchmark, MFA/2SV coverage with stated denominators, stale enabled accounts, bypass users. Where a full audit exists this quarter (security/global-admin-audit, security/identity-mfa-health-check), cite it rather than re-deriving.
   - EOL exposure → liongard-windows-workstations and liongard-macos fleet reads: OS build spread, machines past end-of-support (lifecycle dates verified, not remembered), coverage statements included. Reconcile with devices-and-infrastructure/warranty-eol-report where hardware data exists.
   - Domain & cert hygiene → liongard-internet-domain / liongard_domain: domain expiry and lock status, DMARC/SPF/DKIM presence, certs expiring next quarter.
   - Config drift → liongard_detection / liongard_timeline for the quarter: admin grants, policy loosenings, GPO/NSG/gateway changes — reconciled against search_tickets so the pack distinguishes "changes we made on tickets" (service narrative) from "changes with no ticket" (findings). liongard_cyber_risk_dashboard supplies the summary scoring where the partner licenses it.
3. Trend against the previous pack: search prior QBR notes/tickets for last quarter's numbers and state per-metric movement — improved / unchanged / degraded. A number without its trend is half a slide.
4. Rank findings across sections by client impact (exploitability × business consequence), identity usually leading, and select the top 3–5 as the pack's headline. Everything else is appendix rows.
5. Output the pack: instrumentation inventory (with gaps), four evidence sections (each finding = claim + evidence rows + dataprint date + trend), headline top-5, and a "recommended roadmap inputs" list handed to qbr-and-sbr-prep / it-roadmap-builder. Post as a plain-text note on the QBR-prep ticket on request; client-facing phrasing goes through security/defensive-writing-standard.

## Guardrails

- Every number carries a date and a source inspector; an undated number does not enter the pack.
- Coverage statements are non-negotiable — fleet and identity percentages name their denominators or they don't ship.
- Evidence, not conclusions: the pack says "14 machines past end-of-support (list attached)," the QBR skill decides how to sell the refresh. Never inflate findings to justify a deal.
- No fabricated trends — if last quarter's baseline doesn't exist, the trend column says "first measurement," and this pack becomes the baseline.
- Stale or failed inspectors are reported as gaps, not silently skipped — a pack built on three of nine inspectors says so up front.
- Degradation: little or no Liongard coverage → produce the instrumentation-gap report alone and hand qbr-and-sbr-prep that finding; do not pad the pack from memory or generic benchmarks.
