---
name: WatchGuard Firewall Alerts
description: A WatchGuard event needs triage — a Firebox offline in WatchGuard Cloud, AuthPoint MFA push/token trouble, or mobile-VPN authentication failures. Separate connectivity from compromise and keep the AuthPoint identity discipline.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, get_ninjaone_device_link, add_ticket_note, update_ticket]
---

# WatchGuard Firewall Alerts

Vendor runbook for the WatchGuard stack: Firebox appliances (offline/health alerts via WatchGuard Cloud or Dimension), AuthPoint MFA, and the mobile VPN flavors that authenticate through them. Most tickets are connectivity plumbing — but the AuthPoint and VPN-auth paths are identity events and inherit the full identity canon. Verify feature and console specifics against WatchGuard's current documentation.

## When to use

- A "Firebox offline / not connected to WatchGuard Cloud" alert lands
- Users can't complete AuthPoint MFA (push not arriving, token codes rejected)
- Mobile VPN (SSLVPN/IKEv2) authentication failures — one user or many

## Steps

1. Firebox offline → run device-offline-runbook logic with the firewall twist: "offline" in WatchGuard Cloud means the management tunnel is down, not necessarily the site.
   - Is the site actually down? Check for parallel signals: other device alerts from the same site, user tickets, a quick reachability check the technician can run. Site up + Firebox passing traffic + only cloud-management down → ISP blip or the appliance's outbound management connectivity; low urgency, still investigate.
   - Site down → network-outage-triage takes over: power, ISP, then the appliance itself. An unreachable firewall is a full-site event — set priority accordingly.
   - Check search_tickets for flapping history: a Firebox that "goes offline" nightly has an ISP/DHCP/power pattern worth a problem ticket, not serial alert closes.
2. AuthPoint MFA issues → identity discipline first, troubleshooting second:
   - Verify the requester via a number on file before touching MFA state — MFA "help" requests are a takeover vector (same canon as duo-mfa-anomalies).
   - Push not arriving → mobile-side checks (notifications, connectivity, app signed in) before server-side; token codes rejected → time drift is the classic cause (device clock sync).
   - Unexpected pushes the user didn't initiate → the password is burned: rotate now, review AuthPoint/Firebox auth logs (technician), and on any approved-by-mistake push go to compromised-account-containment.
   - Re-activation/re-enrollment of AuthPoint on a new phone follows the verified-identity ladder; remove the old token first, and never issue bypass mechanisms without time-box + logging.
3. VPN auth failures → scope decides the branch:
   - One user → credential state (expired/locked account upstream — AuthPoint, RADIUS, or the directory behind it), client version, and their network; walk vpn-troubleshooting for the client side.
   - Many users at once → server-side chain: Firebox ↔ authentication server ↔ directory. Check certificate expiry on the VPN portal, the auth server's health, and recent config changes (search_tickets for changes; search_itglue for the documented auth chain). Mass failure right after a config or certificate change is the change until proven otherwise.
   - Repeated failures for one account from unfamiliar sources with no user at the keyboard → treat as credential-stuffing against the VPN: this is a security event (security-alert-response), not a support ticket.
4. Document: what was checked in which console (WatchGuard Cloud status, Traffic Monitor/logs, AuthPoint activity — technician actions the agent directs), the verdict, and the fix or escalation. Escalate hardware/firmware suspicion to WatchGuard support with the escalation package: serial, firmware version, cloud status history, log excerpts, and what was already ruled out.

## Guardrails

- Never close a Firebox-offline alert on "it came back" without noting the gap window and checking for a pattern — flapping is a finding.
- AuthPoint state changes (token resets, re-enrollment, bypass) require identity verification via a number on file — the request channel is not proof. No standing bypasses; anything issued is time-boxed and logged.
- Failed-VPN-auth floods for a single account are treated as an attack signal, not noise — never resolve by unlocking the account repeatedly.
- Firewall config changes are technician actions under the client's change process — the agent directs, records, and never treats a config edit as routine ticket work.
- Certificate and firmware facts get verified in the console, not assumed from the alert age.
- Degradation: without WatchGuard Cloud/Dimension visibility, say the view is partial and name exactly what the tech should pull.
