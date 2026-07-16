---
name: Supporting CPA & Tax Firms
description: Vertical pack for tax-focused CPA firms — the deep e-file-season surge, IRS filing-deadline mechanics (e-file rejections, EFIN/PTIN, transmission vs acceptance), client-portal document exchange (8879 signing, SafeSend/TaxCaddy-class), and the FTC Safeguards WISP. Sharper, tax-season-specific companion to accounting-firms — load when the client's pain is filing-season and IRS-deadline driven. Cross-ref accounting-firms.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting CPA & Tax Firms

This pack is the tax-season-sharp companion to the broader accounting-firms pack. Use accounting-firms for the general CPA stack and firm posture; use **this** pack when the firm's defining pain is the filing-season surge and the IRS deadline machine — the weeks where a rejected e-file at 11 PM on April 15 is a client-facing emergency, not a ticket. It layers on the LOB Application Framework (troubleshooting-playbooks/lob-application-framework) and assumes the accounting-firms baseline (WISP, Pub 4557, taxpayer-data sensitivity) rather than repeating it.

## When to use

- The client is a tax-preparation-heavy CPA or EA firm, or the ticket is filing-season or IRS-deadline driven: e-file rejections, transmission failures, EFIN/PTIN issues, deadline-day capacity, extension crunches
- Client-portal document exchange near a deadline: 8879 e-signature not completing, SafeSend/TaxCaddy/ShareFile upload/download failures, organizer distribution
- Season-surge capacity: hosted-desktop or network slowness under peak concurrent load in March–April
- Scheduling any change/maintenance at a tax firm (season-freeze check) — see accounting-firms for the full freeze posture

> If the firm is a general accounting/bookkeeping practice without a filing-season spike, use accounting-firms instead. If it's both, use both — this pack adds the e-file/deadline depth on top.

## What this pack adds beyond accounting-firms

### E-file mechanics — separate the three failure domains
E-file problems are where the desk earns its keep, and the single most useful move is naming *where* it failed:
- **Local / workstation** — the tax program errors before transmission (update state, data-path version mismatch, corrupt return locally, security agent quarantining a freshly-updated binary). This is the desk's environment work.
- **Vendor transmission service** — the return left the workstation but the vendor's e-file service is down/delayed (Intuit/Thomson Reuters/CCH/Drake transmission). Check the vendor status page early; this is vendor territory, not local surgery.
- **IRS/state acceptance (rejection codes)** — the return reached the agency and was *rejected* with a code (mismatched name/SSN, duplicate SSN already filed, dependent claimed elsewhere, prior-year AGI/PIN mismatch). **Rejection codes are the firm's to resolve — they are return-content problems, not IT problems.** Say so plainly and hand back; the desk fixes transmission, not tax positions.

### EFIN/PTIN and identity
E-filing depends on the firm's **EFIN** (firm-level e-file identifier) and preparers' **PTINs**. EFIN problems (a suspended or mismatched EFIN, IRS e-Services access issues) block *all* transmission and are urgent in-season — but EFIN/e-Services administration is the firm's IRS relationship, not something the desk operates; support the access path and escalate. Any suspicion of **EFIN/PTIN misuse or taxpayer-data theft** → contain, record facts, flag the firm's WISP/compliance owner at once (there is an IRS reporting dimension); regulatory notifications are the firm's. No legal or regulatory advice.

### Client-portal document exchange — the deadline bottleneck
Near a deadline the portal is the critical path: the client must e-sign Form **8879** before the preparer can transmit, and organizers/source documents flow both ways through SafeSend, TaxCaddy, ShareFile-class portals. A portal or 8879-signing failure on April 14 blocks the filing itself — treat portal issues in the final days as high severity, and distinguish client-side (their browser/link/email) from firm-side from portal-vendor status. This surface carries SSNs and full financial lives — no taxpayer data in tickets, no screenshots of returns or signed forms; reference by portal/account ID.

### Season surge — capacity, not just outages
In peak weeks the failure mode is often *slowness under concurrent load*, not a hard outage: hosted-desktop latency (Right Networks-class), network data-path contention, and print/scan queues for source docs. Gather session host, concurrency, and latency evidence; pre-check the firm's core stack the morning of each deadline day (Mar 15, Apr 15, Sep 15, Oct 15) — these are maximum-alert days.

## The rhythm and what's sacred

- **The surge is the whole game.** Mid-January–April 15, then extension crunches around Sep 15/Oct 15: the firm works nights and weekends, the change freeze is in force (per accounting-firms — no discretionary changes without explicit sign-off), and the urgency floor rises. Deadline days are existential.
- **Sacred:** the tax application and its data path, e-file transmission, the client portal and 8879 flow, EFIN/e-Services access, and backups of all of it. In-season backup-failure alerts are never routine noise.

## Steps

1. Establish the calendar: is it filing season or near a deadline? If yes, the accounting-firms freeze and elevated-urgency posture apply. search_tickets for this firm + app/e-file history (rejections and update failures recur annually with known fixes) and search_itglue / search_hudu for the stack, hosting provider, EFIN/e-Services notes, portal, and WISP location.
2. For any e-file failure, classify the domain first — local vs vendor transmission vs IRS/state rejection — before touching anything. Rejection codes hand back to the firm; transmission-service faults go to the vendor with status confirmed; local errors are the desk's.
3. Triage by blast radius × calendar: firm-wide tax-app/hosting/e-file/portal outage in the final deadline days → existential P1, immediate dispatch; single-user off-season → normal. Ask "when is your next deadline?" when unclear.
4. For portal/8879 issues near a deadline, isolate client-side vs firm-side vs portal-vendor status; treat final-days signing failures as blocking the filing.
5. Run the LOB framework with the tax splits above; build vendor-escalation packages with the filing deadline stated in the case. Note in plain text, taxpayer-data-scrubbed: app + versions (workstation and data path), e-file domain identified, deadline context, verbatim error/rejection code, WISP flag if a control gap surfaced, and verification by the preparer running the real workflow (open a return, run a test transmission per vendor procedure).

## Guardrails

- Classify e-file failures before acting — rejection codes and return content are the firm's domain (the desk fixes transmission, not tax positions); transmission-service faults are the vendor's; only local errors are the desk's environment work.
- In-season change freeze (per accounting-firms): no discretionary changes without the firm's explicit sign-off; when in doubt, defer.
- No taxpayer data in tickets — no SSNs, return excerpts, 8879 contents, or portal-document screenshots; reference by portal/account ID, minimum necessary.
- EFIN/e-Services administration is the firm's IRS relationship — support the access path and escalate; do not operate it. Suspected EFIN/PTIN misuse or taxpayer-data theft → contain, record facts, flag the WISP/compliance owner at once; notifications are the firm's. No legal or regulatory advice.
- Never operate on the tax data path or program databases outside vendor procedure; never improvise a rollback of a mid-season form update — vendor guidance only.
- Treat final-days portal/8879 failures as blocking the filing itself — high severity, isolate the domain fast.
- Docs tools vary per tenant — state what you could not verify; a tax firm with no documented WISP location, EFIN/e-Services notes, or portal configuration is itself a flag for the account owner. See accounting-firms for the full firm baseline.
