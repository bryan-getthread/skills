---
name: Vendor Contact Directory
description: Build and maintain a per-client directory of vendors, support contracts, and escalation contacts — so when a vendor-dependent issue hits, the desk has the account number, support line, and contract tier on file instead of hunting.
category: Documentation
tools: [search_clients, search_tickets, search_itglue, search_hudu, search_knowledge_base, notion-search, notion-create-pages, notion-update-page, add_ticket_note]
---

# Vendor Contact Directory

Assembles the sheet a tech needs the moment an issue depends on a third party: who the vendor is, how to reach their support, the account/contract number, the support tier and hours, and the named escalation contact. The outcome is a structured directory sourced from real records — never invented account numbers or phone lines.

## When to use

- "Who do we call at <vendor> for <client>, and what's the account number?"
- Onboarding a client and building the vendors section of the doc pack (see documentation/client-onboarding-documentation-pack).
- A vendor outage or escalation exposed that nobody had the support-contract details to hand.
- Periodic refresh — verifying support contracts and contacts are still current.

## Steps

1. Establish the client with `search_clients`, then gather the vendor set from evidence: `search_itglue` / `search_hudu` (where enabled) and `notion-search` (if connected) for existing vendor records; `search_tickets` for vendors that actually appear in the client's history (ISP, hardware, software, security, backup, print, phone/voice, LOB-app vendors); `search_knowledge_base` for any documented vendor procedures.
2. For each vendor, capture the **directory row** from records — leave any field unknown rather than guessing:
   - **Vendor & role** — name and what they provide for this client (ISP, firewall vendor, backup platform, LOB app, etc.).
   - **Support contact** — support phone/portal/email and hours of coverage.
   - **Account / contract number** — the client's account or contract reference with this vendor (the lookup key support will ask for).
   - **Contract tier & terms** — support level (e.g. next-business-day vs. 24x7), coverage scope, and renewal/expiry where known (cross-ref documentation/warranty-license-registry for the renewal-tracking view).
   - **Named escalation contact** — the account manager / TAM / escalation path beyond first-line support, where one exists.
   - **How the desk authenticates to the vendor** — by reference (e.g. "PIN in vault entry <title>") — never the actual PIN or credential.
   - **Source** — the ticket number or doc the detail came from.
3. Flag gaps explicitly: vendors that appear in ticket history but have no contract details on file, and contract/renewal dates that are missing or stale — these become capture follow-ups, not blanks.
4. Set the **storage discipline**: the directory lives under the client's section in the desk's documentation platform, dated, with a review cadence. Confirm the location convention rather than inventing one.
5. Output the directory as a paste-ready table plus the gap list. If the desk keeps vendor directories in Notion and the destination is confirmed, create it with `notion-create-pages` or refresh the existing page with `notion-update-page`; otherwise deliver paste-ready blocks. Attach to an onboarding/review ticket via `add_ticket_note` if requested.

## Guardrails

- **Sourced facts only** — account numbers, phone lines, and escalation contacts must trace to a record or ticket. Never invent a support number, account number, or contact; unknowns are flagged for capture.
- **No credentials in the directory** — vendor PINs, portal passwords, and account passwords go to the vault and are referenced by entry title only (see documentation/credential-storage-audit).
- Maintaining the directory means verifying entries are current — a stale support number is worse than a flagged gap; mark last-verified dates.
- Notes and tables are plain text per the Note Format Standard (documentation/note-format-standard) — they may sync to a PSA.
- Docs tools are search-only; writes go to Notion only when the member has connected it and confirmed the destination, otherwise paste-ready output. Degradation: without IT Glue/Hudu, build from ticket history and requester knowledge and note the directory is partial.
