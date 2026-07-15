---
name: Certificate Spoofing Alerts
description: A certificate-transparency or brand-monitoring alert reports a TLS certificate issued for a domain resembling (or claiming to be) a client's — verify whether the issuance is legitimate, gauge the threat, and run the registrar/CA follow-up path.
category: Security
tools: [search_tickets, search_clients, liongard_domain, web_search, add_ticket_note, update_ticket, view_openDraft]
---

# Certificate Spoofing Alerts

Certificate-transparency logs record every certificate publicly issued — which means monitoring them catches both attackers staging a lookalike phishing site and, far more often, the client's own marketing team standing up a microsite. This skill separates the two on evidence and packages the CA/registrar follow-up when it's real.

## When to use

- A cert-transparency monitor or brand-protection tool alerts on a certificate for a client-lookalike domain
- A certificate was issued for an actual client domain or subdomain and nobody recognizes it
- A typosquat investigation needs the certificate angle worked up

## Steps

1. Capture the alert facts exactly: the certificate's subject/SAN entries as written, issuing CA, issuance timestamp, validity period, and the monitored name it matched. Passive workup only — never visit the suspect domain or fetch content from it.
2. Classify which case this is, because the paths differ:
   - Certificate for a LOOKALIKE domain (client-name.co, cl1ent-name.com) → impersonation workup, step 4.
   - Certificate for the client's ACTUAL domain or subdomain that isn't recognized → legitimacy check, step 3 — potentially more serious, since it implies control of DNS or domain validation.
3. Legitimacy check for own-domain issuance: most "unknown" certificates are legitimate — SaaS platforms, CDNs, and load balancers auto-issue for hosted subdomains, and IT departments renew things. Verify before alarming: check the client's documented services and DNS (liongard_domain for records; search_tickets for recent projects mentioning the subdomain; the client's IT contact for "did anyone stand this up?"). Check whether a CAA record exists and whether the issuing CA is permitted by it. If NO legitimate explanation is found for an own-domain certificate → treat as possible domain/DNS compromise: verify registrar-account and DNS-host access integrity immediately (MFA, recent changes) via the technician, and escalate per security-alert-response tiering — this is High until explained.
4. Impersonation workup for lookalike domains: run the domain side through the typosquat-domain-alert skill (registration facts, MX capability, active-use check). The certificate adds signal on top: a fresh certificate means the site is being prepared to serve HTTPS — attackers ready pages before campaigns — which raises urgency versus a parked, cert-less registration. Check search_tickets for the domain already appearing in phishing or vendor-fraud reports; active use branches to phishing-triage / vendor-fraud-bec-alert.
5. Follow-up paths, packaged not fired:
   - Malicious own-domain issuance confirmed → the technician requests revocation from the issuing CA, DNS/registrar access is secured (rotate credentials, verify MFA, review recent changes), and affected-service certificates are reissued from a trusted path. Each action timestamped.
   - Lookalike with hostile indicators → evidence pack for the issuing CA's abuse/misissuance process and the registrar abuse report (certificate details, registration facts, capability signals, observed use). Filing takedowns/abuse reports is a management-and-client decision — package it, don't file it.
   - Preventive close-out either way: recommend CAA records constraining which CAs may issue for the client's domains, and certificate-transparency monitoring for their real domains if this alert came from a third party.
6. Client communication where warranted, drafted per defensive-writing-standard (view_openDraft, human sends): what was observed, the verdict, what was done or recommended. A lookalike certificate is "a certificate was issued for a domain resembling yours" — not "you are under attack." Document the decision trail in a plain-text note.

## Guardrails

- Passive only: never browse, probe, or fetch from suspect domains or the certificate's endpoints; facts come from CT data, DNS records, and passive search.
- Own-domain-unknown certificates are High-until-explained — but explained is the common outcome, so verify with the client before escalating language beyond "unrecognized issuance under review."
- Revocation requests, abuse filings, and takedowns are prepared as evidence packs; management and the client decide, the technician executes.
- Don't overstate per the defensive-writing-standard: issuance alone is preparation-capability, not an attack; reserve stronger language for observed use.
- No fabrication of certificate details — quote the alert/CT record exactly; if a field wasn't in the alert, mark it unavailable.
- Degradation: without liongard_domain, DNS and CAA facts come from passive web_search or the technician's dig output — record the source and its timestamp.
