---
name: Tabletop Exercise Prep
description: A client (or the MSP itself) wants an incident-response tabletop exercise — build a scenario pack grounded in their real environment and sanitized real incident history, with injects, decision points, and an evaluation sheet.
category: Security
tools: [search_tickets, search_clients, search_itglue, search_knowledge_base, add_ticket_note, create_ticket]
---

# Tabletop Exercise Prep

Generic tabletop scenarios test nothing. This skill builds the exercise from what the client actually runs and what has actually happened to them — a scenario pack with timed injects, decision points mapped to their real IR plan, and a debrief sheet that turns gaps into tickets.

## When to use

- A client's compliance or insurance requirement calls for an annual IR tabletop
- Management wants to test a client's (or the desk's own) incident readiness
- After a real incident, to exercise the fixed process before the next one

## Steps

1. Ground in the real environment: pull the client's stack from documentation (search_itglue / search_knowledge_base) — identity platform, backup product, EDR, key line-of-business systems, and their documented IR plan and escalation contacts. The scenario must name the systems the participants actually operate; "your SIEM" means nothing to a team that would actually be staring at their real console.
2. Mine sanitized history: search_tickets for the client's past security incidents and near-misses (phishing that got clicked, an account containment, a backup failure during a scare). Real history makes the best injects — but sanitize completely: no real usernames, no real senders, no credentials, no verbatim ticket text. "An accounting user reported entering credentials on a lookalike login page" — never the person.
3. Pick one primary scenario matched to their weakest tested area (common picks: ransomware detonation from a phished endpoint; BEC wire-fraud attempt; compromised admin account; critical-vendor 0-day). One scenario done deeply beats three done shallowly.
4. Build the pack:
   - Scenario narrative: opening situation as it would actually present (the alert text, the user's report).
   - Timed injects (5–8): escalating developments, each forcing a decision — "backups won't mount," "a journalist calls," "the insurer asks for the timeline." Include at least one communications inject and one "the plan's named contact is unreachable" inject.
   - Decision points keyed to their IR plan: who declares, who engages the insurer/IR firm, who talks to the client's leadership, when defensive-writing rules apply.
   - Facilitator notes: expected-answer guide drawn from the desk's runbooks (ransomware-response, compromised-account-containment, data-breach-notification-support).
   - Evaluation sheet: for each decision point — decided by whom, in what time, matching the plan or improvised.
5. Logistics: scope (client-only, or joint MSP+client — joint is usually the point, the handoffs are where incidents die), duration (90–120 minutes), and roles (facilitator, note-taker, participants). This skill preps the pack; a human facilitates.
6. Debrief to action: gaps observed become tickets (create_ticket) — plan updates, missing contacts, tooling gaps, training needs — each with an owner. An exercise that produces no follow-up items was either perfect or unexamined; say which. Record the exercise summary as a plain-text note for the client's compliance evidence.

## Guardrails

- Sanitization is absolute in the pack: no real people, credentials, client-identifying incident details, or verbatim ticket content — the pack will be projected on a wall and possibly shared with auditors.
- Tabletop only: no live systems touched, nothing detonated, no "surprise" real-system injects. Simulated pressure, real decisions.
- The scenario tests the plan, not the people: debrief findings are about process gaps, never individual blame — same no-shame posture as the phishing-simulation-program.
- Don't leak the injects to participants in advance; do brief them that it's an exercise (unannounced tabletops create real panic and real notifications).
- Findings that reveal actual current vulnerabilities (e.g. "we realized nobody CAN restore backups") get real tickets at real urgency, not just a line in the debrief.
- Degradation: without documentation tools, gather the environment facts from the client sponsor via a short intake — don't build on assumptions.
