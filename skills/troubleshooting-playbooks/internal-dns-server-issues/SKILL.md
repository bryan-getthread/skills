---
name: Internal DNS Server Issues
description: Diagnose AD-integrated DNS problems — machines resolving stale/wrong internal names, external resolution dead but internal fine (or vice versa), records vanishing overnight — distinct from public DNS/domain records, which the dns-domain-issues playbook owns.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, liongard_launchpoint, liongard_metric, liongard_timeline, web_search]
---

# Internal DNS Server Issues

This playbook is for the client's internal AD-integrated DNS servers. (Public records, registrars, SPF/MX → the dns-domain-issues and dmarc-spf-dkim-setup playbooks.) The rule: identify which server answered and what it answered (`nslookup` against a named server) before theorizing — "DNS is broken" is four different problems.

## When to use

- Internal names resolve to wrong/old IPs, or stop resolving after a weekend
- Internal resolution works but internet resolution fails from domain machines (or only at one site)
- Records for live machines keep disappearing ("it worked yesterday, the A record is gone")
- Client's internal domain overlaps a public one and some names go to the wrong place

## Steps

1. **History first.** search_tickets for the client on DNS symptoms and for recent changes: new DC, decommissioned DC, firewall/ISP change, scavenging "cleanup" someone enabled. Records vanishing on a cycle strongly suggests a scavenging change — find when it started.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the DNS design: which DCs run DNS, forwarder targets, internal domain name (is it a .local, a subdomain of the public domain, or — the trap — the same name as the public domain?), DHCP-DNS registration setup. If Liongard's Active Directory / DNS-capable inspectors run, use liongard_launchpoint + liongard_metric for zone and forwarder config, liongard_timeline for recent changes — state dataprint age.
3. **Get the evidence before theorizing.** Guide the tech: `ipconfig /all` on an affected machine (which DNS servers is it actually using — a machine pointed at the router or 8.8.8.8 explains "internal names don't resolve" instantly), then `nslookup <name> <specific-DC-IP>` against EACH internal DNS server to see whether they disagree, then the same lookup for an external name. Capture verbatim answers, not "it didn't work".
4. **Branch:**
   1. **Client pointed at the wrong resolver** — machine's DNS is the ISP/router/public. Domain members must use internal AD DNS only (public resolvers as secondary cause intermittent, maddening failures because there is no failback). Fix at DHCP scope options or the NIC, per client standard. Escalate when: DHCP is handing out the wrong DNS servers — pair with dhcp-server-issues.
   2. **Forwarder failure (internal OK, external dead)** — on the DNS server, test the configured forwarders directly (`nslookup <external-name> <forwarder-IP>`). Dead forwarder targets (old ISP resolvers, a decommissioned firewall) or blocked outbound 53 after a firewall change. Fix is updating forwarders to working, policy-approved targets; if root hints are the fallback, confirm outbound DNS isn't filtered. Escalate when: the firewall/ISP side is not yours to change.
   3. **Stale or missing records / scavenging accidents** — records for live machines gone, or ancient records answering. Read the zone's aging settings and the server scavenging state before judging: scavenging deletes records whose timestamp exceeds no-refresh+refresh — enabling it on a zone with old static-looking dynamic records mass-deletes on the first pass. Check whether the vanished records were dynamic (timestamped) or static, who registers them (machine vs DHCP "always dynamically update" with the DnsUpdateProxy setup), and whether refresh intervals fit the DHCP lease. The recovery is re-registration (`ipconfig /registerdns`, DHCP renewal), not hand-recreating hundreds of static records that will go stale again. Escalate when: scavenging design needs deciding — that is a change-management decision, propose settings, don't flip them.
   4. **Servers disagree (same query, different answers per DC)** — AD-integrated zone not replicating, wrong zone replication scope, or a stray secondary/manual zone on one server. This is replication territory — hand off to the ad-replication-issues playbook with the disagreeing servers and the query evidence.
   5. **Split-brain traps** — internal domain shares the name of (or overlaps) a public domain. Symptoms: internal users can't reach the public website ("www works but <domain> doesn't"), or a public service moved and internal DNS still answers with the old pinned record. Every public name used internally must be maintained by hand in the internal zone. Inventory which public records are shadowed before changing anything, and set honest expectations: this maintenance burden is permanent until the design changes. Never "fix" it by forwarding the apex zone externally — that breaks AD SRV lookups.
5. **Verify and note.** Success = the same nslookup that failed now returning the right answer from every internal server, and the end-user symptom gone. Plain-text note: symptom, which server answered what (verbatim), branch, change made or handed off, verification queries and time.

## Guardrails

- Never enable or "tune" scavenging as a quick fix — a wrong aging setting mass-deletes records. Propose settings with the math (no-refresh + refresh vs DHCP lease) and let the client's infra owner approve.
- Never delete records in an AD-integrated zone to test a theory — deletions replicate everywhere.
- Never point domain members at public DNS, even "temporarily as secondary" — it breaks AD in intermittent ways.
- All nslookup/ipconfig steps are guidance for the tech; no remote execution from here.
- If the problem is public records, registrar, or mail DNS — that's the dns-domain-issues / dmarc-spf-dkim-setup playbooks, not this one.
- Docs and Liongard coverage vary per tenant — note anything you could not check.
