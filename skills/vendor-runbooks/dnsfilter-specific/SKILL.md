---
name: DNSFilter
description: A DNSFilter block event, category complaint, or allow request arrived — separate security-category blocks (infection signal) from content-policy blocks, resolve source identity through the Roaming Client, and keep allow/block-list discipline. Verify against DNSFilter's current documentation.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# DNSFilter

Vendor specialization of dns-filtering-alerts for DNSFilter. dns-filtering-alerts owns the core discrimination (a security-category block is a detection; a content-category block is a policy event) and the bypass/allow discipline; this skill adds DNSFilter's specifics — policies, the Roaming Client agent for off-network devices, threat categories, and allow/block lists. Verify category names and console layout against DNSFilter's current documentation.

## When to use

- A DNSFilter security-category block fires (an endpoint tried a malware/C2/phishing/newly-registered domain)
- A user or client asks "why is this site blocked?" or requests an allow, including block-page reports
- A DNSFilter policy or allow/block-list change needs a decision

## Steps

1. Classify the event first, per dns-filtering-alerts: security block (Malware, Botnet/C2, Phishing & Deception, Cryptomining, New Domains/newly-registered) vs content-category block (the client's policy) vs an app block. DNSFilter's Query Log / block-page report shows the domain, the category, the matched policy, and the source — record which policy fired.
2. Security-block path — treat as a detection, not a success:
   - Resolve the source identity. On-network activity attributes to the site/network; off-network devices attribute through the **Roaming Client** agent (per-device). If only a network egress IP is known, the technician resolves the internal source from the Roaming Client report or DHCP/firewall logs — say so if attribution is unavailable. Roaming Client coverage is the specific thing to confirm: a device without the agent installed is unprotected off the network — flag coverage gaps.
   - One-off phishing-domain lookup → likely a clicked link: run phishing-triage on how the URL arrived; the block stopped the page, check for sibling deliveries.
   - Repeated/periodic C2/malware lookups → beaconing: treat the endpoint as suspect via edr-detection-runbook; do NOT close because "DNSFilter blocked it." The block contains the symptom, not the infection.
   - Prior context via search_tickets (same device/domain class, 90 days).
3. Content-block complaint path:
   - Confirm the matched policy and the client's documented filtering policy (search_itglue). Vendor miscategorization → submit a domain reclassification/review to DNSFilter plus, if business-urgent, a narrow temporary allow.
   - Correctly categorized but business-needed → client policy decision: route to the client's authorized approver on file; the desk does not loosen a client's own policy on a user's request.
4. Allow/block-list discipline: allows go on the policy/organization allow list at the narrowest scope (exact domain over wildcard, the specific policy/site that needs it over global), time-boxed where temporary, named client approver recorded, review date set. NEVER allow-list a security-category domain — request DNSFilter reclassification with evidence instead; an allow overrides the security block, which is exactly why it must not end a malware-class complaint.
5. Recurring vendor false positives (CDNs/ad networks tripping security categories) → security-noise-tuning with the same narrow-allow discipline. Document event class, matched policy, source attribution (and Roaming Client coverage state), verdict, and any allow's scope/approver/expiry; classify security-class events per soc-classification-tree.

## Guardrails

- "DNSFilter blocked it" is not "resolved" for security categories — repeated C2 lookups mean an infected endpoint; investigate the source.
- Never allow-list a security-category domain; reclassification requests go to DNSFilter with evidence.
- Roaming Client coverage is the off-network protection boundary — a device without the agent is unprotected off-network; flag gaps rather than assuming coverage.
- Content-policy changes belong to the client's authorized approver, not the requesting user.
- Every allow gets the narrowest scope, an owner, and a review date; an allowlist that only grows is a policy that no longer exists.
- Identity check before user-specific allows — a "please unblock this for me" from a compromised mailbox is a real pattern.
- Console changes are technician actions the agent directs and records; degradation — if the desk lacks console access, name exactly what the tech should pull (Query Log, matched policy, source/Roaming Client identity, category verdict).
