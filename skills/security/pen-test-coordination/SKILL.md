---
name: Pen Test Coordination
description: A client is running a penetration test — coordinate the desk's side: scope and window on record, expected-activity handling that never blanket-disables alerting, and findings intake turned into owned remediation tickets.
category: Security
tools: [search_tickets, search_clients, add_ticket_note, update_ticket, create_ticket, schedule_ticket, search_itglue]
---

# Pen Test Coordination

The desk's playbook for someone else attacking the client on purpose: know the engagement's scope and window, mark tester activity as expected without turning detection off, keep out-of-scope alerts fully live, and convert the findings report into tracked remediation work.

## When to use

- A client announces a scheduled penetration test (often insurance- or compliance-driven) and the desk needs to prepare
- Alerts are firing mid-window and a tech asks "is this the pen test?"
- The findings report arrived and needs turning into remediation tickets

## Steps

1. Get the engagement facts on the record before the window opens: testing firm, engagement window (dates/times, timezone), scope (IP ranges, applications, whether social engineering or physical is included), tester source IPs where provided, the rules of engagement, and the client's emergency contact for the test. Store in the coordination ticket and the client's documentation (search_itglue to check what exists); schedule_ticket the window so the desk sees it coming.
2. Alert-suppression discipline — the core rule: expected tester activity gets MARKED, never silenced. Do not disable EDR, SIEM rules, or monitoring for the window, and do not blanket-allowlist the tester IPs in detection tools. Detection firing on the tester is a finding for the report (detection works); detection disabled is a real attacker's dream window. If the client insists on suppression, scope it to the documented tester source IPs only, time-box it to the window, record who authorized it, and revert on schedule — the security-noise-tuning skill's narrow-scope principle applies.
3. During the window, triage keeps running: alerts matching the documented tester IPs + scope + window get verified as expected, annotated "expected pen-test activity, engagement <ticket ref>" in a plain-text note, and left in the record — not deleted, not auto-closed unseen. Anything NOT matching all three (source, scope, window) is treated as a real incident with the full security-alert-response path. Attackers know when pen tests happen; "it's probably the testers" is a hypothesis, not a verdict.
4. Escalation path stays hot: destructive behavior, out-of-scope targets, or production impact from the testers goes to the client's engagement contact immediately — pausing the test is the client's call, protecting production is the desk's duty to raise.
5. Findings intake when the report lands: create one remediation ticket per finding (create_ticket) — title, severity as rated by the testers, affected asset, the tester's recommended fix, an owner, and a target date scaled to severity. Cross-check each finding against existing tickets (search_tickets) to link known issues rather than duplicating. Critical/high findings on internet-facing assets route through vulnerability-report-triage urgency logic.
6. Close the engagement: revert any window-scoped suppressions (verify, don't assume), confirm detections that fired are back to normal handling, and note the retest plan — most engagements include a retest of remediated criticals. Document the whole engagement trail: scope, suppressions applied and reverted, incidents raised, findings ticketed.

## Guardrails

- Never blanket-disable or broadly allowlist detection for a pen-test window; suppression, if any, is per-tester-IP, time-boxed, authorized, and reverted — and reversion is verified.
- "Probably the pen test" never closes an alert: expected-activity classification requires matching documented source, scope, AND window.
- The desk does not conduct or extend testing — no running tester tools, no validating findings by re-exploitation.
- Findings reports are confidential: store per the client's documentation standards, don't paste report contents into loosely-scoped tickets.
- Unauthorized-window activity from "the testers" (wrong dates, wrong IPs) is treated as real until the engagement contact confirms in writing.
- Degradation: without search_itglue, the engagement record lives on the coordination ticket — make it complete there.
