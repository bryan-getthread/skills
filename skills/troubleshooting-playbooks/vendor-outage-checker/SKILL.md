---
name: Vendor Outage Checker
description: When a ticket smells like a vendor or service outage (M365, ISP, SaaS app), check the vendor's status page and outage reports on the web and post sourced findings to the ticket — the commonly requested "is it just us or is it down for everyone?" workflow.
category: Troubleshooting Playbooks
tools: [web_search, add_ticket_note, search_tickets, run_assistive_ai]
---

# Vendor Outage Checker

Gather external evidence for a suspected vendor outage — the vendor's own status page plus independent outage reports — and post timestamped, sourced findings to the ticket so the tech stops troubleshooting a problem that isn't theirs. Complement of `triage-and-routing/cross-client-outage-detector`: that skill reads the *internal* signal (same symptom across our clients); this one fetches the *external* evidence half. Run both when a widespread issue is suspected.

## When to use

- A ticket reports "email is down", "Teams won't load", "the internet is out" and the symptom pattern suggests the vendor, not the client's environment.
- A tech asks "is M365 / <SaaS app> / the ISP having an outage right now?"
- A Flow fires on tickets matching outage-flavored keywords and the desk wants status-page evidence attached automatically.

## Steps

1. Identify the suspected vendor/service from the ticket text (M365 workload, ISP, LOB SaaS). If multiple candidates, check each.
2. `web_search` the vendor's official status page first (e.g. the Microsoft 365 service health page, the vendor's status.* domain), then one or two independent outage-report sources for corroboration. Prefer the vendor's own page as the primary source.
3. Extract: current status, affected service/region, vendor incident ID if published, the vendor's timestamps, and when *you* checked (state both — status pages change fast).
4. Optionally check the internal signal: `search_tickets` for the same symptom across other clients in the last few hours (or point to `cross-client-outage-detector` for the full pass).
5. Post the findings:
   - **Confirmed by the vendor** → internal note (plain text) with source URLs, vendor incident ID, timestamps, and a suggested client reply the tech can review: acknowledges the vendor incident, no ETA promises beyond what the vendor published.
   - **Outage reports but no vendor confirmation** → note it as *unconfirmed*: "independent reports exist; vendor status page shows healthy as of <time>". Recommend continued normal troubleshooting in parallel.
   - **No evidence** → say exactly that, with what was checked and when, so the tech knows the outage theory is cold.

## Guardrails

- **Never assert an outage without a source.** Every claim carries a URL and two timestamps (the source's and yours). "Probably an outage" without evidence is worse than silence.
- Vendor status pages lag real incidents — absence of confirmation is not absence of outage. Say "not confirmed by vendor as of <time>", never "there is no outage".
- Client-facing replies are drafts for tech review in attended mode; do not auto-send. Do not promise restoration times the vendor hasn't published.
- Do not invent incident IDs, status-page URLs, or vendor statements. Quote or paraphrase only what the fetched sources say.
- Notes are plain text (PSA-safe). Include the raw URLs, not markdown links.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the internal note — plain text, no narration, no markdown.
- Post evidence only: sources, timestamps, confirmed/unconfirmed verdict, and (only when vendor-confirmed) a clearly labeled SUGGESTED REPLY block. Never send anything to the client.
- Never change status, priority, or ownership; never merge tickets — that's a human or cross-client-outage-detector decision.
- If web_search fails or returns nothing relevant, post "Vendor outage check: no evidence found for <vendor> as of <time>; checked <sources>." and stop.
