---
name: Splashtop Access
description: A Splashtop remote-access problem landed — troubleshoot streamer/agent health and session connectivity from the symptom and docs, and hand the technician into the console. It is a remote-access tool, not a script-execution surface.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_itglue, add_ticket_note, update_ticket]
---

# Splashtop Access

Vendor specialization of remote-access troubleshooting for Splashtop. No native Super Magic integration today, so this is a diagnostic-and-handoff skill: the agent works the access symptom from the ticket, streamer behavior, and documentation, then directs the technician into the Splashtop console. Splashtop is a remote-*access* tool — the agent triages connectivity and streamer/agent health; it does not run commands or scripts on endpoints. Verify feature names (Streamer, SOS, unattended access, Splashtop Business/Enterprise) against Splashtop's current documentation.

## When to use

- An unattended computer won't show online in Splashtop, or the Streamer keeps disconnecting
- A tech can't start or hold a remote session (connection fails, times out, black screen, authentication rejected)
- A tech asks how to read Splashtop streamer status or why a machine is unreachable

## Steps

1. Classify the problem: streamer-offline (the unattended agent isn't connecting) vs session-connectivity (streamer online but the session fails/drops/black-screens) vs authentication (login, 2FA, or device-authorization rejection). Copy the exact symptom and any error wording.
2. For streamer-offline, run the reachability ladder per device-offline-runbook: is the endpoint itself online, is the Splashtop Streamer service running, is outbound to Splashtop's relay reachable (firewall/proxy/DNS), and is the Streamer version current. Distinguish whole-machine-down from streamer-only-down.
3. For session-connectivity, check relay/network path, streamer-vs-client version compatibility, display/black-screen causes (secure desktop, GPU/driver, monitor asleep), and session-permission scope. For authentication, check the user's team membership, 2FA state, and device-authorization/allow-list — a rejected login is often policy, not a fault.
4. Use available context: ticket history (search_tickets, same device/user, recent window) for recurrence, documentation (search_itglue) for the team/deployment and access policy, and a Liongard inspector if one covers the endpoint (verify last run, note age; degrade if absent).
5. Security check: unexpected sessions or unknown authorized devices are a potential access-compromise — treat per security-alert-response rather than as a connectivity ticket, and involve the security path.
6. Hand off for action: reinstalling/repairing the Streamer, adjusting team/permission/device-authorization, or any on-endpoint work is a technician action. Write the handoff with the classified cause and steps; the agent directs and records.
7. Document classification, ladder results, security check, and handoff. Client-facing wording per defensive-writing-standard.

## Guardrails

- Splashtop is an access tool — the agent never runs commands or scripts on endpoints; on-endpoint work is a technician step the agent directs and records.
- Separate "machine offline" from "streamer offline" before recommending a fix.
- Access scope, team membership, and device-authorization are security decisions — never widen them to "make it work"; narrowest scope, named approver, review date.
- Unexpected sessions or unknown authorized devices → security incident path, not a support ticket.
- No native Splashtop read integration: state is inferred from ticket/docs/Liongard — give the source and freshness.
- Degradation: without documentation, the client's Splashtop team layout and access policy may be unknown — say so.
