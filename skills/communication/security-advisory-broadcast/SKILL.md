---
name: Security Advisory Broadcast
description: Draft a security advisory going to many clients at once — new threat, vendor breach, critical vulnerability — built from verified facts only, with a per-client relevance check so no one gets an advisory about a product they don't run.
category: Communication
tools: [search_clients, search_tickets, search_itglue, search_hudu, web_search, view_openDraft]
---

# Security Advisory Broadcast

Drafts the one-to-many security notice that has to be right the first time: verified facts, defensive wording, a clear "what we're doing / what you should do" split — and a relevance pass so the broadcast list contains only clients the advisory actually applies to.

## When to use

- "Vendor <X> disclosed a breach — draft the advisory for our affected clients."
- "There's a critical vulnerability in <product> being exploited — notify clients who run it."
- A high-profile threat is in the news and clients need the desk's position before they start calling.

## Steps

1. **Verify before writing.** Establish the facts via web_search from primary sources — the vendor's own advisory/disclosure, or an official body (e.g. the national CERT/CISA-equivalent). Capture: what is affected (product, versions), what is confirmed vs under investigation, and the vendor's recommended action. Media coverage is a pointer to primary sources, never the source itself. Apply the Defensive Writing Standard skill (security/defensive-writing-standard) to every word — this message will be forwarded, quoted, and possibly shown to insurers.
2. **Per-client relevance check — mandatory before any send list exists.** For each candidate client, verify they actually use the affected product/vendor: documentation (search_itglue / search_hudu where enabled), ticket history (search_tickets), or the requester's knowledge. Output two lists: **confirmed relevant** (evidence found — cite it) and **unverified** (no evidence either way — human decides). Never default the unverified list into the broadcast.
3. Draft per the Email Baseline Standard skill (communication/email-baseline-standard), in this order:
   - What happened, in one or two verified sentences, naming the vendor/product and citing the vendor's own disclosure.
   - **Whether this client is affected and how** — written so each recipient reads "this concerns the <product> you use," which the relevance check just made true for everyone on the list.
   - **What we are doing:** the desk's active response (assessing exposure, applying vendor patches as released, monitoring). Only actions actually underway.
   - **What you should do:** concrete client-side actions if any (reset passwords, expect a maintenance window, report suspicious emails) — or explicitly "no action needed from you right now."
   - **What we don't know yet** and when the next update comes, if the situation is evolving.
4. Keep it to one screen. Anxious readers stop at the first unclear sentence.
5. Present the draft plus both recipient lists via view_openDraft. The human reviews the facts, approves the confirmed list, decides the unverified list, and owns the send — a mass security broadcast is never sent autonomously.

## Guardrails

- **Only verified facts.** Every claim traces to the vendor's advisory or an official source at draft time. No speculation about attacker identity, scope, or motive; no repeating unconfirmed media numbers. "The vendor has not yet confirmed" is a complete sentence.
- Defensive-writing rules govern the vocabulary: no "breach"/"hacked"/"compromised" beyond what the primary source itself states, no absolute safety assurances ("your data is safe") without evidence.
- **No advisory to a client who doesn't run the product.** Irrelevant security mail trains clients to ignore relevant security mail. The relevance check is not skippable for speed — if there's no time to verify, the human sends to their own list and owns it.
- Never reveal which other clients are affected, the count of affected clients, or anything enabling cross-client inference.
- If the desk itself may be inside the blast radius (an MSP-tooling supply-chain event), stop — that is a declared-incident scenario where management and the incident policy govern communication, not this skill.
- Degradation: without IT Glue/Hudu, the relevance check runs on tickets + requester knowledge only — say so, and expect a larger "unverified" list. view_openDraft is in-app only — over external MCP, output in chat labeled "SECURITY ADVISORY DRAFT" with both recipient lists.
