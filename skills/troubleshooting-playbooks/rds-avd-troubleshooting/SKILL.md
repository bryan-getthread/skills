---
name: RDS / AVD Troubleshooting
description: Diagnose Remote Desktop Services and Azure Virtual Desktop session problems — can't connect, stuck at "loading profile", licensing errors, black screens, printers missing in session — by identifying the layer (broker, host, licensing, profile, redirection) before touching anything.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, liongard_launchpoint, liongard_metric, web_search]
---

# RDS / AVD Troubleshooting

A session-host stack fails in layers: reach the broker/gateway → get assigned a host → license check → session created → profile loads → redirections attach. The rule: place the failure on that ladder first — the user's screenshot ("can't connect") is compatible with all six layers.

## When to use

- "<user> can't connect to the remote desktop / AVD" — error, hang, or immediate disconnect
- Everyone disconnected from or unable to reach one session host, or the whole farm
- "No Remote Desktop license servers available" / licensing-grace errors appearing
- Stuck at "Please wait for the user profile service", black screen after logon, or printers/drives missing inside the session

## Steps

1. **Version identification first.** Establish which stack this client runs before any advice: classic on-prem RDS (broker/gateway/session hosts), AVD (host pools, Azure-side broker), or Windows 365 / a third-party layer (Citrix → the citrix-basics playbook). search_itglue / search_hudu / search_knowledge_base for the deployment doc: host names, broker/gateway, host pool names, FSLogix or roaming profiles, license server and CAL type (per-user vs per-device). The fix paths differ completely.
2. **History first (scope the blast radius).** search_tickets: one user → profile/assignment/account; all users on one host → that host; all users everywhere → broker, gateway, licensing, or (AVD) an Azure-side or agent issue. Check the vendor-outage-checker playbook logic for AVD before deep-diving — if Azure's service is degraded, only Microsoft can act; say so honestly.
3. **Get the error/event before theorizing.** Exact client-side error text plus, on the suspect host: Event Viewer → TerminalServices-* operational logs (connection, session broker client), and for AVD the agent/stack event logs and host status in the host pool. For profile hangs, the FSLogix logs if FSLogix is in play. Capture event IDs verbatim.
4. **Branch by ladder rung:**
   1. **Can't reach broker/gateway (nobody connects, connection times out before any credential prompt)** — on-prem: broker/gateway service, certificate expiry on the gateway (sudden farm-wide failure on a date = check cert dates first), DNS for the farm name. AVD: host pool's hosts show unavailable/agent unhealthy. Escalate when: certificates or Azure subscription issues are owned elsewhere.
   2. **Licensing (error names licensing, or exactly ~120 days after a new deployment)** — the RDS licensing **grace period is 120 days**; deployments "work fine" unlicensed then fail all at once when it expires. Check the licensing diagnoser: license server reachable? CALs installed and the right mode (per-user vs per-device — mismatch is common)? Do NOT clear the client-side GracePeriod registry key as a fix — it's a diagnostic dodge that returns in 120 days; the fix is real CALs and correct mode. Escalate when: CAL purchase/agreements are an account-management conversation.
   3. **One host sick (only its users affected)** — check host resource state (memory/disk full, hung Windows Update reboot pending), and in AVD/farm setups drain it: **do not sign users out to test** — set drain mode / disallow new sessions, let it empty, then investigate. Rejoining or rebuilding a host is preferable to long forensic surgery when the farm is otherwise healthy. Escalate when: the same failure follows across hosts (that's an image/GPO issue, not a host).
   4. **Profile load failures / black screens / "temporary profile"** — if FSLogix or roaming profiles are in play, hand off to the **roaming-profiles-fslogix** playbook (VHD locks and orphaned sessions live there). Black screen after logon with profile fine is commonly shell/GPO-loopback processing time or a stuck appx registration — check how long, not just whether, logon completes.
   5. **Redirection failures (printers, drives, clipboard missing in-session)** — determine policy vs plumbing: is the redirection allowed by GPO/host pool RDP properties (deliberately disabled redirections are common security posture — check docs before "fixing"), and for printers whether the path is Easy Print or a print-server driver. One user → their client device/driver; everyone → policy or the Easy Print/driver layer on hosts. Pair with printer-troubleshooting for the driver side.
5. **Verify and note.** Success = a fresh session for the affected user(s) with profile loaded and expected redirections present — confirmed by the user in-session, not by a successful ping. Plain-text note: stack (RDS/AVD), ladder rung, event IDs verbatim, action or handoff, verification and time.

## Guardrails

- Never reboot a session host or sign out sessions with users connected as a diagnostic step — drain first; every session is someone's workday.
- Never clear licensing grace registry keys or reinstall the licensing role to silence CAL errors — surface the real licensing state to the account owner.
- Do not change host pool / farm-wide RDP or GPO settings to fix one user; one-user problems have one-user causes.
- All console/event work is guidance for the tech — no script execution or remote commands from here.
- If Citrix (or another broker layer) fronts these hosts, use the citrix-basics playbook — fixing the Microsoft layer under a Citrix problem wastes the outage.
- For AVD platform-side failures, be honest that only Microsoft can act; reference the incident and track it.
- Docs coverage varies per tenant — note anything you could not check.
