---
name: Liongard Change Review
description: Answer "what changed in this client's environment recently" from Liongard detections and timelines, and correlate those changes with new tickets. Use when something broke after a change, before/after an incident window, or for a periodic change-awareness pass.
category: Devices & Infrastructure
tools: [liongard_environment, liongard_detection, liongard_timeline, liongard_events, liongard_alert, search_tickets, add_ticket_note]
---

# Liongard Change Review

Pulls the change history Liongard's inspectors detected across a client's environment — configs, accounts, licenses, DNS, policies — and lines it up against the tickets that appeared around the same time.

## When to use

- "Something broke at <client> this week — what changed?"
- Incident investigation needs the change picture around a time window.
- Periodic per-client change review ("anything notable change in the last 30 days?").
- A server diagnostic or outage triage wants change correlation.

## Steps

1. Resolve the client to a Liongard environment via `liongard_environment`; rank name matches and state your pick.
2. Fix the window: the requester's incident window plus padding (start 48–72h before the first symptom — changes precede symptoms), or default to the last 14 days for a periodic review.
3. Pull the change record: `liongard_detection` for detected changes, `liongard_timeline` for system-by-system history, `liongard_events` and `liongard_alert` for anything the inspectors escalated. Flag result caps and narrow by system or sub-window rather than trusting a truncated list.
4. Sort the changes by risk relevance, highest first: identity/admin changes (new admin accounts, permission grants, MFA changes), security config (firewall rules, conditional access, DNS records), infrastructure (server roles, services, storage), licensing/subscription shifts, then routine churn (expected user adds/removes). Note which system each came from and when.
5. Correlate with tickets: `search_tickets` for the client over the same window. Line up ticket open times against change times — a cluster of "can't log in" tickets hours after a conditional-access change is a lead. Present correlations as time-adjacency, not proven causation, and say which direction the evidence points.
6. Check the inverse too: high-risk changes with NO corresponding ticket or documented work order deserve a flag of their own — an undocumented admin-account creation is a finding even when nothing is broken.
7. Output: the change list ranked by relevance (system, change, when, detected by), ticket correlations with their strength, unexplained high-risk changes called out, and recommended follow-ups. Offer a plain-text note via `add_ticket_note`.

## Guardrails

- Time-adjacency is not causation — every correlation is labeled with its strength, and the output never asserts "this change caused the outage" without corroborating evidence.
- Defensive writing on identity/security findings: an undocumented admin account is "unexplained — verify", never "breach" or "compromise" unconfirmed.
- Liongard sees what its inspectors inspect — name the systems covered in this environment so absence of a detected change is not read as absence of change.
- Read-only skill: it reports and recommends; reverting changes or opening investigations is human-owned.
- If Liongard is not enabled for the tenant, say change detection is unavailable and degrade to ticket-history inference, clearly labeled as such.
- Plain-text notes only.
