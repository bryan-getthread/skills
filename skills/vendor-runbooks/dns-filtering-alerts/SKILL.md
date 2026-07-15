---
name: DNS Filtering Alerts
description: A DNS-filter block event or category complaint arrived (Cisco Umbrella, DNSFilter, or similar) — separate security blocks (possible infection signal) from category blocks (policy), and keep bypass/category-change discipline.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# DNS Filtering Alerts

Vendor runbook for DNS-layer filtering products (Cisco Umbrella, DNSFilter, and equivalents). The core discrimination: a security-category block (malware, C2, phishing, DGA) is a detection — something on the endpoint tried to go there — while a content-category block is a policy event. Conflating them either buries an infection signal in web-filter noise or turns a policy complaint into a false alarm. Verify category names against the vendor's current taxonomy.

## When to use

- A DNS-filter security alert lands (endpoint attempted a malware/C2/phishing domain)
- A user or client asks "why is this site blocked?" or requests unblocking
- A category-change or bypass request needs a decision

## Steps

1. Classify the event first: security block (malware, command-and-control, phishing, cryptomining, newly-seen/DGA domains) vs content/category block (social media, streaming, gambling — the client's policy) vs uncategorized-domain block.
2. Security-block path — treat as a detection, not a success story:
   - Identify the source: which device/user made the lookup (roaming client or network egress — if only the site's egress IP is known, the technician identifies the internal source from the filter's console or DHCP/firewall logs; say so if attribution is unavailable).
   - One-off lookup to a phishing domain → likely a clicked link: run phishing-triage on how the user got the URL; the block prevented the page, but check for sibling deliveries.
   - Repeated/periodic lookups to C2/malware domains → beaconing pattern: treat the endpoint as suspect — edr-detection-runbook on that device; do NOT close because "it was blocked." The block is containment of the symptom, not the infection.
   - Prior context via search_tickets (same device/domain class, 90 days) for recurrence.
3. Category-block complaint path:
   - Confirm the block reason and the client's documented filtering policy (search_itglue). If the site is miscategorized by the vendor → the fix is a category-dispute/recategorization request to the vendor plus, if business-urgent, a narrow temporary allow.
   - If correctly categorized but business-needed → this is a client policy decision, not a desk favor: route to the client's authorized approver on file. The desk does not loosen a client's own policy on a user's request.
4. Bypass/allow discipline: narrowest scope (exact domain over wildcard, specific user/site over global), time-boxed where the need is temporary, named client approver recorded, and a review date. Never bypass a security category — if someone insists a malware-class block is wrong, escalate the domain for vendor recategorization instead of allowing it.
5. Recurring vendor false positives (CDNs, ad networks tripping security categories) → security-noise-tuning with the same narrow-allow discipline.
6. Document: event class, source attribution, verdict, any allow's scope/approver/expiry. Console changes are technician actions the agent directs and records. Classify per soc-classification-tree for security-class events.

## Guardrails

- "Blocked" is not "resolved" for security categories — repeated C2 lookups mean an infected endpoint; investigate the source, never just the domain.
- Never create an allow for a security-category domain; recategorization requests go to the vendor with evidence.
- Content-policy changes belong to the client's authorized approver, not the requesting user — verify authorization per the client's documentation.
- Every allow entry gets an owner and a review date; an allowlist that only grows is a policy that no longer exists.
- Identity check before user-specific bypasses — a "please unblock this for me" from a compromised mailbox is a real pattern.
- Degradation: if the filter's console isn't accessible to the desk, name exactly what the tech should pull (query logs, source identity, category verdict) rather than inferring from the ticket alone.
