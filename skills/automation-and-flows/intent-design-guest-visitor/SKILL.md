---
name: Guest Visitor Intent Design
description: Build the guest wifi/access intent — sponsor capture and expiry defaults baked in, so visitor access is granted deliberately and always expires. Use when asked to "build a guest wifi intent", "automate visitor access requests", or when guest-access asks appear in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets, search_knowledge_base]
---

# Guest Visitor Intent Design

Guide the admin through building a guest/visitor intent that answers the simple case (guest wifi joining info, where policy allows) and turns everything else into a sponsored, expiring access request — every guest has a named sponsor and every grant has an end date.

## When to use

- "Build an intent for guest wifi requests."
- "Visitors get waved onto the network with no record of who or until when."
- Intent Mining flagged guest/visitor access as recurring small-volume, high-hygiene traffic.

## Design

**Trigger phrases (adapt to real ticket language):** "guest wifi", "wifi for a visitor", "client coming in needs internet", "visitor access", "wifi password for a guest", "contractor needs network access", "get a vendor online", "guest network", "temporary wifi access", "visitor needs to connect".
Near-miss watch: staff asking for the corporate wifi is the office-wifi intent; "contractor needs a login/account" is bigger than wifi — treat as an access request (or onboarding-lite) and route accordingly.

**Arguments to collect:**
- Who the visitor is (name/company — placeholder: <visitor>) and how many people.
- **Sponsor:** the employee hosting/vouching for them (defaults to the requester — confirm, don't assume).
- Which site and dates/times on-site.
- What they need: internet-only guest wifi vs. anything touching internal resources (the fork).
- **Expiry:** when access should end — default to the visit's end date; never open-ended.

**Reply flow:**
1. **Internet-only guest wifi, self-service allowed by client policy** → reply with the client's guest-join instructions (portal/QR/front-desk process). Only ever share credentials the client has designated as publicly shareable guest credentials — otherwise point to the process, not the secret.
2. **Guest wifi requiring desk provisioning** (per-visitor voucher/PSK) → create a ticket with visitor, sponsor, site, dates, expiry; route to whoever provisions guest access.
3. **Anything beyond internet-only** (file access, internal systems, a workstation) → this is not guest wifi; create an access-request-grade ticket flagged for review, sponsor and expiry mandatory, elevated-risk note attached.
4. Confirm the summary and expiry date back to the requester in every path.

**Handoff rule:** internal-resource access for non-employees is never granted from chat and never without review. No corporate-network credentials in replies, ever.

**Variation hooks (per client):** guest-join process (open SSID/portal/voucher), whether guest wifi info is freely shareable, default expiry (same-day vs. visit length), contractor policy, sites.

**Success metric:** sponsor-and-expiry coverage — percentage of guest-access grants with a named sponsor and an expiry recorded. Deflection on the self-service-wifi path is a bonus; the hygiene metric is the point.

## Steps

1. `list_intents` — check for an existing guest/visitor intent; prefer updating.
2. `search_tickets` for recent visitor/guest tickets; mine phrasing and see what access visitors actually end up needing (calibrates the internet-only vs. more fork).
3. `search_knowledge_base` for the client's guest-wifi process article; the self-service reply should link it rather than restate.
4. Draft the full spec: triggers, arguments with sponsor and expiry mandatory, three-path flow, variations. Test plan: 5 should-match, 3–5 should-not (staff-wifi and contractor-account near-misses).
5. Show the draft; on explicit confirmation call `create_intent` then the variation tools.
6. Report what was created, restate the test plan, recommend activation after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, output the complete written spec for an admin to apply.
- Sponsor and expiry are non-optional arguments by design; an open-ended, unsponsored guest grant is the failure mode this intent exists to prevent.
- Never place corporate (non-guest) network credentials in a reply; only client-designated shareable guest info may appear, and only if the variation confirms it.
- Non-employee access to internal resources always routes through review — no exceptions for "it's just for an hour".
- Confirm before any write; never activate on the member's behalf. Field block in plain text.
