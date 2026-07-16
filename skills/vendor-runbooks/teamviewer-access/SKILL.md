---
name: TeamViewer Access
description: A TeamViewer remote-access problem landed — troubleshoot host/agent health, unattended access, and session connectivity from the symptom and docs, watch for the commercial-use-detected flag, and hand the technician into the console. Access tool, not a script surface.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_itglue, add_ticket_note, update_ticket]
---

# TeamViewer Access

Vendor specialization of remote-access troubleshooting for TeamViewer. No native Super Magic integration today, so this is a diagnostic-and-handoff skill: the agent works the access symptom from the ticket, host behavior, and documentation, then directs the technician into the TeamViewer console. TeamViewer is a remote-*access* tool — the agent triages connectivity and host/agent health; it does not run commands or scripts on endpoints. A TeamViewer-specific gotcha: the "commercial use detected/suspected" flag throttles or blocks sessions on free-tier usage and masquerades as a connectivity fault — check for it early. Verify feature names (Host, unattended access, Assignment, allowlist, Conditional Access) against TeamViewer's current documentation.

## When to use

- An unattended device won't connect in TeamViewer, or sessions drop/time out
- Sessions are being blocked, time-limited, or nagged — possibly the commercial-use flag
- A tech asks how to read TeamViewer host/assignment status or why a machine is unreachable

## Steps

1. Classify the problem: host-offline (unattended agent not connecting), session-connectivity (host online but session fails/drops), authentication/assignment (device not assigned to the company account, allowlist/Conditional-Access block), or commercial-use restriction. Copy the exact symptom and any on-screen message.
2. Commercial-use check first when sessions are throttled, time-limited, or show a commercial-use notice: this is a licensing/account state, not a network fault. Confirm the account tier and the device's assignment; the fix is licensing/assignment in the TeamViewer management console (a technician/account action), not endpoint troubleshooting. Flagging this early saves a wasted connectivity hunt.
3. For host-offline, run the reachability ladder per device-offline-runbook: endpoint online, TeamViewer service running, outbound to TeamViewer's network reachable (firewall/proxy/DNS), host version current, and device correctly assigned to the company account.
4. For session/auth issues, check version compatibility, allowlist/blocklist and Conditional Access policy, and account/group permission scope. Use ticket history (search_tickets, same device/account, recent window) for recurrence, documentation (search_itglue) for the account/assignment and policy, and a Liongard inspector if one covers the endpoint (verify last run, note age; degrade if absent).
5. Security check: unexpected sessions, unknown assigned devices, or logins from unfamiliar locations are a potential access-compromise — treat per security-alert-response and involve the security path; TeamViewer credentials are a known attacker target.
6. Hand off for action: reinstalling/repairing the Host, fixing assignment/licensing, adjusting allowlist/Conditional-Access, or any on-endpoint work is a technician/account action. Write the handoff with the classified cause; the agent directs and records.
7. Document classification (including commercial-use outcome), ladder results, security check, and handoff. Client-facing wording per defensive-writing-standard.

## Guardrails

- TeamViewer is an access tool — the agent never runs commands or scripts on endpoints; on-endpoint work is a technician step the agent directs and records.
- Check the commercial-use flag before chasing a network cause when sessions are throttled or blocked — it's a licensing state, not connectivity.
- Separate "machine offline" from "host offline" and from "device unassigned" — three different fixes.
- Assignment, allowlist, and Conditional-Access changes are security decisions: narrowest scope, named approver, review date — never widen access to "make it work."
- Unexpected sessions or unknown assigned devices → security incident path, not a support ticket.
- No native TeamViewer read integration: state is inferred from ticket/docs/Liongard — give the source and freshness.
- Degradation: without documentation, the client's TeamViewer account, assignments, and policy may be unknown — say so.
