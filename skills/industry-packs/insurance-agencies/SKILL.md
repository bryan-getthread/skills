---
name: Supporting Insurance Agencies
description: Vertical pack for independent insurance agency clients — the AMS (Applied Epic, EZLynx, AMS360, HawkSoft-class), carrier-portal sprawl and IVANS downloads, E&O sensitivity (the activity trail is legal evidence), and the renewal-season rhythm. Load when the client is an insurance agency or brokerage, or the ticket names an AMS, a carrier portal, or ACORD forms.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Insurance Agencies

An independent agency's entire value is a database: the book of business inside the AMS, and the documented trail proving what was quoted, bound, and told to whom. That trail is the agency's defense in an errors-and-omissions claim — which makes data integrity and audit history matter here in a way "restore from last night's backup" doesn't capture. This pack layers agency-specific knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is an independent insurance agency, brokerage, or MGA, or the ticket names Applied Epic, EZLynx, AMS360, HawkSoft, NowCerts, QQCatalyst, Applied TAM, or a comparative rater (EZLynx Rating, PL Rating-class)
- Carrier-portal problems: "I can't log into the carrier site," certificate/quote portals failing, IVANS download errors
- Renewal-season slowdowns, ACORD form or certificate-of-insurance generation failures
- Any question about restoring, purging, or bulk-editing AMS data

## The stack you'll meet

- **Agency management system (AMS):** Applied Epic, EZLynx, AMS360, HawkSoft, NowCerts, QQCatalyst-class — clients, policies, activities, attachments, accounting all live here. Older shops may run on-prem Applied TAM or AMS360 with a database server; the market has largely moved cloud/hosted, so many "AMS is slow" tickets are really bandwidth, browser, or vendor-side tickets.
- **Carrier portals — the sprawl:** an agency writes through dozens of carriers, each with its own portal, login, MFA scheme, and browser quirks. Password churn and portal breakage are the highest-volume ticket class. Many portal failures are carrier-side; recognize the boundary fast and say so plainly rather than debugging someone else's website.
- **IVANS / downloads:** carrier policy data flows into the AMS via IVANS-class download services. A stuck download queue means the AMS is quietly going stale — treat download failures as data-integrity tickets, not cosmetic ones.
- **Adjacent:** comparative raters, ACORD form generation, certificate-issuance workflows (a COI request from an insured's customer is often same-hour urgent), e-signature, VoIP with call recording (sometimes retained for E&O reasons — confirm before touching retention settings).

## E&O sensitivity — the activity trail is evidence

Agencies defend errors-and-omissions claims with their AMS activity log: timestamps, notes, attached correspondence. Practical consequences for the desk:

- Never bulk-delete, purge, or "clean up" AMS activities, notes, or attachments — even obvious duplicates — without the agency principal's written direction. What looks like clutter may be evidence.
- Data restores need extra care: restoring an AMS to an earlier point can silently erase activity records created since. Surface that consequence explicitly before any restore and get the principal's sign-off.
- Client data is regulated PII (names + policy + financial details; GLBA applies to insurance data). Minimum necessary in tickets — policy numbers over insured names where possible; no AMS screenshots showing client lists.

## The rhythm and what's sacred

- **Renewal clustering:** commercial books renew heavily at 1/1, 4/1, 7/1, 10/1 — the weeks before those dates are peak load, and an AMS or rater outage then hits every account manager at once. Personal-lines desks feel month-end pressure instead. Ask where the agency is in its renewal cycle when setting severity.
- **Certificate urgency:** contractors and vendors get stopped at job sites for want of a COI. A "can't issue certificates" ticket has a person standing somewhere unable to work — treat it as high urgency regardless of how small it looks.
- **Sacred:** the AMS database and its backup, the IVANS download chain, and the activity/attachment history. An agency that loses its AMS has lost its book and its E&O defense simultaneously.

## Steps

1. Context: search_tickets for this agency's history with the named system (portal quirks and rater fixes repeat), and search_itglue / search_hudu for the AMS flavor (on-prem vs hosted), version, vendor support contract, IVANS account details location, and the carrier-portal credential vault location.
2. Set severity on the agency clock: whole-agency AMS outage or download-chain failure in a renewal-cluster week → top priority; single-user portal issue with a documented carrier-side history → normal, with the carrier help desk handoff stated honestly.
3. Sort the boundary early: carrier-portal and rater-connectivity failures are frequently carrier/vendor-side — check the vendor's status page and this agency's ticket history before deep local debugging, and hand the user the right carrier contact when it's theirs.
4. Run the LOB framework for AMS problems: exact version client and server (on-prem AMS client/server mismatch after partial updates is a classic), change correlation, verbatim error, scope (one user / one workstation / everyone). Anything inside the AMS database is vendor territory — build the vendor-escalation package with case number.
5. For data operations (restore, merge, purge): state the E&O consequence, require the principal's explicit approval in writing, and record that approval in the ticket before proceeding.
6. Note in plain text: system + versions, scope, renewal-clock context, error verbatim (client-PII-scrubbed), boundary handoffs, vendor case number, and verification by the user running the real workflow (pull a client, run a quote, issue a certificate).

## Guardrails

- Never delete, purge, or bulk-edit AMS activities, notes, or attachments without written principal approval — the activity trail is E&O evidence.
- Any restore that could lose activity history gets the consequence stated and approved first; when in doubt, do nothing and escalate.
- Never operate directly on an on-prem AMS database outside vendor-documented procedure; the vendor-escalation package is the deliverable.
- Minimum-necessary client PII in tickets; no AMS screenshots showing books of business; GLBA-class data handling throughout.
- Do not store or handle carrier portal credentials ad hoc — they belong in the agency's documented vault; flag an undocumented credential sprawl as its own follow-up.
- Suspected client-data exposure → facts only, flag the agency's compliance/privacy owner; no legal or E&O advice from the desk.
- Docs tools vary per tenant — state what you could not verify, and flag missing AMS/IVANS documentation as a follow-up.
