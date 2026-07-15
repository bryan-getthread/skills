---
name: Welcome Packet New Client
description: Draft the new-client welcome packet — how to reach support, what to expect, portal/Messenger setup, and who's who on their account — built only from this client's actual onboarding facts.
category: Communication
tools: [search_clients, search_contacts, search_members, search_knowledge_base, view_openDraft]
---

# Welcome Packet New Client

Drafts the first document a new client keeps: every way to reach the desk, what happens after they do, how to get set up on the portal/Messenger, and the names of the people on their account — so the first real ticket isn't also their first lesson in how support works.

## When to use

- "<client> signed — draft their welcome packet."
- Onboarding a new client and they need the "how to work with us" document.
- Refreshing a stale welcome document for re-sending at onboarding.

## Steps

1. Gather this client's actual setup — the packet is per-client, not boilerplate:
   - Contact channels the desk really operates for this client (email address, phone, portal URL, Messenger) — from the requester or client record (search_clients). Include only channels that exist and are monitored.
   - Service coverage: support hours, what's included under their plan, response expectations appropriate to their agreement — from the requester. Never quote SLA numbers the requester didn't supply.
   - **Who's who:** account manager and primary technician by name and role (search_members), and which registered contacts on the client side can request support or approve changes (search_contacts) — confirm this list with the requester.
   - Any desk-standard onboarding article worth linking (search_knowledge_base) — real articles only.
2. Draft the packet per the Email Baseline Standard skill (communication/email-baseline-standard), sections in this order:
   - **Welcome** — two sentences, warm, zero marketing prose.
   - **How to reach us** — each channel with when to use it, and which channel is right for *urgent* issues. This section must be skimmable in ten seconds.
   - **What to expect** — the life of a request in plain words: you'll get a confirmation, a technician picks it up, you'll hear from us as we work it, you confirm it's resolved. Include the golden habits: one issue per ticket, include screenshots, say when it started.
   - **Portal / Messenger setup** — the steps to get signed in or the widget installed, and who on their side receives the invitations. Real steps for this desk's actual setup, or a link to the real setup article.
   - **Your team** — the named people and roles, plus who to contact about service/billing questions vs technical issues.
   - **After-hours and emergencies** — the path and what qualifies, matched to what their plan actually covers.
3. Keep the whole packet to roughly one page. It's a reference card, not a contract restatement.
4. Present via view_openDraft for the account manager to review, personalize, and send. Recommend it go out before or with the client's portal invitations.

## Guardrails

- Every operational fact must be true for *this* client: channels, hours, coverage, names. A welcome packet with a wrong phone number or a generic "24/7 support" claim their plan doesn't include poisons the relationship at hello.
- No invented links, portal URLs, or KB articles — link only what search_knowledge_base or the requester confirms exists.
- Do not restate contract terms, pricing, or SLA legalese — point to the agreement for that; the packet is operational.
- People named in the packet must be confirmed current (search_members) — a packet introducing a departed technician is worse than no packet.
- Send is human: the account manager reviews and delivers it.
- Degradation: view_openDraft is in-app only — over external MCP, output the packet in chat labeled "WELCOME PACKET DRAFT."
