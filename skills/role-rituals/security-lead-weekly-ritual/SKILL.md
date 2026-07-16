---
name: Security Lead Weekly Ritual
description: A security lead's weekly runbook — alert-noise skim, noise-tuning candidates, open-incident review, and posture-change check — run when a security lead says "weekly security pass", "review the security week", or "how noisy were we".
category: Role Rituals
tools: [search_tickets, list_boards, liongard_cyber_risk_dashboard, liongard_detection]
---

# Security Lead Weekly Ritual

The security lead's weekly hour: measure the noise, tune the worst of it, make sure no incident is drifting, and check whether anything about the clients' posture actually changed.

## When to use

- "Run my security weekly" / "how noisy was the week" / "weekly security review."
- A fixed weekly slot, ideally the same day the SOC/alert stack reports land.
- After an alert-heavy week, to decide what gets tuned.

## Steps

1. **Noise skim (15 min)** — run `skills/reporting-and-analytics/alert-noise-assessment` for the week: alert volume by source, true-positive rate, the top repeat offenders.
2. **Tuning candidates (15 min)** — feed the worst offenders into `skills/security/security-noise-tuning`: pick at most TWO tuning changes this week (suppression, threshold, dedupe rule). Tuning proposals include the evidence and the risk of the tune — a suppressed real alert is the failure mode this step must respect.
3. **Open-incident review (15 min)** — search open security-classified tickets/incidents: for each, current state, owner, next action, and age. Anything without movement in 72 hours gets a nudge or an escalation — security incidents must never go quiet (`skills/qa-and-closure/silent-ticket-detector` lens, security boards only).
4. **Posture-change check (15 min)** — run `skills/security/cyber-risk-posture-review` in delta mode (backed by `liongard_cyber_risk_dashboard` / `liongard_detection` where available): what changed across client environments this week — new admins, MFA coverage shifts, config drift. Verify inspector freshness before trusting deltas; state dataprint age.
5. Output the weekly security card: noise scorecard, the (max two) tuning changes proposed/made, open incidents with age and next action, and posture deltas worth a client conversation.

## Time-short version

Step 3 only — open incidents drifting is the unacceptable failure. Noise, tuning, and posture roll into next week with a note.

## Guardrails

- Tuning changes bias conservative: when the evidence is ambiguous, tune nothing — a missed real alert costs more than a noisy week.
- Tuning writes (rule/flow changes) require explicit approval; the ritual proposes with evidence, a human enacts.
- Incident details stay in incident channels; the shareable card references ticket numbers, not IOCs or client specifics.
- Liongard-sourced posture claims carry dataprint age; absent or stale inspectors degrade to "no data" — never to assumption.
- Skip-with-note beats fake completion; disclose result caps on alert-volume searches.
