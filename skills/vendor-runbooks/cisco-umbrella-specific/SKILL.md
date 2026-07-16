---
name: Cisco Umbrella
description: A Cisco Umbrella block event, category complaint, or allow/policy request arrived — separate security-category blocks (infection signal) from content-policy blocks, and handle Umbrella's policy-precedence and destination-list discipline. Verify against Cisco's current Umbrella documentation.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# Cisco Umbrella

Vendor specialization of dns-filtering-alerts for Cisco Umbrella. dns-filtering-alerts owns the core discrimination (a security-category block is a detection; a content-category block is a policy event) and the bypass/allow discipline; this skill adds Umbrella's policy model — ordered policies, identities, destination lists, and its Intelligent Proxy. Verify category names and console layout against Cisco's current Umbrella documentation.

## When to use

- An Umbrella security-category block fires (an endpoint tried a malware/C2/phishing/DGA domain)
- A user or client asks "why is this site blocked by Umbrella?" or requests an allow
- An Umbrella policy or destination-list change needs a decision

## Steps

1. Classify the event first, per dns-filtering-alerts: security block (Malware, Command and Control, Phishing, DNS Tunneling/VPN, Newly Seen Domains) vs content/category block (the client's Acceptable-Use policy) vs an application (AppAware) block. The category name and the policy that matched are both in the Activity Search / report — record which policy fired.
2. Security-block path — treat as a detection, not a success:
   - Identify the source identity. Umbrella attributes activity to the matched identity (Roaming Client / AnyConnect module, network, network device, or AD user/computer). If only the network egress is known, the technician resolves the internal source from the Roaming Client report or DHCP/firewall logs — say so if attribution is unavailable.
   - One-off phishing-domain lookup → likely a clicked link: run phishing-triage on how the URL arrived; the block stopped the page, but check for sibling deliveries.
   - Repeated/periodic C2/malware lookups → beaconing: treat the endpoint as suspect via edr-detection-runbook; do NOT close because "Umbrella blocked it." The block contains the symptom, not the infection.
   - Prior context via search_tickets (same identity/domain class, 90 days).
3. Content-block complaint path:
   - Umbrella policies are ordered and evaluated top-down by identity — a user hitting the "wrong" rule is often a policy-precedence issue (a broader policy above a narrower one), not a miscategorization. Check which policy and which identity matched before assuming the category is wrong.
   - Vendor miscategorization → submit a domain reclassification to Cisco plus, if business-urgent, a narrow temporary allow.
   - Correctly categorized but business-needed → client policy decision: route to the client's authorized approver on file; the desk does not loosen a client's own AUP on a user's request.
4. Allow/policy discipline — Umbrella destination lists:
   - Allows go in a destination list scoped to the narrowest identity that needs them (a specific roaming identity or network, not global), exact domain over wildcard, time-boxed where temporary, named client approver recorded, review date set.
   - NEVER add a security-category domain to an allow destination list — request Cisco reclassification with evidence instead. An allow can override a security block, which is exactly why it must never be used to end a malware-class complaint.
5. Recurring vendor false positives (CDNs/ad networks tripping security categories) → security-noise-tuning with the same narrow-allow discipline. Document event class, matched policy/identity, source attribution, verdict, and any allow's scope/approver/expiry; classify security-class events per soc-classification-tree.

## Guardrails

- "Umbrella blocked it" is not "resolved" for security categories — repeated C2 lookups mean an infected endpoint; investigate the source.
- Never add a security-category domain to an allow/destination list; reclassification requests go to Cisco with evidence.
- Check policy precedence and matched identity before calling a content block a miscategorization — ordered policies explain most "wrong" blocks.
- Content-policy changes belong to the client's authorized approver, not the requesting user.
- Every destination-list allow gets the narrowest identity scope, an owner, and a review date; an allowlist that only grows is a policy that no longer exists.
- Identity check before user-specific allows — a "please unblock this for me" from a compromised mailbox is a real pattern.
- Console changes are technician actions the agent directs and records; degradation — if the desk lacks console access, name exactly what the tech should pull (Activity Search, matched policy, source identity, category verdict).
