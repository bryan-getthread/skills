---
name: Printer Scan to Email Guide
description: Draft reply-ready instructions for an end user to use scan-to-email at an office multifunction printer — "tell the user how to scan a document to their email at the copier."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Printer Scan to Email Guide

Produces a client-ready instruction block for a user standing at the office multifunction printer (MFP) who needs to scan a document to email — written for the client's actual device model and their scan flow, since the touchscreen differs on every brand.

## When to use

- "User needs to scan a document to their email — send them how at the copier."
- "How do I scan to email on the office printer?"
- "User's scan isn't arriving in their inbox — send the correct scan-to-email steps."

## Steps

1. **Identify the client's MFP and scan model first** (search_itglue / search_hudu / search_knowledge_base / search_tickets): the device brand/model (Xerox, Canon, Ricoh, Konica, HP, etc.) and — the decisive question — whether scan-to-email works by (a) picking the user from an on-device **address book**, (b) the user **typing their email address** on the touchscreen, or (c) a "**scan to my email**" login (badge/PIN sign-in that knows who they are). Note any known constraint (attachment size cap, must be on the office network). If the model or scan flow is unknown, ask the technician ONE question — every brand's touchscreen is laid out differently.
2. **Draft the instruction block** to end-user rules, one action per step with what-you'll-see cues, generic enough to survive brand differences but matching the identified flow:
   - Physical first: place the document — face-up in the top feeder for multiple pages, or face-down on the glass for a single/bound page. Cue the paper guides.
   - Wake/sign in at the panel if the device requires a badge or PIN (only if that's their model).
   - Choose **Scan** / **Email** on the touchscreen (cue the icon), then the recipient step per the identified flow: pick yourself from the address book / type your email / it's already you if signed in.
   - Set the basics if offered: color vs. black-and-white, PDF format, single vs. double-sided. Keep this to the few options that matter.
   - Press **Start/Scan**, wait for the "sent" confirmation on the panel, and remove the original.
   - The success + expectation note: "the scan arrives as a PDF attachment — check your Inbox and your Junk folder, and note very large scans can take a few minutes or bounce for size."
   - Off-ramps: "If your name/email isn't in the list, or typing it isn't offered, stop and reply — we'll add you." / "If it says sent but nothing arrives, reply and we'll check the scan settings."
3. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Model/flow match matters most here** — MFP touchscreens are brand-specific; a "type your address" walkthrough sent to an address-book-only device dead-ends the user. Verify or ask.
- Keep panel cues generic (icons, "roughly named") rather than pixel-precise, since firmware updates rebrand buttons.
- The "check Junk, and big scans may bounce" note stays in every draft — the top reason a scan "doesn't arrive."
- No admin steps (SMTP/scan-to-email server config on the device, adding address-book entries, firmware) in the user block — those are tech actions; the draft routes there when the user isn't in the address book.
- For sensitive documents, the sender should still consider whether email is the right channel — but that's a judgment note, not a block.
- Localizable; version-cautious panel cues.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled; fall back to KB and ticket history.

## Cross-references

- end-user-guides/printer-connect-guide — the "connect to the printer to print" counterpart.
- end-user-guides/large-file-share-guide — when the scan is too big to email and should be shared as a link.
- communication/email-baseline-standard.
