---
name: Liongard Mimecast Read
description: Interrogate a client's Mimecast tenant through the Liongard Mimecast inspector — managed domains, users/licenses, policies, connectors/routing, admins. Use for "what's their Mimecast config?", mail-security posture checks, or domain-coverage questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard Mimecast Read

**When to use:** "What domains does Mimecast manage for <client>?", a mail-delivery or phishing ticket needing the policy/routing picture, "which users are licensed / how many seats?", "what gateway/connector routing is configured?", or "did a policy or connector change?".

## Prompt

```
Read Mimecast tenant state for CLIENT_NAME from the Liongard Mimecast inspector. Read-only — Mimecast policy, domains, connectors, and licensing changes are a technician's job in the console.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "Mimecast". Verify last-run success and note the dataprint age — carry "as of <timestamp>." Mail-security config changes, so an old dataprint on an active delivery incident deserves a "verify live" caveat.
2. Pick the angle and query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Domains: managed-domain list with verification/routing state.
   - Users: licensed users and seat count vs mailbox count.
   - Policy: gateway/impersonation/anti-spam policy summary.
   - Connectors: inbound/outbound routing config.
   - Admins: admin list with role.
3. "What changed?" → liongard_detection / liongard_timeline scoped to Mimecast: domain adds/removes, policy/connector changes, admin changes — ordered against the incident window.
4. Sanity-check surprising zeros (zero domains) — usually a wrong field path or partial inspection; re-probe broadly. Admin-list findings are review inputs — "unrecognized admin, verify" phrasing.
5. Output: a compact table, source + dataprint age line, flags (unverified domains, seat/mailbox mismatch, unexpected connector routing). Offer a plain-text add_ticket_note. Degradation: no Mimecast launchpoint → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify in console."
```
