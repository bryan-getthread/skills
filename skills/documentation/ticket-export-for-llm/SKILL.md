---
name: Ticket Export for LLM
description: Produce a clean, sanitized, self-contained export of a ticket for pasting into another AI tool — credentials and PII stripped, context preserved.
category: Documentation
tools: [search_tickets]
---

# Ticket Export for LLM

Package a ticket's full story into one clean text block another AI tool can work from,
with everything sensitive removed and everything technically relevant kept.

## When to use

- "Export this ticket so I can paste it into another AI tool."
- A tech wants a vendor's support chatbot or a specialized model to analyze the case.
- Handing an incident to an external analysis tool that only accepts pasted text.

## Steps

1. Pull the complete ticket with `search_tickets`: description, all replies and internal notes in order, time entries, status/priority history.
2. Build the export in this structure:
   - **Context header** — anonymized one-liner: environment type, product/system involved, priority, current status, ticket age.
   - **Problem statement** — the reported issue with exact error text preserved.
   - **Chronological thread** — each entry as `[T+<offset>] <role: user | technician | system>: <content>`, using relative times and roles instead of names.
   - **Technical facts** — versions, OS, configurations, and diagnostics mentioned.
   - **Current question** — what the tech wants the other tool to figure out (ask if not stated).
3. Sanitize aggressively before output:
   - **Delete entirely:** passwords, API keys, MFA codes, license keys, session tokens, credential-store references' contents.
   - **Replace with placeholders:** person names → `<user>`/`<technician>`, client/company names → `<client>`, email addresses → `<user>@<client-domain>`, hostnames → `<server-1>`, public IPs → `<public-ip-1>`, internal IPs kept only as subnet shape (`10.x.x.x`) when diagnostically relevant, phone numbers → `<phone>`, ticket/invoice numbers → `<ticket-ref>`.
   - Keep placeholders **consistent** (`<server-1>` is the same machine every time) so the receiving model can reason across the thread.
4. Do a second pass specifically for sensitive data quoted inside pasted logs, signatures, and email footers — the usual leak paths.
5. Output the export as one plain-text block, prefixed with a one-line note of what was redacted (categories only) so the tech knows what the other tool will not see.

## Guardrails

- Sanitization is the product: when unsure whether something identifies a person or client, redact it. Nothing credential-shaped ever survives, even partially masked.
- Preserve technical meaning — error messages, versions, and sequence of events stay verbatim; sanitize identity, not evidence.
- Read-only: never modify the ticket, and never send the export anywhere yourself — the tech pastes it.
- Remind the tech (one line, once) that the export leaves Thread's environment, so it must comply with the client's data-handling agreement — flag regulated-industry clients (medical, legal, financial content in thread) explicitly.
- Do not invent context to fill gaps; mark unknowns as `<not in ticket>`.
