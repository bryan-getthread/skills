---
name: DMARC / SPF / DKIM Setup
description: Diagnose email authentication failures and produce correct SPF, DKIM, and DMARC record guidance — including new sending sources, alignment problems, and honest propagation expectations.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# DMARC / SPF / DKIM Setup

Turns "our email is failing authentication" or "set up DMARC" into verified current-state records, a specific diagnosis (which mechanism, which source, which alignment), and exact corrected records — with honest caveats about propagation and phased DMARC enforcement.

## When to use

- Recipients reject or junk the client's mail with SPF/DKIM/DMARC failure wording
- "Set up / fix DMARC (or SPF, DKIM) for <client domain>"
- A new sending service (marketing tool, CRM, scanner, app) needs to send as the domain
- DMARC aggregate reports show failing sources

## Steps

1. **History first.** search_tickets for prior authentication work on this domain — a half-finished migration or a previous record change explains most sudden failures.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the domain's DNS host, who is authorized to edit DNS, and the inventory of legitimate sending sources.
3. **Get the evidence before theorizing.** The failing message's full headers (Authentication-Results header) or a DMARC report excerpt. Identify: which mechanism failed (SPF, DKIM, or DMARC alignment), for which source IP/service, and what the receiver did (none/quarantine/reject).
4. **Read the current records.** Guide the tech to query the live TXT records (root SPF, `_dmarc`, and the relevant DKIM selectors). Diagnose against what's actually published, never from documentation alone.
5. **Branch:**
   1. **SPF** — failing source missing from the record, or the record is broken. Check: does the record include every legitimate source (from step 2's inventory)? Does it exceed the 10-DNS-lookup limit (a silently fatal, common defect)? Multiple SPF records (invalid)? Produce the exact corrected single record, using includes per the sending vendors' documented values (verify with web_search, never from memory). Prefer `~all`; recommend `-all` only when the source inventory is confirmed complete.
   2. **DKIM** — missing or broken signature. Identify which service should be signing (M365, gateway, marketing tool) and whether its selector's public key is published. Fix is two-sided: enable signing at the source and publish the selector record the vendor specifies. Escalate when: the sending service doesn't support DKIM — say so; the client accepts SPF-only alignment for that source or changes vendor.
   3. **DMARC alignment** — SPF and DKIM may each pass, yet DMARC fails because neither passes *for the visible From domain*. Classic with forwarders and third-party senders using their own envelope domain. Fix: custom return-path / DKIM signing as the client domain at that vendor. This is the branch most often misdiagnosed — check alignment explicitly.
   4. **New DMARC rollout** — publish `p=none` with `rua` reporting first. Monitor reports for a full business cycle, fix failing legitimate sources, then step p=none → quarantine (optionally with pct=) → reject. Refuse to jump straight to `p=reject` on a domain with unverified sources — state the mail-loss risk plainly.
6. **Deliver the guidance.** Exact record name, type, and value for each change, plus where it gets entered (the documented DNS host). State the current TTL and honest propagation: changes take up to the TTL (and practically up to 24–48h at stragglers) — one change, then wait and re-verify; do not thrash the record.
7. **Verify and note.** After propagation, re-check the live records and a fresh Authentication-Results header from a real message. Plain-text note: diagnosis, records changed (before/after), verification result, and any enforcement-phase plan.

## Guardrails

- DNS edits are guidance for whoever owns DNS access — presented as exact records, never executed by the agent.
- Never recommend `p=reject` (or `-all`) as a first move on an unaudited domain; phased enforcement only, and say why.
- Vendor include/selector values must be verified against the vendor's current documentation via web_search — do not recite from memory.
- Be honest about propagation: no "it should work now" immediately after a DNS edit. Give the TTL-based window and schedule the re-check.
- Only the receiving side controls its filtering; if mail still junks after authentication passes, say the remaining lever is sender reputation and the receiver — not more record edits.
- Docs tools vary per tenant — state what source inventory you could not confirm; an incomplete inventory blocks strict enforcement recommendations.
