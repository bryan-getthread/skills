---
name: SIEM Alert Generic
description: A SIEM or log-platform alert landed that no vendor-specific runbook covers — extract the alert's anatomy, ask the correlation questions, reach an evidenced verdict, and feed tuning back to the platform owner.
category: Security
tools: [search_tickets, search_clients, search_contacts, add_ticket_note, update_ticket]
---

# SIEM Alert Generic

The vendor-neutral workup for correlation-platform alerts: decompose what the rule actually matched, ask what the alert alone cannot answer, and close the loop — either into an incident path or into tuning feedback — without ever letting "the SIEM is noisy" become the verdict.

## When to use

- A SIEM/log-analytics alert arrives from a platform with no dedicated vendor runbook on this desk
- A tech asks "what is this correlation alert even telling me?"
- A recurring SIEM alert needs a structured verdict before someone tunes it away

## Steps

1. Extract the alert anatomy from the body — every SIEM alert reduces to these fields; pull each one or mark it missing:
   - Rule: name, ID, and (if included) the logic/threshold that fired.
   - Entities: user(s), host(s), IP(s), process or application involved.
   - Trigger window: when, over what period, how many matching events.
   - Source telemetry: which log source(s) fed the rule (identity, firewall, endpoint, mail).
   - Severity as assigned by the platform — treat as input, not verdict.
2. Route the ticket to the right client if it arrived on a shared intake (per the security-alert-response routing rules — extracted tenant/user/domain via search_clients / search_contacts, never name-similarity alone).
3. Ask the correlation questions the alert can't answer itself:
   - Is the entity privileged, VIP, or a service account? (search_contacts, prior tickets)
   - Has this rule fired for this client/entity before, and what was the verdict? (search_tickets, last 90 days — a documented benign pattern informs but never auto-closes)
   - Does the trigger window line up with anything legitimate: change windows, patch runs, backup jobs, a documented pen-test engagement, or a migration?
   - Is this alert alone, or part of a cluster? Search adjacent alerts for the same entity in the same window — three medium alerts on one account in an hour outrank their individual severities.
4. Verdict with evidence, on the security-alert-response tiering: credible compromise signals → tier it, and branch to the matching specialist runbook (impossible-travel-runbook, inbox-rule-alert-runbook, edr-detection-runbook, compromised-account-containment for anything active). Benign with a confirmed explanation → close with the evidence named. Ambiguous → it stays open and gets escalated; "the SIEM cries wolf" is not a closure reason.
5. Tuning feedback loop: if the verdict is benign AND the same documented explanation now recurs across occurrences, hand the pattern to the security-noise-tuning skill to build the evidence pack for the platform owner. The fix happens in the SIEM rule, scoped narrowly — never as auto-closure in the PSA. Until tuning lands, every occurrence still gets verified.
6. Document the decision, not just the action: rule, entities, correlation answers, verdict, and tier in a plain-text note; classify per soc-classification-tree.

## Guardrails

- Platform severity is an input; the desk's tier comes from the correlation questions and evidence.
- Never close on rule reputation ("that rule is always noise") — every close names its evidence, and repeat-noise routes to tuning, not to reflexive closure.
- Missing anatomy fields are stated as missing; do not reconstruct or guess what the rule matched.
- Cluster check is mandatory before closing any single alert benign — the correlation platform's whole value is context, so look for it.
- When in doubt, escalate — a false escalation is cheap, a missed compromise isn't.
- Degradation: this skill works from the alert body and PSA history; without access to the SIEM console itself, deeper log pivots are directed to the technician with specific queries to run, and their results are recorded.
