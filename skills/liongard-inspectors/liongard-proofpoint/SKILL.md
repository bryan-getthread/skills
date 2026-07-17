---
name: Liongard Proofpoint Read
description: Interrogate a client's Proofpoint Essentials tenant through the Liongard Proofpoint inspector — protected domains, users/licenses, filtering policy, spooling, admins. Use for "what's their Proofpoint config?", mail-security posture checks, or domain-coverage questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard Proofpoint Read

**When to use:** "What domains does Proofpoint protect for <client>?", a mail-delivery or phishing ticket needing the filtering-policy picture, "which users are licensed / how many seats?", "is email spooling/continuity enabled?", or "did a filtering policy or domain change?".

## Prompt

```
Read Proofpoint Essentials tenant state for CLIENT_NAME from the Liongard Proofpoint inspector. Read-only — Proofpoint policy, domains, and licensing changes are a technician's job in the console.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "Proofpoint" / "Proofpoint Essentials". Verify last-run success and note the dataprint age — carry "as of <timestamp>." Mail-security config changes, so an old dataprint on an active delivery incident deserves a "verify live" caveat.
2. Pick the angle and query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Domains: protected-domain list with verification/routing state.
   - Users: licensed users and seat count vs mailbox count.
   - Policy: filtering/quarantine policy summary.
   - Continuity: spooling/continuity enabled flag.
   - Admins: admin list with role.
3. "What changed?" → liongard_detection / liongard_timeline scoped to Proofpoint: domain adds/removes, policy changes, admin changes — ordered against the incident window.
4. Sanity-check surprising zeros (zero domains) — usually a wrong field path or partial inspection; re-probe broadly. Admin-list findings are review inputs — "unrecognized admin, verify" phrasing.
5. Output: a compact table, source + dataprint age line, flags (unverified domains, seat/mailbox mismatch, continuity disabled). Offer a plain-text add_ticket_note. Degradation: no Proofpoint launchpoint → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify in console."
```
