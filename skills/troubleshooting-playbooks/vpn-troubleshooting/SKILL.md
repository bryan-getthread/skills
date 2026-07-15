---
name: VPN Troubleshooting
description: Diagnose VPN problems — won't connect, authenticates then no traffic, drops while working from home, or can't reach internal resources by name — via a client/auth/transport/DNS matrix.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# VPN Troubleshooting

Separates VPN failures into the four places they actually live — client, authentication, transport, and name resolution — and gives branch-specific remediation guidance plus escalate-when lines. Remediation is guidance for the tech or user; nothing executes remotely.

## When to use

- "<user> can't connect to the VPN" / "VPN connects but nothing loads"
- "VPN keeps dropping when I work from home"
- "Connected to VPN but can't reach <server> by name"
- MFA or SSO prompt loops during VPN sign-in

## Steps

1. **History first.** search_tickets for this user and this client's VPN. Same user recurring → local environment (home router/ISP). Many users at once → head-end, certificate, or identity outage; treat as an incident.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the client's VPN standard: vendor, client software, gateway address, auth method (SAML/MFA, certificate, PSK), split vs full tunnel, DNS suffix.
3. **Identify versions — never assume.** VPN client name and version, OS version, and whether the client was recently updated. Version mismatch against the head-end is a top cause after firmware upgrades.
4. **Get the error before theorizing.** Exact client error text/code and the client's own log. "Doesn't work" has at least four different meanings — pin down which stage fails: connect, authenticate, tunnel up, or traffic/name resolution.
5. **Branch:**
   1. **Authentication (SAML/MFA/certificate)** — fails at sign-in or loops. Check whether the user can sign in to other SSO apps (isolates identity vs VPN). MFA loop → often stale cached token or an identity conditional-access change; pair with the M365 sign-in playbook if the IdP is Entra. Certificate auth → check cert expiry on the device. Escalate when: an identity policy change broke all VPN users — route to whoever owns the IdP; if the failure is inside the vendor's SAML implementation, only the vendor can fix it — say so.
   2. **Transport / NAT-T** — client times out or connects then dies at the same point every time. From home networks, IPsec needs NAT traversal (UDP 4500) and IKE (UDP 500); some routers or ISPs mangle these. Guide: try a phone hotspot — if that works, the home router/ISP path is the culprit; disable SIP ALG / enable IPsec passthrough on the router, or move the client to a TLS-based transport if the platform supports it. Escalate when: the head-end firewall drops NAT-T after a firmware change — network resource, and possibly a vendor case.
   3. **"Drops while working from home"** — connects fine, dies intermittently. Ladder: Wi-Fi signal → router (SIP ALG, session timeouts) → ISP (CGNAT). Also check the laptop's power settings suspending the NIC, and DTLS/keepalive intervals. Guide one change at a time so cause is identifiable. Escalate when: multiple remote users drop at the same clock interval → head-end session/idle timeout or license limit, not the homes.
   4. **Split tunnel routing** — VPN is up, some resources unreachable. Determine what's supposed to be in the tunnel (docs). A resource outside the split-tunnel routes will never work by design — say so rather than chasing it. Escalate when: the route/ACL push from the head-end is wrong for everyone — config change request, not a per-user fix.
   5. **DNS suffix / name resolution** — reachable by IP but not by name. Check the tunnel adapter got the internal DNS servers and search suffix. Guide: `nslookup <server> <internal-dns-ip>` to prove the resolver works over the tunnel; if it does, the client's adapter DNS config or suffix list is wrong. Escalate when: internal DNS itself is unhealthy — switch to the DNS playbook.
6. **Verify and note.** Have the user reach the actual resource that failed, not just get a green icon. Plain-text note: stage that failed, error code, branch, fix or handoff, verification.

## Guardrails

- No remote execution — all commands are relayed to the tech or user to run.
- Never guess gateway addresses, transport ports, or auth methods — pull from client docs or ask. Do not invent vendor KB articles; verify with web_search.
- If the defect is in the VPN vendor's client or firmware, state that only the vendor can fix it and offer the documented workaround.
- Firewall/head-end changes are never "just do it" guidance — flag them for the resource who owns the device, with the evidence attached.
- Docs tools vary per tenant (search_itglue / search_hudu may be absent) — say what you could not check.
