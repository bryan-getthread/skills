---
name: Supporting Accounting Firms
description: Vertical pack for CPA and accounting-firm clients — the tax and engagement stack (Lacerte, ProSeries, Drake, UltraTax, CCH-class), the tax-season freeze-and-urgency regime, IRS Pub 4557 / FTC Safeguards WISP obligations, and client-portal sensitivity. Load when the client is an accounting/CPA firm or the ticket names tax software or a tax deadline.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Accounting Firms

An accounting firm has two calendars: the normal one, and the one where the entire year's revenue is earned between late January and April 15. The same ticket means different things on those calendars, and the firm's regulator has written IT security obligations directly into its playbook. This pack layers accounting-vertical knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a CPA/accounting/bookkeeping firm, or the ticket names Lacerte, ProSeries, Drake, UltraTax CS, CCH Axcess, ProSystem fx, ATX, TaxAct Professional, QuickBooks Desktop (hosted or local), or a client portal (ShareFile, SafeSend, TaxCaddy-class)
- Any change, maintenance, or project scheduling at an accounting client (tax-season freeze check)
- E-file rejections, "the tax program won't update," hosted-desktop slowness during season
- Questions touching the firm's WISP, IRS Pub 4557, or FTC Safeguards obligations

## The stack you'll meet

- **Tax preparation:** Lacerte and ProSeries (Intuit), Drake, UltraTax CS (Thomson Reuters), CCH Axcess (cloud) and ProSystem fx (on-prem) are the majors. On-prem ones are classic LOB: a shared network data path, per-workstation clients, and *constant in-season updates* (federal and state form releases) — client/data-path version mismatch after a partial update is the top repeat ticket. E-filing runs through the vendor's transmission service; distinguish "our network" from "vendor e-file service" from "IRS acceptance" when returns won't transmit.
- **Accounting & engagement:** QuickBooks Desktop (frequently on a hosting provider — Right Networks-class — making tickets session/latency problems), QuickBooks Online, engagement/workpaper tools (CCH Engagement, CaseWare-class), and document management (FileCabinet, Doc.It-class).
- **Client exchange:** portals (ShareFile, SafeSend, TaxCaddy-class) and e-signature (8879 signing). This surface carries SSNs and full financial lives — its security posture is the firm's reputation.

## The compliance regime: Pub 4557 and the WISP

- Tax preparers are subject to the **FTC Safeguards Rule**, and **IRS Publication 4557** is the practical guide: every firm must have a **Written Information Security Plan (WISP)**, and the MSP is very often named in it as the technical safeguard provider. Know whether this firm has a WISP and where it lives (documentation system) — work that touches security controls (MFA, encryption, access, retention, backups) should be consistent with it, and gaps get flagged to the firm's WISP/data-security owner, not silently improvised around.
- **Taxpayer data is the sensitivity bar:** never paste SSNs, return contents, or client financials into tickets; no screenshots of open returns. Reference clients by portal/account ID where a specific record is unavoidable.
- **Suspected data theft has an IRS dimension** (firms have reporting paths to the IRS and states). Your move on any suspected compromise of taxpayer data: contain per your incident process, record facts, and flag the firm's WISP/compliance owner immediately — the regulatory notifications are theirs to make. Also route EFIN/PTIN misuse suspicions the same way.

## The rhythm: tax season changes everything

- **Season (mid-January through April 15, again mid-August through Sept 15 / Oct 15 extensions):** the firm works nights and weekends; partners are in the office Sunday at 10 PM. Two regime changes apply:
  1. **Change freeze.** No discretionary maintenance, migrations, upgrades, or "quick improvements" to anything the firm touches during season. Emergencies only, with the firm's explicit sign-off. Schedule projects for May–July or late October–December.
  2. **Urgency floor rises.** A single preparer's workstation down in April is a same-hours problem; the tax application or hosted desktop down firm-wide in the first two weeks of April is an existential P1. Deadline days themselves (Mar 15, Apr 15, Sep 15, Oct 15) are maximum-alert days — pre-check the firm's core stack the morning of.
- **Off-season** is when everything deferred happens — and when the desk should proactively propose it, because the window is short.

## What's sacred

The tax application and its data path, the hosted-desktop environment if any, the client portal, e-file transmission, and backups of all of the above. In-season, the nightly backup of the tax data directory is the firm's disaster insurance — backup failures at an accounting client in March are never routine noise.

## Steps

1. Establish the calendar first: is it season? If yes, the freeze and the urgency floor both apply to everything below. Check search_tickets for this firm + app history (tax-software update failures and data-path locks recur annually with known fixes) and search_itglue / search_hudu for the stack documentation, hosting provider, vendor support contracts, and WISP location.
2. Triage by blast radius × calendar: firm-wide tax-app/hosting outage in season → top severity, immediate dispatch; single-user off-season → normal. Ask "when is your next filing deadline?" when impact is unclear.
3. Run the LOB framework with accounting splits: update problems → compare program version on the failing workstation vs the network data path vs a working workstation before anything else; e-file failures → separate local error vs vendor transmission status vs agency rejection code (rejection codes are the *firm's* to resolve — content problems, not IT problems; say so plainly); hosted-desktop tickets → gather session host, latency evidence, and the hosting provider's status before local surgery.
4. Branch per the framework: environment (network path permissions, workstation, printer/scanner for source docs, security-agent quarantining a freshly-updated tax binary — a seasonal classic) is the desk's; tax-data corruption, program defects, and hosting-platform faults are vendor territory — full escalation package, case number, follow-up cadence with the filing deadline stated in the case.
5. Note in plain text, taxpayer-data-scrubbed: app + exact versions (workstation and data path), scope, season/deadline context, verbatim error, branch, vendor case, WISP flag if a control gap surfaced, and verification by the preparer running the real workflow (open a return, run a test transmission per vendor procedure).

## Guardrails

- In-season change freeze: no discretionary changes mid-January–April 15 (and around extension deadlines) without the firm's explicit sign-off; when in doubt, defer.
- No taxpayer data in tickets — no SSNs, return excerpts, or client financial screenshots; minimum necessary always.
- Suspected taxpayer-data compromise or EFIN/PTIN misuse → contain, record facts, flag the firm's WISP/compliance owner at once; regulatory notifications are theirs. No legal or regulatory advice.
- Security-control changes (MFA, encryption, retention, access) at an accounting firm are checked against the WISP and flagged to its owner when they diverge — the MSP is likely named in that document.
- Never operate on the tax data path or program databases outside vendor procedure; never improvise a rollback of a mid-season form update — vendor guidance only.
- Rejection codes and return content are the firm's domain — the desk fixes transmission, not tax positions.
- Docs tools vary per tenant — state what you could not verify; an accounting client with no documented WISP location is itself a flag for the account owner.
