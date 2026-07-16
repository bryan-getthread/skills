---
name: Client Onboarding Documentation Pack
description: Define and capture the full documentation set a newly onboarded client needs — contacts, environment, access, escalation, and quirks — as a checklist of what to gather, from where, and where each piece is filed.
category: Documentation
tools: [search_clients, search_contacts, search_tickets, search_itglue, search_hudu, search_knowledge_base, notion-search, notion-create-pages, create_ticket, add_ticket_note]
---

# Client Onboarding Documentation Pack

Turns "we just signed <client>" into the concrete set of documents the desk must have on file before it can support them well — and a capture plan for who gathers what, from where, and where it lands. The outcome is a scoped checklist plus paste-ready skeletons, not invented content.

## When to use

- A new client has been signed and onboarding is being planned.
- "What do we need to document for <client> before we go live?"
- An acquired client book arrived with thin or no documentation and needs a baseline.
- A support review found a client the desk has supported for months with no proper doc pack.

## Steps

1. Establish the client record with `search_clients` and confirm the scope (single site vs. multi-site, headcount, cloud/on-prem/hybrid as far as known). Check what already exists so this becomes "fill the gaps," not "start from zero": `search_itglue` / `search_hudu` (where enabled), `search_knowledge_base`, and `notion-search` (if the member has connected Notion).
2. Build the **documentation pack checklist** — the sections a complete pack contains, each marked present / partial / missing against what step 1 found:
   - **Client profile** — what they do, size, sites, hours of operation, industry/compliance context.
   - **Key contacts** — primary contact, change/spend approver, escalation contact, VIPs, after-hours contact; role and preferred contact method each (see documentation/client-wiki-page for the readable one-pager).
   - **Environment overview** — network shape, ISPs, firewall/switching/wireless, servers and hypervisors, endpoints, cloud tenants (M365/Google), line-of-business apps. Point the deep network capture at documentation/network-documentation-standard.
   - **Access & identity** — how staff authenticate, admin/tenant accounts (by reference, not secrets), where credentials are vaulted, MFA posture.
   - **Vendors & contracts** — ISP, hardware, and software vendors with support-contract and escalation details (see documentation/vendor-contact-directory).
   - **Warranties & licenses** — hardware warranties and software licensing with renewal dates (see documentation/warranty-license-registry).
   - **Backup & DR** — what is backed up, where, retention, and the recovery plan (see documentation/disaster-recovery-plan-doc).
   - **Escalation matrix** — internal and vendor who-to-call ladder (see documentation/escalation-matrix-doc).
   - **Runbooks** — new-hire and leaver runbooks specific to this client's stack (see documentation/onboarding-runbook-per-client and documentation/offboarding-runbook-per-client).
   - **Quirks & standing rules** — environment oddities, maintenance windows, and handling rules the desk must respect.
3. For each missing/partial section, assign a **source and owner** on the checklist: what to gather (from the client's IT contact, from a discovery/site visit, from tenant admin reads, from ticket history) and who on the desk owns capturing it. Do not fill sections from assumption — an unknown stays "to capture," never inferred.
4. Set the **filing convention** explicitly: each finished piece goes to the desk's real documentation platform under the client's section, in the location convention already used for other clients. Confirm the convention with the requester rather than inventing folders.
5. Output the checklist (present/partial/missing + source + owner per section) plus paste-ready skeleton headings for each document. If the desk keeps client docs in Notion and the requester confirms the destination, create the pack's index page and section skeletons with `notion-create-pages`; otherwise deliver paste-ready blocks. If onboarding is being tracked as tickets, create the capture tasks via `create_ticket` (or `add_ticket_note` on an existing onboarding ticket) with each section's capture instruction embedded.

## Guardrails

- **Structure and capture plan, not fabricated content.** This skill defines what a complete pack contains and where the facts come from — it never invents contacts, topology, licenses, or contracts. Every populated fact must trace to a source; unknowns stay flagged "to capture."
- **No credentials in the pack.** Reference the vault entry by title only; secrets never land in the doc pack, checklist, notes, or tickets (see documentation/credential-storage-audit).
- Docs tools are search-only. Writing lands in Notion only when the member has connected it and the destination is confirmed; otherwise emit paste-ready blocks. Never auto-file into IT Glue/Hudu — those are read surfaces here.
- Notes and embedded checklists are plain text per the Note Format Standard (documentation/note-format-standard) — they may sync to a PSA.
- Degradation: without IT Glue/Hudu, run the existing-docs check against the Thread KB and requester knowledge only, and note the narrower gap analysis. Without Notion connected, everything is paste-ready output.
