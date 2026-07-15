---
name: Vendor Escalation Package
description: Assemble everything a third-party vendor's support team needs to work a case — environment, reproduction steps, timeline, diagnostics, and the contract or entitlement reference — with credentials stripped.
category: Escalation
tools: [search_tickets, add_ticket_note, search_knowledge_base, search_itglue, search_hudu, web_search]
---

# Vendor Escalation Package

When a ticket has to go to the software or hardware vendor, this builds the case package that gets it past the vendor's L1 in one pass: their intake questions answered before they're asked, in a copy-paste-ready block.

## When to use

- "Open a case with the vendor for this" / "prep this ticket for <vendor> support."
- A ticket has hit the limit of what the desk can do and vendor support is the next tier.
- The vendor bounced a case for missing information and it needs to be resubmitted properly.

## Steps

1. Read the full ticket thread and pull environment context: affected product, exact version/build, OS and platform, deployment shape (cloud/on-prem, single/multi-site), and affected scope at <client>. Use search_itglue / search_hudu for documented environment details where those integrations are enabled; otherwise ask the technician.
2. Build reproduction: numbered steps to reproduce (or "intermittent — occurs roughly X times per day since <date>"), expected vs actual behavior, and exact error text verbatim.
3. Collect diagnostics referenced in the ticket — log excerpts, screenshots, diagnostic outputs. List each item and where it lives; flag anything the vendor will predictably ask for that is missing (e.g. a support bundle) so the tech gathers it before submitting.
4. Find the support entitlement: contract number, support agreement level, customer/site ID with the vendor, and licensed-under name — from documentation (search_itglue / search_hudu / search_knowledge_base). If not documented, leave the field blank and flag it; do not guess. Use web_search only for the vendor's public case-submission process, never for entitlement data.
5. Scrub the package: remove passwords, API keys, tokens, and internal-only remarks from everything outgoing — including inside pasted log excerpts (mask as <redacted>). Replace internal jargon with plain language a vendor can follow.
6. Assemble in vendor-friendly order: Contact & entitlement / Environment / Problem summary / Repro steps / Timeline / Diagnostics attached / What we've ruled out / Impact & requested priority.
7. Output the package as a copy-paste block for the vendor portal or email. Offer to also post it as a plain-text internal note (with a placeholder for the vendor case number once assigned) — post only on confirmation. Do not submit to the vendor yourself.

## Guardrails

- Credentials never leave: no passwords, keys, tokens, or internal admin URLs in the outgoing package — even embedded in logs.
- Entitlement and contract references come from documentation only; a blank field beats a wrong contract number.
- Error text is quoted verbatim, never paraphrased — vendors match on exact strings.
- "What we've ruled out" may only list steps actually documented in the ticket.
- If IT Glue / Hudu are not enabled for this tenant, gather environment and entitlement details from the technician and say which fields are unverified.
