---
name: Guest WiFi Intent Design
description: Build the guest-wifi intent — deflect to standing guest credentials where they exist, and otherwise capture the sponsor and expiry so a scoped guest account can be issued. Use when asked to "build a guest wifi intent", "handle visitor network requests", or when guest-access shows up in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets, search_knowledge_base]
---

# Guest WiFi Intent Design

Guide the admin through building an intent for guest / visitor wireless access. Where the client runs a standing guest SSID with a self-service captive portal, the intent deflects with the current details. Where guest access is issued per-visit, it captures the sponsor and the expiry so a time-boxed account can be created. The intent never issues credentials itself.

## When to use

- "Build an intent for guest wifi requests."
- "Reception keeps asking IT for the visitor wifi password."
- Intent Mining flagged guest/visitor network access.

## Design

**Trigger phrases (adapt to real ticket language):** "guest wifi password", "visitor wifi access", "wifi for a visitor", "internet for a guest", "temporary wifi access", "conference wifi for attendees", "client coming in, need wifi", "guest network login".
Near-miss watch: "I can't connect to the office wifi" (an employee on the corporate SSID) belongs to the office-wifi/troubleshooting intent — keep triggers scoped to *guest/visitor* access.

**Arguments to collect:**
- Is standing guest wifi available at this site (drives deflect vs. issue)?
- Number of guests and the visit dates/duration (drives expiry).
- Sponsor — the internal employee vouching for the guest(s) (authorization anchor — required when an account is issued).
- Any elevated need beyond basic internet (rare; usually a red flag to route to a human).

**Reply flow:**
1. If the site has a standing guest SSID with a captive portal or shared code → reply with the SSID and how to connect (or the self-service portal), no ticket. This is a deflection.
2. If guest access is issued per-visit → capture sponsor + guest count + dates, create a ticket to issue a time-boxed guest account, and route to the network workflow; tell the requester the account will be scoped to the visit and expire after.
3. If someone asks for anything beyond guest internet (access to internal resources) → route to a human; do not treat as a standard guest request.

**Handoff rule (non-negotiable):** the intent never creates guest credentials or shares them insecurely — issuing access is a network-admin/workflow action, always tied to a named sponsor and an expiry. Never post a guest password into an untrusted channel.

**Variation hooks (per client):** whether a standing guest SSID exists per site, the captive-portal vs. shared-code model, default expiry length, the sponsor-approval requirement, the network routing target.

**Success metric:** for standing-SSID sites, deflection rate (guest wifi questions answered without a ticket); for per-visit sites, first-touch completeness (sponsor + count + dates + expiry captured). Report both where the client mixes models.

## Steps

1. `list_intents` — check for an existing wifi/guest intent; prefer `update_intent` and avoid overlap with the office-wifi intent.
2. `search_tickets` for past guest-wifi contacts; determine per site whether access is standing or per-visit — that split defines the branch logic.
3. `search_knowledge_base` for the client's guest-wifi details; reference the SSID/portal in the deflection reply rather than hardcoding a password that will rotate.
4. Draft the full spec: triggers, arguments, branch replies, routing target, variations, and a test plan (5 should-match, 3–5 should-not including an employee-wifi near-miss). Show before any write.
5. On explicit confirmation: `create_intent`, then `set_variation_arguments` / `set_variation_replies` / `update_variation`.
6. Report what was created, restate the test plan, recommend activation only after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, degrade to a complete written spec for an admin to apply.
- The intent deflects (standing SSID) or captures + routes (per-visit) — it never issues credentials, and every issued account requires a named sponsor and an expiry.
- Do not hardcode a rotating guest password into the reply; reference the portal or current-details source so it can't go stale.
- Route anything beyond basic guest internet to a human — guest access must not reach internal resources.
- Confirm before any write; never activate on the member's behalf. Ticket field block in plain text (PSA-safe, no markdown).
