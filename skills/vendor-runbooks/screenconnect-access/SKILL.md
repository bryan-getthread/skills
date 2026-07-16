---
name: ScreenConnect Access
description: A ScreenConnect / ConnectWise Control access problem landed — troubleshoot unattended-agent health and session connectivity from the symptom and docs, and hand the technician into the console. It is a remote-access tool, not a script-execution surface.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_itglue, add_ticket_note, update_ticket]
---

# ScreenConnect Access

Vendor specialization of remote-access troubleshooting for ScreenConnect (ConnectWise Control). No native Super Magic integration today, so this is a diagnostic-and-handoff skill: the agent works the access symptom from the ticket, agent behavior, and documentation, then directs the technician into the ScreenConnect console. ScreenConnect is a remote-*access* tool — the agent triages connectivity and unattended-agent health; it does not run commands, scripts, or backstage tools on endpoints. Verify feature names (Access agent, Backstage, guest/host client) against ConnectWise's current documentation, and treat security advisories seriously (ScreenConnect has been a targeted product — a compromised instance is an incident, not a support ticket).

## When to use

- An unattended device won't appear online in ScreenConnect, or drops session repeatedly
- A tech can't launch or complete a remote session (client won't install, session times out, host/guest mismatch)
- A tech asks how to read ScreenConnect agent health or why a machine is unreachable

## Steps

1. Classify the problem: unattended-agent-offline (the Access agent isn't checking in) vs session-connectivity (agent is online but the session fails) vs client-install (the guest/host client won't deploy). Copy the exact symptom and any error wording.
2. For agent-offline, run the standard reachability ladder per device-offline-runbook: is the endpoint itself online (other monitoring), is the ScreenConnect service running, is outbound to the relay/server reachable (firewall/proxy/DNS), and is the agent version current. Distinguish "the whole machine is down" from "only the ScreenConnect agent is down" — different tickets.
3. For session-connectivity, check relay/port reachability, host-vs-guest client version compatibility, session-group/permission scope, and whether the server itself (self-hosted instance) is healthy. For client-install, check OS/permission blockers and endpoint-protection interference.
4. Use context sources the agent does have: ticket history (search_tickets, same device/instance, recent window) for recurrence, documentation (search_itglue) for the instance URL, version, and access-policy, and — if a Liongard inspector covers the endpoint — corroborating state (verify last run, note age; degrade if absent).
5. Security check — do not skip: unexpected sessions, unknown access agents, or an alert about the ScreenConnect instance itself are potential compromise (this product has been actively exploited). Treat as a security incident per security-alert-response, not a connectivity ticket, and involve the security path.
6. Hand off for action: reinstalling/repairing the Access agent, changing session/permission config, patching the instance, or any on-endpoint work is a technician action in ScreenConnect (or on the device). Write the handoff with the classified cause and steps; the agent directs and records.
7. Document the classification, ladder results, security check outcome, and handoff. Client-facing wording per defensive-writing-standard.

## Guardrails

- ScreenConnect is an access tool — the agent never runs commands, scripts, or Backstage sessions on endpoints; all on-endpoint work is a technician step the agent directs and records.
- Separate "machine offline" from "access agent offline" before recommending a fix — reinstalling the agent on a powered-off machine helps no one.
- Treat instance-level anomalies (unknown sessions/agents, advisories about the ScreenConnect server) as a security incident, not a support ticket — this product is a known target; when in doubt, escalate to the security path.
- Never widen session permissions or access scope to "make it work" — access scope is a security decision: narrowest scope, named approver, review date.
- No native ScreenConnect read integration: state is inferred from ticket/docs/Liongard — give the source and freshness, don't present it as a live console read.
- Degradation: without documentation, the instance URL, version, and access policy may be unknown — say so.
