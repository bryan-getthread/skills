---
name: Typosquat Domain Alert
description: A lookalike or typosquatted domain impersonating a client was reported or detected — gather registrar and DNS facts without visiting it, gauge attack capability, and draft the client warning.
category: Security
tools: [liongard_domain, search_clients, search_tickets, web_search, add_ticket_note, update_ticket, view_openDraft]
---

# Typosquat Domain Alert

Work a lookalike-domain report from "someone registered our-cl1ent.com" to a fact-based severity call and a client warning email — using passive sources only, never the domain itself.

## When to use

- A user or monitoring tool reports a domain that looks like the client's
- A phishing investigation surfaces a cousin domain and it needs its own workup
- A client asks "should we worry about this domain?"

## Steps

1. Capture the suspect domain exactly as written in the report. Do not visit it, resolve links on it, or fetch content from it — the workup is passive.
2. Gather facts: with Liongard enabled, use liongard_domain for the client's legitimate domain records and any visibility on the suspect (registrar, creation date, name servers, MX/SPF presence). Supplement with passive web_search for registration facts. Record the as-of date on everything.
3. Read the capability signals: MX records on the lookalike mean it can send and receive mail — that is email-attack capability and raises severity. A very recent creation date raises it further. Parked with no MX and no content history is watch-and-warn territory.
4. Check for active use: search_tickets for the suspect domain appearing in recent mail, phishing reports, or vendor-fraud tickets at the client. Active use converts this from a warning into a live phishing/BEC response — branch to phishing-triage or vendor-fraud-bec-alert for those threads.
5. Recommend actions: block the domain at the client's mail gateway and web filter; add it to monitoring/watchlists; prepare the evidence pack (registration facts, capability signals, any observed use) for a registrar abuse report or takedown — filing a takedown is a management and client decision, so package it, don't file it.
6. Draft the client warning email (present as a draft, human sends): what the domain is, that it closely resembles theirs and could be used to impersonate them, to verify full sender addresses character by character, and to verify any payment or banking change by phone using a number on file. Use defensive-writing-standard wording — a registered lookalike does not mean the client "was targeted" or compromised; say only what the facts support.
7. Document the decision, not just the action: facts gathered, capability read, recommendation, and what was sent to whom.

## Guardrails

- Never browse, screenshot, or interact with the suspect domain, and never click links containing it.
- Passive sources only for the workup; no probing or scanning.
- Don't overstate: registration alone is not an attack. Reserve stronger language for observed use, per the defensive-writing-standard.
- Takedown/abuse filings are prepared, not performed — management decides.
- Degradation: Liongard absent → registrar and DNS facts may be limited to passive search; state the visibility gap in the note instead of guessing record contents.

## Unattended (Flows) variant

- Follows the Unattended Output Discipline contract: the entire reply is the plain-text internal workup note posted verbatim — suspect domain (exactly as reported), registration facts with as-of dates, capability read (MX presence, creation date), severity call, and recommended next steps. No narration, no markdown.
- Deterministic inputs from the flow: the triggering ticket id containing the reported domain. No domain extractable verbatim from the report → output nothing; never reconstruct or guess a domain.
- Active use found in recent tickets → the note leads with `LIVE USE DETECTED - ESCALATE NOW` plus the evidence; the phishing/BEC response itself stays attended.
- Permitted writes: `add_ticket_note` only. The client warning email, status changes, and takedown packaging stay attended — client-facing wording is a human send.
- Passive only, in any mode: never visit, resolve, screenshot, or probe the suspect domain. Liongard absent → state the visibility gap inside the note instead of guessing records.
