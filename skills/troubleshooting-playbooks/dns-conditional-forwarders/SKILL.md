---
name: DNS Conditional Forwarders
description: Diagnose internal↔external name-resolution problems that hinge on conditional forwarders and split-brain DNS — a zone resolving one way inside and another outside, forwarders pointing at dead resolvers, and stale scavenging edge cases — from actual query traces, never by flushing and hoping. Broader DNS lives in internal-dns-server-issues / dns-domain-issues.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# DNS Conditional Forwarders

This playbook is for the specific class of DNS problems that live at resolution boundaries: a conditional forwarder sending a domain's queries to the wrong (or dead) resolver, split-brain zones where the internal answer must differ from the public one, and scavenging edge cases that delete or resurrect records. It works from `nslookup`/`Resolve-DnsName` traces against specific servers — not from flushing caches and hoping. General internal-DNS and public-domain DNS have their own playbooks.

## When to use

- A partner/vendor/other-AD-domain name resolves externally but not internally (or vice-versa)
- A name that must point *inside* (split-brain: same FQDN, internal IP) resolves to the public IP or the reverse
- Conditional forwarding to a partner domain, Azure private zones, or a merged/acquired domain stopped working
- Intermittent resolution failures, or records that vanish/reappear (scavenging suspicion)

## Steps

1. **Resolution design first.** search_itglue / search_hudu / search_knowledge_base for the DNS layout: the internal DNS servers (usually AD DCs), the forwarders and **conditional forwarders** configured (which domains go to which resolvers), any **split-brain** zones (an internal zone deliberately overriding a public domain), Azure/cloud private-DNS or private-resolver in the path, and the scavenging settings (aging/no-refresh/refresh intervals). Map which domains are supposed to resolve where before testing. If a Liongard AD/DNS inspector runs, corroborate zone/forwarder config via liongard_launchpoint / liongard_metric and note dataprint age.
2. **History first.** search_tickets for this client + DNS: a recent domain merger/trust, an Azure/cloud DNS change, a resolver IP change at a partner, a DC promotion/demotion, or a scavenging incident. A conditional forwarder that "broke" often points at a resolver IP that changed on the other end.
3. **Trace the actual query before flushing anything.** `nslookup <name> <specific-DNS-server>` / `Resolve-DnsName -Server` against each internal DNS server, and compare with the same query against a public resolver — the *difference* between internal and external answers is the whole diagnosis for split-brain and forwarding. Note whether you get NXDOMAIN (no such name), SERVFAIL (the resolver failed/timed out — often a dead forwarder target), or a *wrong* answer (split-brain/override). Read the trace — flushing before you've captured the answer destroys the evidence.
4. **Branch:**
   1. **Conditional forwarder to the wrong/dead resolver** — a specific domain SERVFAILs or times out internally: the conditional forwarder for that domain points at a resolver that's down, changed IP, or no longer authoritative (common after a partner/acquired-domain change or an Azure resolver move). Confirm the target resolver actually answers for that domain from the DC, then correct the forwarder's IP(s). For AD-integrated conditional forwarders, replication of the forwarder to all DCs matters — a forwarder present on one DC but not another causes intermittent failures.
   2. **Split-brain not overriding (or over-overriding)** — the internal answer should differ from public but doesn't: confirm the internal zone that overrides the public FQDN exists and holds the intended record, and that clients query the internal DNS (not a public resolver via DoH/hardcoded 8.8.8.8, or a VPN/DNS-suffix quirk). A missing internal record for a split zone falls through to nothing (NXDOMAIN) because the internal zone is authoritative and *won't* forward — so the record must exist internally. The reverse (internal leaking the public IP) is usually a client not using internal DNS.
   3. **Forwarding vs authoritative confusion** — the server won't forward a domain it thinks it's authoritative for: if an internal primary zone exists for a domain (or subdomain), the DC answers authoritatively and never forwards, even if a conditional forwarder also exists. Check for an unintended internal zone shadowing the domain. Precedence: authoritative zone > conditional forwarder > general forwarder > root hints.
   4. **Scavenging edge cases** — records vanishing or resurrecting: aging/scavenging with mismatched no-refresh/refresh intervals, scavenging enabled on multiple servers, or static records without proper timestamps can delete live records or let stale ones linger. This is a careful, low-blast-radius area — read the current settings and the specific record's timestamp before changing intervals; a scavenging misconfiguration can delete many records at once. Escalate the interval design rather than tweaking under pressure.
5. **Verify and note.** Success is the query returning the intended answer from the internal servers (and, for split-brain, the correct *different* answer inside vs outside), confirmed by a fresh trace after any cache flush. Plain-text note: the internal-vs-external trace, NXDOMAIN/SERVFAIL/wrong-answer classification, branch, action or handoff, verification.

## Guardrails

- No remote execution — nslookup/Resolve-DnsName and DNS console steps are guidance for a tech with DNS-admin access; use get_ninjaone_device_link to reach a DC/client when the RMM integration is enabled.
- Capture the query trace *before* flushing caches — flushing first destroys the evidence you need.
- Scavenging changes have wide blast radius (they can delete live records) — read current intervals and the specific record's timestamp first, and escalate interval redesign rather than adjusting reactively.
- Don't add a general forwarder or a permissive zone to "make it resolve" — fix the specific conditional forwarder/zone; a wrong authoritative zone silently shadows forwarding.
- Conditional forwarders and zones on AD-integrated DNS replicate — verify the change is consistent across all DCs, not just the one you edited.
- Do not invent forwarder precedence, scavenging math, or cmdlet syntax — web_search Microsoft's current DNS docs and cite.
- Docs/Liongard coverage varies per tenant — note what you couldn't check and dataprint age. Notes destined for a PSA sync: plain text, no markdown or emojis.
