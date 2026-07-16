---
name: Printer Connect Guide
description: Draft reply-ready instructions for an end user to add or reconnect the office printer, matched to the client's actual print setup — "send the user steps to add the printer."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Printer Connect Guide

Produces a client-ready instruction block for adding the office printer — written for how this client actually delivers printers (auto-deployed, print server, cloud print service like Universal Print/Printix/PaperCut, or direct), because "add a printer" means something different in each setup.

## When to use

- "Send <user> steps to add the printer near them."
- New desk / moved desk / new machine tickets where the printer didn't follow.
- "User can't find the printer in their list — send reconnect steps."

## Steps

1. **Identify the client's print delivery model first** (search_itglue / search_hudu / search_knowledge_base / search_tickets): (a) printers auto-deploy to managed machines, (b) users add from a print server, (c) a cloud print platform with its own client/portal, or (d) small-office direct/wifi printers. Also pull the printer's documented friendly name for the user's location. If the model or printer name is unknown, ask the technician ONE question — a guide for the wrong model sends the user hunting through menus that don't apply.
2. **Draft only the matching branch**, to end-user rules, one action per step with what-you'll-see cues:
   - **Auto-deploy client:** the draft is short — where printers appear (Settings → the printers list, phrased version-cautiously), the "give it a few minutes after sign-in on the office network" expectation, and the off-ramp. Do not include manual-add steps that fight the deployment.
   - **Print server client:** open the add-printer search, pick the documented printer by its exact friendly name ("choose the one called exactly <documented name> — names matter here"), wait for the driver cue ("your computer sets it up by itself; a test page option appears when done"). Never expose raw server paths unless docs show users add by path at this client.
   - **Cloud print platform:** the platform's user-side flow only (its portal/app, sign in with work credentials, select the printer for their floor), per the client's documentation for that product.
   - **Direct/wifi:** must be on the office network first; add from the printers list; cue the name/model match.
   - Universal off-ramps: "If the printer isn't in the list, stop and reply with where you sit — we'll map it from our side." / "If you're asked for a driver, a file, or an admin password, stop and reply — you should never need those."
3. Include the one-line success test: print a test page (where the flow offers it) or a one-page print from any document, and what to do if it queues forever ("if it says 'printing' for more than a couple of minutes with nothing coming out, reply — don't print it five more times").
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Never invent a printer name, share path, or IP address.** Docs or ask — a user adding a guessed printer prints to the wrong floor for a month.
- Delivery-model match is mandatory; manual-add steps on an auto-deploy client create duplicate broken queues.
- The don't-print-it-again line stays in every draft — repeat-printing a stuck job is the #1 way one ticket becomes twelve wasted pages.
- No admin steps (driver installs, server queue management, deployment policy) in the user block — that's troubleshooting-playbooks/printer-troubleshooting.
- Localizable; Windows/Mac cue differences noted only if the client has both platforms per docs.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled.

## Cross-references

- troubleshooting-playbooks/printer-troubleshooting — tech-facing when adding works but printing doesn't.
- communication/email-baseline-standard.
