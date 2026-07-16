---
name: RD Gateway Issues
description: Diagnose Remote Desktop Gateway / RD Web Access problems — external RDP connections failing through the gateway, certificate errors, CAP/RAP connection- and resource-authorization-policy mismatches, and 2FA/MFA integration failures — from the RD Gateway operational logs and the exact client error, never by opening RDP to the internet.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# RD Gateway Issues

RD Gateway tunnels RDP over HTTPS so users reach internal desktops/servers without exposing raw RDP — and it fails at a few defined gates: the certificate, the Connection Authorization Policy (who may connect), the Resource Authorization Policy (what they may reach), and any MFA layer in front. This playbook reads the gateway's own logs and the client's exact error code, and never "fixes" access by exposing RDP directly.

## When to use

- External users can't connect through RD Gateway / RD Web, or connections drop at the gateway
- Certificate warnings/failures connecting, or "your computer can't connect to the Remote Desktop Gateway server"
- Some users/resources connect and others don't (authorization policy suspicion)
- RD Web sign-in or the MFA/2FA prompt in front of the gateway fails

## Steps

1. **Deployment and version first.** search_itglue / search_hudu / search_knowledge_base for the RDS deployment: Windows Server version, the roles in play (RD Gateway, RD Web Access, Connection Broker, Session Hosts), the external FQDN and the **certificate** (issuer, expiry, SAN matching the external name), the **CAP/RAP** design (which AD groups may connect, which resource groups/computers they may reach), and any MFA layer (NPS extension for Entra MFA, Duo, or another RADIUS/2FA in front). Note the cert expiry — it's the first thing to break. If a Liongard Windows inspector runs, corroborate role/cert state via liongard_launchpoint / liongard_metric and note dataprint age.
2. **History first.** search_tickets for this client + RDS/RD Gateway: a recent certificate renewal (gateway TLS dies on expiry/rebind — sudden onset on a date is a cert until proven otherwise), a CAP/RAP or group change, an MFA rollout, or a firewall/NAT change on 443. Cert and policy edits are the usual triggers.
3. **Get the client error and gateway log before theorizing.** The exact client-side message/code (a cert-trust error, an authorization "not authorized to connect", or a can't-reach-gateway are three different problems), and on the gateway the **Operational logs** under Applications and Services → Microsoft-Windows-TerminalServices-Gateway (they log per-connection which CAP/RAP was evaluated and why a connection was denied). Read the gateway's own verdict — it usually names the failing gate.
4. **Branch:**
   1. **Certificate** — trust/name errors or connections failing after a renewal: the gateway cert must be valid, trusted by clients (public CA or a trusted internal chain), match the external FQDN (SAN), and be **bound** in RD Gateway Manager and across the RDS roles after renewal — a renewed cert with a new thumbprint often isn't re-applied to all roles. Pair with ssl-certificate-renewal / adcs-pki-issues. This is the most common gateway break.
   2. **CAP (Connection Authorization Policy)** — "not authorized to connect to this gateway": the user isn't in a group the CAP allows, the CAP's device/auth requirements aren't met (e.g. it requires smart-card or a specific auth method), or CAP ordering denies them. The operational log names the CAP evaluated. Fix group membership or the CAP condition — don't create an allow-all CAP.
   3. **RAP (Resource Authorization Policy)** — connects to the gateway but can't reach the target host: the RAP doesn't grant that user group access to that resource group/computer, or the target isn't in the allowed resource list (a new session host not added to the RAP group is classic). Read the RAP the log evaluated and add the resource/group properly.
   4. **MFA / 2FA integration** — sign-in fails at the MFA step or the gateway rejects post-MFA: if MFA is via the NPS extension (Entra MFA) or a RADIUS 2FA (Duo etc.), the RD Gateway is configured to use a **central NPS/RADIUS** server — a failure there (NPS extension error, RADIUS shared-secret/timeout, the user not MFA-registered, or a Conditional Access interaction) blocks the connection. Read the NPS/RADIUS logs; pair with radius-nps-auth. Never disable MFA to "get them in".
5. **Verify and note.** Success is a real external user connecting through the gateway to their intended resource, past MFA, with a valid cert and no warnings. Plain-text note: the client error, the CAP/RAP the gateway log evaluated, cert state, MFA path, branch, action or handoff, verification.

## Guardrails

- **Never work around a gateway problem by exposing RDP (3389) directly to the internet** — that's a critical security exposure; the gateway exists precisely to avoid it. Fix the gateway.
- No remote execution — RD Gateway Manager, cert, and NPS steps are guidance for a tech with the right access; use get_ninjaone_device_link to reach the gateway/host when the RMM integration is enabled.
- Never create allow-all CAP/RAP policies or drop MFA to unblock a user — grant the specific group/resource, or fix the MFA path; keep least privilege.
- Certificate work touches live external access — treat as a change, rebind across all RDS roles, and pair with the cert playbooks.
- Shared secrets and certs are credentials — keep them out of PSA notes; refer to RADIUS clients and certs by name.
- Do not invent event log paths, policy behaviours, or MFA-extension specifics — web_search Microsoft's current RDS docs and cite.
- Docs/Liongard coverage varies per tenant — note what you couldn't check and dataprint age. Notes destined for a PSA sync: plain text, no markdown or emojis.
