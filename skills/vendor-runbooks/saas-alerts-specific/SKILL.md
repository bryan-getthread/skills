---
name: SaaS Alerts Rule Triage
description: A specific SaaS Alerts rule fired — triage per-rule (the exact rule name and its logic), read what the rule's threshold actually measured, and decide whether it's a true signal or benign-by-context noise for tuning.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# SaaS Alerts Rule Triage

A finer-grained companion to saas-alerts-mdr. Where saas-alerts-mdr maps SaaS Alerts events onto the identity-family investigation runbooks by *event class*, this skill works one level down: at the level of the *specific named rule* that fired. SaaS Alerts is rule-driven — each notification carries the exact rule (e.g. a foreign-country sign-in rule, a mailbox-rule-creation rule, a mass-download threshold rule, a global-admin-change rule), and reading the rule's own logic and threshold is what tells you whether the alert is meaningful. Use this when the triage question is "what does *this rule* mean and did it fire for a real reason?" Route the actual investigation through saas-alerts-mdr; do not duplicate its containment/sweep steps here. Verify rule names and defaults against the vendor's current documentation.

## When to use

- A named SaaS Alerts rule fired and you need to read that rule's specific logic before deciding
- Recurring firings of one rule need a tuning decision (real signal vs benign-by-context)
- A tech asks "what triggers this rule?" or "why did this one fire?"

## Steps

1. Identify the exact rule by name and read its logic: what condition and threshold it evaluates, what data it inspects, whether it's a built-in template rule or a custom/partner rule, and whether it has a Respond automation attached. Copy the rule name and its stated logic verbatim; if the rule's meaning is custom or unclear, mark it unconfirmed rather than guessing.
2. Read what actually tripped it: the specific values against the threshold (which country/IP, how many files, which admin role) — the raw measurement, not just "the rule fired." A threshold rule that barely crossed differs from one that blew past it.
3. Judge signal-vs-context: does the measured value have a benign explanation for *this* client (documented VPN egress, a known travel pattern, a scheduled bulk operation, a planned admin change)? Check documentation (search_itglue) and prior context (search_tickets, same user/rule, 90 days). Context can downgrade; it cannot dismiss without evidence.
4. If it's a real signal, hand to saas-alerts-mdr for the investigation and containment/sweep by event class — do not re-implement those steps here; this skill's job ends at "this rule fired for a real reason, here's the measured evidence."
5. If it's benign-by-context and recurring, produce a scoped tuning proposal per security-noise-tuning: adjust *this rule* for the specific benign pattern (a per-user travel window, a known egress range, a scheduled-operation exception) — never disable the rule class wholesale. Name the owner and a review date.
6. Document the rule, its logic, the measured trip values, the signal-vs-context judgment, and either the handoff to saas-alerts-mdr or the tuning proposal. Client-facing wording per defensive-writing-standard.

## Guardrails

- Read the rule's actual logic and the values that tripped it — never triage on the rule name alone; two firings of the same rule can mean very different things.
- Context downgrades, it never dismisses without evidence — a documented VPN explains a foreign login; a guess does not.
- Do not duplicate saas-alerts-mdr's investigation/containment — hand real signals to it; this skill is the per-rule reading layer.
- Tuning is scoped to the specific rule and pattern, with a named approver and review date — never blanket-disable a rule class to quiet noise.
- Custom/partner rules whose logic isn't documented are marked unconfirmed, not interpreted from the name.
- The agent has no SaaS Alerts console access — rule edits, Respond changes, and unlocks are technician steps the agent directs and records.
- Degradation: without documentation, benign-context signals (VPN, travel, scheduled jobs) are unknown — lean on the mdr verification ladder and say so.
