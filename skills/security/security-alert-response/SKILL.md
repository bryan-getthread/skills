---
name: Security Alert Response
description: A security alert ticket arrived (SOC detection, sign-in anomaly, breached credential, dark web, user-created alert) — extract the facts, route it to the right client, tier the severity, and contain or close with documented reasoning.
category: Security
tools: [search_tickets, search_clients, search_contacts, add_ticket_note, update_ticket, search_itglue]
---

# Security Alert Response

The front door for security alerts: parse the alert body, attach it to the correct client, check prior context, assign a severity tier with a response clock, and drive either containment or an evidenced closure.

## When to use

- An alert from a SOC, SIEM, identity-protection, or dark-web monitoring tool lands as a ticket
- An alert arrived on a shared alert-intake company and needs routing to the real client
- A tech asks "what do I do with this security alert?"

## Steps

1. Extract fields from the alert body: affected user/UPN, tenant or client identifiers, alert type, indicator detail, and timestamps. Alerts often arrive addressed to a shared alert-intake company — use the extracted tenant/domain/user fields with search_clients and search_contacts to route the ticket to the correct client. Never route on name similarity alone; if confidence is low, leave the routing unchanged and flag it for a human.
2. Prior-context check: search_tickets for the same client + same alert type + same account within the last 90 days. A documented benign explanation there (confirmed VPN egress, known travel, accepted risk) informs the verdict — but confirm it still applies today. Never close purely on the strength of an old ticket.
3. Assign a severity tier and its response clock (adapt labels to the desk's documented tiers):
   - Critical — confirmed active compromise: containment starts now, before further investigation.
   - High — credible sign of compromise: respond within the hour.
   - Medium — suspicious but plausible-benign: same business day.
   - Low / informational — within normal queue cadence.
4. Identity first: most alert volume is login events, not malware. Evaluate the sign-in evidence — location, device, MFA outcome, session context — before assuming an endpoint or malware problem. Route to the matching specialist runbook where one fits: impossible-travel-runbook, inbox-rule-alert-runbook, new-user-created-alert, edr-detection-runbook, dark-web-alert-lifecycle.
5. Live-threat path — contain fast, investigate second: have sign-in blocked and active sessions revoked for the affected account (the compromised-account-containment checklist; the technician executes in the identity console, the agent records each action with a timestamp), then investigate scope.
6. Benign or stale path: close only with evidence. Never close on assumption — expected travel or VPN use must be confirmed with the client or their documentation, not presumed. The closing note records what was checked and why it is benign.
7. Write the internal note documenting the decision, not just the action — verdict, evidence, tier, and response taken — then set classification and status per the soc-classification-tree skill. Closure statuses stay with management.

## Guardrails

- Do not auto-close a live or recent exposure; age and confirmed validity decide the path.
- Never close on assumption — "probably the VPN" is a hypothesis, not a closure reason.
- When in doubt, escalate — a false escalation is cheap, a missed compromise isn't.
- Low routing confidence → no reassignment; flag for a human instead.
- Follow the client's documented incident policy for anything resembling active compromise.
- Defensive writing in anything client-facing: "alert," "detection," "suspicious sign-in" — reserve "breach" for confirmed system-level events; never "hacked."
- Degradation: without documentation tools (search_itglue), VPN ranges and travel records may be unavailable — say so in the note rather than guessing.

## Consolidates

Breached-credential response, dark-web-alert auto-close, security incident investigation, SOC user-created-alert, alert-source routing, and SecOps triage/classification skills.
