---
name: Liongard Email Security Config Read
description: Read a client's email-security platform configuration (Mimecast/Proofpoint-class) through its Liongard inspector — policy posture, connector state, and config drift — to answer "how is their mail filtering actually set up" without an admin console.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_metric, liongard_detection, liongard_timeline, liongard_query, search_tickets, add_ticket_note]
---

# Liongard Email Security Config Read

The alert-response side of email security lives in the vendor runbooks (vendor-runbooks/mimecast-email-gateway, vendor-runbooks/proofpoint-email-security, vendor-runbooks/avanan-checkpoint-email); this skill is the configuration side: what the gateway's policies actually say, whether the connectors that carry the mail are healthy, and what changed. When a "why did this get through / why was this blocked" ticket lands, the policy read is the first fact to establish.

## When to use

- "How is <client>'s Mimecast/Proofpoint configured?" — policy posture before tuning or an incident review
- "Did an email-security policy change?" — drift context on a sudden spam wave or false-positive spike
- "Are their connectors healthy / is mail actually flowing through the gateway?"
- Policy evidence for a QBR, onboarding review, or security assessment

## Steps

1. Base pattern (skills/liongard-inspectors/liongard-access-pattern/SKILL.md): liongard_launchpoint filtered by the email-security platform's systemType (Mimecast, Proofpoint, or the client's equivalent — check which of the ~300 inspectors the partner actually runs rather than assuming). Verify last run succeeded; date the dataprint.
2. Route the question:
   - Policy posture → liongard_metric over the policy sections the inspector captures: anti-spam/anti-malware policy settings, impersonation/anti-spoofing protection, URL and attachment handling (rewrite/sandbox on or off, and for whom), and allow/block list sizes. Field layouts differ sharply per platform — verify field paths against the live dataprint; there is no shared schema across email-security inspectors.
   - The allow-list is the classic finding: broad domain-level allows (a whole domain allowlisted to "fix" one false positive years ago) bypass filtering silently. Report allow-list entries at domain scope as findings with a recommendation to re-justify each.
   - Coverage carve-outs → policies scoped to groups rather than the whole org: "URL rewriting on, except the executives group" is the finding, and executives are the target population. State enforcement scope with every policy read.
   - Connector / mail-flow state → gateway connector or route configuration as captured, cross-checked against the domain's MX (pair with the liongard-internet-domain read): MX pointing past the gateway straight at the tenant means the gateway is decorative for inbound — a critical finding. Route mail-auth specifics to security/dmarc-spf-failure-triage and connector rebuilds to m365-administration/email-connector-setup.
   - Drift → liongard_detection / liongard_timeline for policy changes, new allow-list entries, and connector modifications, dated. On a "suddenly getting spam" ticket, changes in the window come first.
   - Open-ended → liongard_query, verified before reporting.
3. Cross-check policy changes against search_tickets for the authorizing request; an undocumented policy loosening (new allow entry, disabled rewrite) with no ticket is flagged and, if recent and unclaimed, treated as possible tampering — attackers who gain gateway admin weaken policy before campaigns.
4. Output: posture summary (protection layers on/off and for whom), findings ranked (bypass routes and unexplained loosenings first), dataprint as-of date. Plain-text note; anything client-facing follows security/defensive-writing-standard.

## Guardrails

- Policy reads always state scope — "enabled" without "for whom" is not an answer.
- Config data is not delivery data: this skill says what SHOULD happen to mail; message traces (m365-administration/mail-trace-investigation, vendor consoles) say what DID. Never infer delivery outcomes from policy alone.
- Unexplained recent policy loosening escalates to security review rather than sitting in a drift report.
- Read-only: policy tuning and connector changes route to the vendor runbook and the client's change process; never present a recommended tightening as applied.
- Degradation: no email-security inspector → answer from docs, tickets, and the M365-side reads (transport rules, connectors via liongard-m365-tenant) only, labeled "gateway not instrumented."
