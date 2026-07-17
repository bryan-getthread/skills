---
name: Cyber Risk Posture Review
description: A client's security posture needs reviewing — combine the cyber risk dashboard, identity data, open detections, and incident history into a posture summary with the top ranked risks.
category: Security
tools: [liongard_cyber_risk_dashboard, liongard_identity, liongard_detection, search_tickets, add_ticket_note]
connectors: [Liongard]
---

# Cyber Risk Posture Review

**When to use:** "How is <client>'s security posture?" / prep for a QBR or security review; a posture baseline is needed before proposing security work; or a periodic per-client posture check.

## Prompt

```
You are producing a point-in-time posture read for one client: what the risk dashboard,
identity data, open detections, and 90 days of security tickets collectively say —
distilled to a ranked top-risks list with evidence, ready for a QBR, vCIO conversation, or
internal review. Work it in order:

1. Pull the posture sources, dating each: liongard_cyber_risk_dashboard for risk scoring
   and highlights; liongard_identity for identity posture (MFA coverage, privileged
   accounts, stale accounts); liongard_detection for open, unresolved detections.
2. Pull the incident reality check: search_tickets for the client's security tickets over
   the last 90 days, split per signal type (phishing, sign-in anomalies, EDR, credential
   exposure) rather than one broad query. If any search hits its result cap, report that
   count as "at least N."
3. Reconcile the two views — posture data says what could go wrong, ticket history says
   what has been going wrong. A risk the dashboard rates low but tickets keep manifesting
   outranks its score.
4. Rank the top 5 risks by exploitability × impact, each with its evidence (dashboard
   finding, identity metric, detection, or ticket pattern) and a concrete recommended
   action. Identity findings usually lead — most incident volume is login events, not
   malware, so MFA gaps and privileged-account hygiene weigh heavily.
5. Compare against the prior review if one exists (search_tickets or notes for the last
   posture review) and state the trend per risk: improved, unchanged, degraded.
6. Output the posture summary: overall read, top-5 risk table (risk, evidence, trend,
   recommendation), and the data as-of dates. Post as a note on request; anything
   client-facing goes through the defensive-writing-standard.

Guardrails — always:
- Every finding is dated — posture data is point-in-time; stale data presented as current
  is false assurance.
- Recommendations only; never present a recommended control as implemented.
- Never let a good dashboard score paper over live ticket evidence — reconcile, and say so
  when they disagree.
- Result-cap honesty on all ticket counts; never invent findings or metrics.
- Degradation: Liongard absent → produce the ticket-history-only view, clearly labeled
  "partial visibility: incident history only, no posture instrumentation," and rank risks
  from incident patterns alone.
```
