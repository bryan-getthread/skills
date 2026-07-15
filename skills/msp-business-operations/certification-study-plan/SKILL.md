---
name: Certification Study Plan
description: Build a technician a per-track certification plan — CompTIA, Microsoft, networking, or security — with a study cadence that survives shift work, voucher and renewal tracking, and mapping to the skills this desk actually bills for.
category: MSP Business Operations
tools: [search_tickets, search_members, search_knowledge_base, web_search, create_ticket, add_ticket_note]
---

# Certification Study Plan

Turns "I want to get certified" into a plan a working tech can actually follow: the right cert for their track and this desk's client stack, a weekly cadence that respects on-call and shift patterns, exam-voucher and renewal dates tracked as tickets so they don't silently lapse, and an honest line between what the cert teaches and what the desk needs.

## When to use

- "I want to go for Security+ / AZ-104 / CCNA — build me a study plan."
- A manager budgeting the team's certification goals for the year.
- "My cert expires in a few months — what does renewal take?"
- Choosing between tracks: "network or security next?"

## Steps

1. Establish the starting point: current role and level, existing certs, the target cert (or the goal if they haven't picked one — "I want to move toward security work"), and their shift pattern including on-call weeks.
2. If the cert isn't chosen, recommend from track logic — and ground it in this desk's demand: skim the tech's own resolved tickets and the desk's volume by technology (search_tickets) to see what they touch daily. Typical ladders, stated as guidance not gospel:
   - **Foundation:** CompTIA A+ → Network+ → Security+ (the classic desk spine).
   - **Microsoft/cloud track:** MS-900 fundamentals → MS-102 (M365 admin) → AZ-104 (Azure admin) for desks heavy in M365/Azure tenants.
   - **Network track:** Network+ → CCNA (or vendor-matched: whatever firewall/switch line the desk's clients actually run).
   - **Security track:** Security+ → vendor EDR/SIEM certs matching the desk's security stack → CySA+/SSCP territory.
   A cert that matches the client stack turns into billable capability; one that doesn't is personal development (still fine — just name which it is).
3. **Verify before planning** (web_search): current exam code/version, objectives, price, and format for the target cert. Exam versions roll over regularly — never build a plan from memory of an exam that may have retired. Cite what you verified and the date.
4. Build the cadence around their real schedule:
   - Default rhythm: 3–5 hours/week in 45–60 minute blocks beats weekend cramming; plan 8–12 weeks for a foundation cert, longer for CCNA-class exams.
   - On-call weeks are half-load or zero-load weeks by design — a plan that ignores on-call gets abandoned in week two. Same for shift rotation: anchor study blocks to the stable parts of their week.
   - Structure per week: objectives-domain reading + one hands-on lab + a short recall/practice-question session. Book the exam once practice scores hold steady above the pass threshold, not before.
5. Set up the tracking scaffolding as tickets on the internal board (create_ticket): a voucher-purchase task (with approval routed per the tenant's training-budget process), milestone check-ins every 2–3 weeks, the exam date, and — the one everyone forgets — the renewal/CE deadline ticket dated well before expiry with the renewal requirements noted.
6. Map cert domains to desk work as you go: each study block should name one live connection ("this week's identity domain = the MFA-reset flow you ran twice yesterday"). It aids retention and shows the manager the desk-relevant payoff.

## Guardrails

- Exam details (codes, versions, prices, objectives, renewal rules) are verified against the vendor's current pages at planning time and marked with the verification date — never asserted from memory as current.
- No promises attached: a cert plan is development support, not a raise/promotion commitment and not a performance obligation. Slipped weeks are replanned without judgment — the plan serves the tech.
- Whether the employer pays for vouchers/renewals is a policy + approval question — route it (per the expense-ticket-handling process), never assume it.
- Study progress is between the tech and (if they choose) their manager — not team-visible, not a leaderboard.
- Recommend official objectives and reputable prep sources generically; don't guarantee any third-party course's quality or current accuracy.
- Cross-reference: skills-matrix-builder for the team-level view of which certs close real capability gaps; career-path-brief for how a cert fits a longer arc.
