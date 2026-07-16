---
name: Supporting Credit Unions
description: Vertical pack for credit-union clients — the core-banking-adjacent environment (Symitar, Corelation Keystone, Fiserv/Jack Henry-class cores the desk supports around, not inside), NCUA/FFIEC examination awareness, member-data sanctity (GLBA), branch and ATM/ITM uptime, and heavy change control. Load when the client is a credit union or community bank, or the ticket touches core banking, branches, ATMs, or an examiner request.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Credit Unions

A credit union is a federally examined financial institution: it answers to the NCUA (or a state regulator) under FFIEC guidance, it lives and dies on member trust, and its core banking system is almost always run by a specialized provider the MSP supports *around* — network, endpoints, branches, identity — not *inside*. The defining traits are regulatory weight and change discipline: nothing moves without a change record, and member data is sacred. This pack layers credit-union knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a credit union, community bank, or small financial institution, or the ticket names a core banking platform (Symitar/Episys, Corelation Keystone, Fiserv DNA/Portico, Jack Henry, Finastra-class), a teller/branch platform, or online/mobile banking
- Branch connectivity, teller-workstation, ATM/ITM, or cash-recycler network issues
- Anything referencing NCUA/FFIEC examination, an examiner request, an IT audit, vendor-management/due-diligence, or a change-control requirement
- Member-facing digital banking, card-processing, or bill-pay outages

## The stack you'll meet — and the core boundary

- **Core banking (support around, not inside):** Symitar/Episys, Corelation Keystone, Fiserv (DNA/Portico), Jack Henry — the system of record for accounts, transactions, and members. It is typically hosted or run by the core provider under a heavily contracted relationship. The desk's job is the environment the core depends on (network, VPN/circuits to the core, endpoints, print) and coordinating with the core provider — **not** operating inside the core database. Draw that line explicitly.
- **Branch and teller:** teller-line workstations, receipt/check scanners (remote-deposit and branch capture), cash recyclers/dispensers, and member-facing terminals. A branch that can't reach the core cannot serve members at the counter — a branch-down during banking hours is high severity.
- **ATM/ITM network:** ATMs and interactive teller machines depend on network and a processor connection; outages are member-visible and often SLA- and regulator-relevant.
- **Digital banking and cards:** online/mobile banking, card processing/switch, bill-pay, and their vendors — outages here are member-facing and frequently vendor-side; distinguish member internet vs credit-union edge vs processor.
- **Adjacent:** the identity tenant, imaging/document systems (loan and member docs), and the security stack the examiners scrutinize.

## The compliance regime: NCUA/FFIEC, GLBA, and vendor management

- **Examinations are a fact of life.** Credit unions undergo NCUA/state exams under FFIEC guidance; the MSP is often a critical vendor subject to due-diligence review, and IT controls (access, MFA, patching, backups, logging, incident response, BCP/DR) are examined. Know whether this client has an examination cadence and where its IT policies live — work that touches controls should be consistent with them, and gaps get flagged to the client's information-security officer (ISO)/compliance owner, not improvised around.
- **Examiner and audit requests are not routine tickets.** If a ticket is fulfilling an examiner or auditor request (evidence, logs, configuration attestations), treat it as high-priority, factual, and documented — provide what is asked, accurately, and route anything requiring an attestation to the client's ISO. Never fabricate or estimate evidence.
- **Member data is sacred (GLBA).** Account numbers, SSNs, balances, and member PII must never land in tickets or screenshots — reference a member by internal account/member ID only, minimum necessary. Suspected exposure or a member-data incident → contain per the incident process, record facts, and flag the ISO/compliance owner immediately; regulatory notifications (including any NCUA-required reporting) are theirs to make. No legal or regulatory advice.

## The rhythm and what's sacred

- **Heavy change control.** Financial institutions run formal change management — changes to anything touching the core, network, or member-facing systems typically require a change record, approval, and a maintenance window, and often after-hours execution. Do not make discretionary changes during banking hours; propose them through the client's change process. This is a defining habit of the vertical.
- **Banking hours and cutoffs.** Branch hours (and Saturday morning branches) are peak; day-end/nightly core processing and settlement cutoffs are load-bearing windows — a problem that threatens end-of-day processing is urgent.
- **Sacred systems:** the connection to the core (circuits/VPN), member-facing digital banking and card processing during member hours, branch/ATM uptime, and backups and logging (the examiners look at both). Backup- and log-integrity alerts at a credit union are never routine noise.

## Steps

1. Establish context: search_tickets for this client's history (branch-connectivity and core-VPN issues recur with known fixes), and search_itglue / search_hudu for documentation — core provider and support contacts, circuits/VPN to the core, branch/ATM inventory, change-control process, examination cadence, and ISO/compliance contact.
2. Draw the core boundary immediately: is this inside the core (provider territory — coordinate, do not operate) or in the environment the core depends on (the desk's)? Say which.
3. Check the change-control gate before any change: does this require a change record, approval, and a window? If yes and it's not an emergency, route it through the client's process rather than executing ad hoc.
4. Triage by member impact and hours: branch- or member-facing outage during banking hours, or anything threatening end-of-day core processing → top severity and immediate coordination; single back-office user off-peak → normal.
5. Run the LOB framework with financial-institution splits: for member-facing digital/card issues separate member-side vs credit-union edge vs processor/vendor status; for branch issues separate workstation vs branch network vs circuit-to-core; verbatim errors, vendor status pages early, vendor-escalation package with member-impact and any SLA/examiner context stated.
6. Note in plain text, member-data-scrubbed: system, core-boundary determination, change-control status, member/branch impact, error verbatim, vendor/provider case, compliance flag if a control gap or examiner request surfaced, and verification by the appropriate staff running the real workflow.

## Guardrails

- Support around the core, not inside it: never operate on the core banking database or member records outside the provider's documented procedure and the client's authorization — coordinate with the core provider.
- Heavy change control is the norm: no discretionary changes during banking hours; route changes through the client's change-management process with a record, approval, and window. When in doubt, defer.
- No member data in tickets — no account numbers, SSNs, balances, or member PII; reference members by internal ID, minimum necessary.
- Examiner/auditor requests are high-priority, factual, and never fabricated; attestations go to the client's ISO. Suspected member-data incident → contain, record facts, flag the ISO/compliance owner at once; regulatory notification is theirs.
- Security controls (access, MFA, patching, backups, logging, DR) are examined — changes are checked against the client's IT policies and flagged to the ISO when they diverge; the MSP is likely a reviewed vendor.
- Confirm backups and logging are in scope and intact any time you touch them — both are examination evidence.
- Docs tools vary per tenant — state what you could not verify; a credit union with no documented change process, examination cadence, or ISO contact is itself a flag for the account owner.
