---
name: Supporting Title & Escrow Companies
description: Vertical pack for title and escrow/settlement clients — closing/title-production software (SoftPro, RamQuest, Qualia-class), the extreme wire-fraud threat model (this vertical moves the actual closing funds), recording deadlines, and ALTA Best Practices. Load when the client is a title agency, escrow, or settlement company, or the ticket mentions closings, wires, recording, or title production. Cross-ref security/vendor-fraud-bec-alert.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Title & Escrow Companies

A title/escrow company is the party that actually holds and disburses the money at a real-estate closing — hundreds of thousands to millions of dollars per transaction, moving by wire on a fixed date. That makes it the single richest target in the wire-fraud ecosystem: real estate is the most-attacked vertical, and the settlement agent is where the funds live. Everything in this pack is downstream of one rule — the desk is never in the funds path, and any hint of wire manipulation is an incident before it is a support request. This pack layers title/escrow knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework) with the fraud posture first.

## When to use

- The client is a title agency, escrow company, settlement/closing firm, or an attorney closing office, or the ticket names SoftPro, RamQuest, Qualia, ResWare, TitlePoint, or a closing/settlement production system
- ANY ticket touching wires, wiring instructions, disbursement, closing funds, earnest money, or a change to payment/account details — treat as a potential incident, not a support request
- Suspicious email reports, mailbox-rule oddities, spoofed-closer complaints, or compromised-mailbox suspicion at a title/escrow client
- Recording, e-recording, or title-plant access problems near a closing or a recording deadline

## The threat model: wire fraud is existential here

- **Why the settlement agent is the target:** the closer holds the funds and sends the disbursement wires. Attackers compromise or spoof a closer's, lender's, or buyer's mailbox, watch the transaction timeline, and inject "updated wiring instructions" so payoff or seller-proceeds funds land in a mule account. At a title company the amounts are large and the window to claw back is measured in hours.
- **Standing rules for the desk:**
  - Any email about *changed* wiring or disbursement instructions, any report of a party receiving new payment details, or any compromised-mailbox suspicion at a title/escrow client is a **security incident** — run security/vendor-fraud-bec-alert immediately; do not treat it as a routine phishing question.
  - **The desk is never in the funds path.** Never send, confirm, relay, edit, or "verify" wiring or disbursement instructions in any direction, ever. Verification of wire details is the title company's own callback-to-a-known-number procedure — not something the desk performs or vouches for.
  - If funds may already have moved, minutes matter: the client must contact their bank's fraud/wire-recall desk and law enforcement (IC3) *immediately*, and initiate a SWIFT/wire recall — surface this to the client's principal at once. The desk's role is speed and evidence preservation (headers, mailbox rules, sign-in logs), not investigation theater.
  - Mailbox-compromise indicators (new auto-forward/delete rules, unfamiliar sign-ins, sent items the closer denies) get the full containment runbook, and *every open file that mailbox touched* is potentially exposed — flag scope to the principal.

## The stack you'll meet

- **Title/closing production:** SoftPro, RamQuest, Qualia, ResWare are the majors — order intake, title commitment, CD/settlement-statement (HUD/ALTA) preparation, escrow accounting, and disbursement live here. Qualia is cloud-class; SoftPro/RamQuest/ResWare are commonly on-prem or hosted. This is the system of record and it carries bank-grade sensitivity.
- **Escrow/trust accounting:** the escrow account is regulated trust money; reconciliation and positive-pay integrations with the bank are load-bearing. Never touch anything that alters escrow-accounting data outside vendor procedure.
- **Title search/examination:** title plants, TitlePoint/DataTrace-class search services, county recorder portals, and underwriter portals (each underwriter with its own login/MFA). Many search and remittance issues are underwriter- or county-side — recognize the boundary.
- **Recording:** e-recording services (Simplifile/CSC-class) and county recorder systems. A recording that misses a deadline has legal-priority consequences.
- **Adjacent:** e-signature/RON (remote online notarization) platforms, the email/identity tenant (the fraud attack surface), and lender-connection portals.

## The rhythm and what's sacred

- **Closings are date-certain.** A closing scheduled for a given day cannot slip without cascading consequences (rate locks, moves, chained transactions). Closing-day production, disbursement, and e-signature/RON failures are top severity, and the timing is externally fixed.
- **Recording deadlines and the "gap."** Documents must record promptly after closing; the period between closing and recording (the gap) carries risk, so e-recording/recorder access near a closing is load-bearing. Month-end and end-of-year cl-close volume spikes.
- **Sacred:** the integrity of the email path (because of the threat model), the escrow/trust accounting data, the closing-production database and its backups, and recording connectivity around a closing. At a title company, treat essentially everything at bank-grade.

## Steps

1. Fraud screen first, always: does the ticket involve wires, changed payment/disbursement details, unexpected party emails about funds, or mailbox oddities? If yes → security/vendor-fraud-bec-alert path immediately, principal notified, evidence preserved, and the bank-recall/IC3 urgency stated to the client if funds may have moved.
2. Context: search_tickets for this client + system history, and search_itglue / search_hudu for the stack documentation — closing platform and version/hosting, underwriter portals, e-recording service, escrow-bank integrations, ALTA Best Practices / WISP location, and the designated approver.
3. Triage on the closing clock: closing-day production, disbursement, RON/e-sign, or recording-deadline failures → top severity, immediate human dispatch; ask "is a closing happening today?" and "is there a recording deadline?"
4. Route underwriter/county-side problems honestly: title-plant, county recorder, and underwriter-portal issues are frequently only fixable by that party — identify the boundary and hand off, noting it, rather than burning hours.
5. Run the LOB framework for production failures: exact app/version, change correlation, verbatim error, vendor status pages early, vendor-escalation package with the closing/recording date stated — never operate on escrow-accounting or closing databases outside vendor procedure.
6. Note in plain text, financial-details-scrubbed: system, closing-clock context, fraud-screen result, error verbatim, boundary handoffs, vendor case, verification by the closer running the real workflow.

## Guardrails

- Wire/disbursement-change tickets are incidents, never routine — run security/vendor-fraud-bec-alert, notify the principal, preserve evidence before any cleanup. If funds may have moved, the bank-recall-and-IC3 message goes to the client immediately.
- The desk is NEVER in the funds path: never send, confirm, relay, edit, or verify wiring or disbursement instructions, in any direction, for any reason.
- Escrow/trust accounting is regulated money — never alter escrow-accounting data outside vendor-documented procedure; reconciliation and positive-pay integrations are the client's and bank's domain.
- No transaction financial details (amounts, account/routing numbers, party identities paired with a file) in tickets; bank-grade minimum necessary.
- Mailbox remediation always includes a rules/forwarding audit and sign-in review, and open-file exposure scope flagged to the principal — cleanup without scope assessment is malpractice here.
- Recording deadlines have legal-priority consequences — treat recorder/e-recording access near a closing as load-bearing and escalate misses immediately.
- No legal, regulatory, coverage, or funds-recovery advice — those moves belong to the title company, its underwriter, its bank, and law enforcement; the desk's move is speed, evidence, and escalation.
- Docs tools vary per tenant — state what you could not verify; a title client with no documented wire-fraud/BEC response contact or ALTA/WISP location is itself a flag for the account owner.
