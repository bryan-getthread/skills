---
name: Supporting Real Estate Clients
description: Vertical pack for real-estate brokerage and title/escrow clients — transaction platforms (Dotloop, SkySlope-class), MLS and lockbox systems, the heightened wire-fraud/BEC threat model (this is the #1 targeted vertical), and agent BYOD sprawl. Load when the client is a brokerage, team, or title company, or the ticket mentions closings, wires, MLS, or agent devices.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Real Estate Clients

Real estate is where six-figure wire transfers are arranged over email between parties who have never met — which is why it is the most-targeted vertical for business email compromise, full stop. The IT estate is loose by design: commission-based agents on personal devices and personal email, a brokerage core the broker actually controls, and closings that cannot move. This pack layers real-estate knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework), with the fraud posture first because it outranks everything else.

## When to use

- The client is a residential/commercial brokerage, agent team, property manager, or title/escrow company, or the ticket names Dotloop, SkySlope, DocuSign (Rooms), Lone Wolf, kvCORE, Follow Up Boss, MLS access, or Supra/SentriLock lockboxes
- ANY ticket mentioning wires, wiring instructions, closing funds, earnest money, or changed payment details — treat as a potential incident, not a support request
- Suspicious email reports, mailbox-rule oddities, or spoofed-agent complaints at a real-estate client
- Agent onboarding/offboarding or BYOD questions

## The threat model: wire fraud is the vertical

- **Why here:** every transaction has a predictable, public timeline (listings are public record), large one-time wires, emotionally-invested first-time participants, and email threads spanning agents, lenders, title, and buyers — perfect BEC conditions. Attackers compromise or spoof an agent/title mailbox, watch until closing nears, then send "updated wiring instructions." The money is usually unrecoverable within days.
- **Standing rules for the desk:**
  - Any email about *changed* wiring instructions, any report of clients receiving payment instructions, or any compromised-mailbox suspicion at a real-estate client is a **security incident** — run security/vendor-fraud-bec-alert immediately; do not treat it as a routine phishing question.
  - If funds may already have moved, minutes matter: the client must contact their bank's fraud department and law enforcement (IC3) *immediately* — surface this to the client's principal at once; the desk's role is speed and evidence preservation, not investigation theater.
  - Mailbox-compromise indicators (new inbox rules auto-forwarding or deleting, unfamiliar sign-ins, sent items the agent denies) get the full containment runbook, and *everyone in active transactions with that mailbox* is potentially exposed — flag scope to the broker.
  - Push the preventive posture whenever touching email at these clients: MFA everywhere without exception, and support the industry-standard warning ("we will never change wiring instructions by email — verify by phone at a known number") in signatures/templates when the broker adopts it.

## The stack you'll meet

- **Transaction management:** Dotloop, SkySlope, DocuSign Rooms, Lone Wolf Transactions-class — offers, disclosures, signatures, deadlines live here. A signature or document failure the day of closing is a top-severity event; ask "when does this transaction close?" during triage.
- **MLS and lockboxes:** MLS access runs through regional associations with their own logins/SSO (many issues are association-side — recognize when to send the agent to the MLS help desk, and say so plainly); Supra eKEY and SentriLock lockbox apps live on agent phones — Bluetooth, app-auth, and phone-update breakage are the recurring tickets, and a broken key app means an agent standing outside a showing.
- **CRM/lead-gen:** kvCORE, Follow Up Boss, BoomTown-class; website/IDX feeds. Lead-routing failures are revenue leaks the broker wants flagged fast.
- **Brokerage core:** back-office/commission accounting (Lone Wolf Back Office-class), the brokerage's email/identity tenant, office printers/scanners for the paper that still exists, and — for title/escrow clients — closing-production software with bank-grade sensitivity.

## BYOD sprawl — the reality

Agents are typically independent contractors on personal phones and laptops, often with personal Gmail addresses in professional use, and they resist heavy management. The workable posture: the brokerage tenant gets identity discipline (MFA enforced, brokerage email for transaction business where the broker mandates it), agent BYOD gets *lightweight* controls (MDM for brokerage data, screen lock, ability to revoke brokerage access) rather than full management, and offboarding an agent means revoking tenant access, transaction-platform access, MLS/lockbox association access, and lead-routing — agents leave for competitors with their pipeline, so offboarding is same-day work at the broker's direction. Support scope for personal-device issues beyond brokerage services is the broker's call; know where the line is documented.

## The rhythm and what's sacred

- Closings cluster at month-end; Fridays are the heaviest closing day. Month-end Friday is this vertical's peak-fragility window — no discretionary changes to email, transaction platforms, or e-signature paths then.
- Weekend showings mean lockbox/eKEY and phone issues spike Saturday morning with real urgency (buyers standing at the door).
- **Sacred:** the integrity of the email path (because of the threat model), the transaction platform and e-signature flow near closings, MLS/lockbox access during showing hours, and — at title/escrow clients — everything, treated at bank-grade.

## Steps

1. Fraud screen first, always: does the ticket involve wires, changed payment details, unexpected client emails about funds, or mailbox oddities? If yes → security/vendor-fraud-bec-alert path immediately, principal notified, evidence preserved (headers, rules, sign-in logs), and bank/IC3 urgency stated to the client if funds may have moved.
2. Context: search_tickets for this brokerage + system history, and search_itglue / search_hudu for the stack documentation: tenant, transaction platform, MLS association(s), lockbox vendor, BYOD policy and support-scope line, broker's designated approver.
3. Triage on the transaction clock: closing-day document/signature failures and showing-hour lockbox failures → top severity; ask "when does this close?" and "is a client waiting at a property?"
4. Route association-side problems honestly: MLS logins, association SSO, and Supra/SentriLock account states are frequently only fixable by the association/vendor — identify the boundary quickly, hand the agent the right contact, and note it rather than burning hours on the unfixable.
5. Run the LOB framework for platform failures: exact app/plan versions, change correlation (phone OS updates breaking eKEY apps is the classic), verbatim error, vendor status pages early (cloud-heavy vertical), vendor-escalation package with case number when it's vendor territory — closing date stated in the case.
6. For onboarding/offboarding: follow the broker-directed checklist across tenant, transaction platform, MLS, lockbox, CRM/lead routing — same-day for departures; record each revocation in the ticket.
7. Note in plain text, client-financial-details-scrubbed: system, transaction-clock context, fraud-screen result, error verbatim, boundary handoffs, vendor case, verification by the agent performing the real workflow.

## Guardrails

- Wire/payment-change tickets are incidents, never routine — run security/vendor-fraud-bec-alert, notify the principal, and preserve evidence before any cleanup. If funds may have moved, the bank-and-IC3 message goes to the client immediately.
- Never send, confirm, relay, or "verify" wiring instructions yourself, in any direction — the desk is not in the funds path, ever.
- No transaction financial details (amounts, account numbers, client identities paired with deals) in tickets; minimum necessary.
- Mailbox remediation at these clients always includes rules/forwarding audit and sign-in review — cleanup without scope assessment is malpractice here; flag exposure scope to the broker.
- Agent offboarding is same-day, broker-directed, and checklist-complete across all platforms; partial offboarding is an open door.
- Respect the documented BYOD support-scope line; personal-device work beyond it goes to the broker for a scope call, not quietly absorbed.
- No legal, regulatory, or funds-recovery advice — the compliance/legal moves belong to the broker, their bank, and law enforcement; the desk's move is speed, evidence, and escalation.
- Docs tools vary per tenant — state what you could not verify; a brokerage with no BEC-response contact or BYOD policy documented is itself a flag worth raising.
