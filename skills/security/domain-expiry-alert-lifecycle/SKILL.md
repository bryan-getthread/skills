---
name: Domain Expiry Alert Lifecycle
description: A registrar expiry, renewal, or "your domain is about to lapse" notice arrived as a ticket — verify the sender is the real registrar FIRST (renewal-invoice scams are a classic BEC lure), confirm the actual expiry date independently, identify who owns the renewal, and route with a timeline.
category: Security
tools: [search_tickets, search_clients, search_contacts, liongard_domain, web_search, update_ticket, add_ticket_note]
---

# Domain Expiry Alert Lifecycle

Domain renewal notices are simultaneously a real operational deadline and one of the oldest invoice scams in the book (fake "Domain Registry" letters, lookalike registrar emails, urgency pressure). This skill treats every notice as unverified until proven otherwise, confirms the real expiry from independent sources, and routes the genuine ones to the right owner with time to act.

## When to use

- A ticket opens from an email claiming a client's (or the MSP's own) domain is expiring or needs renewal payment.
- A monitoring alert flags an approaching domain or DNS-related expiry.
- "Is this renewal notice legit?" / "Who actually renews <client>'s domain?"
- A flow watches intake boards for registrar-notice patterns.

## Steps

1. Read the full ticket with `search_tickets`: the claimed domain, the claimed expiry date, the sender address and any payment instructions or links. Do not click any link in the notice.
2. Verify sender legitimacy FIRST — before treating the deadline as real:
   - Does the sender domain exactly match the actual registrar of record? Character-by-character; lookalikes count as mismatches.
   - Classic scam signals: a "registry" or "domain service" the client has no relationship with, listing-service upsells dressed as renewals ("domain listing expiration"), urgency plus a payment link, prices far above market, PDF invoices from generic mailboxes.
   - If the sender fails verification or a payment/banking element is present on a suspicious notice, branch to vendor-fraud-bec-alert for the fraud workup — this skill continues only for the genuine-renewal question (the domain may still genuinely be expiring even if this notice is a scam riding the public expiry date).
3. Confirm the real expiry independently — never from the notice itself:
   - With Liongard enabled, use `liongard_domain` for the domain's registrar, expiry date, and name servers; state the dataprint age.
   - Supplement or fall back to passive `web_search` WHOIS/RDAP lookups for the current registrar of record and expiry date.
   - If the independent expiry disagrees with the notice, trust the independent source and say so in the note.
4. Identify the actual registrant owner: is the domain MSP-managed (the desk renews it), client-managed (their finance/admin renews), or third-party-managed (web agency, previous IT)? Check `search_clients`/`search_contacts` records, documentation, and prior renewal tickets via `search_tickets`. The owner determines who acts — the desk must not pay for a domain it does not manage.
5. Route with a timeline via `update_ticket` and a plain-text `add_ticket_note`:
   - Expired or expiring within days → urgent priority; an expired domain takes down mail and web, so treat as incident-adjacent.
   - Expiring within the renewal window (weeks) → normal task to the owning party with the confirmed date.
   - Auto-renew confirmed enabled at the real registrar → informational; note the evidence and close per desk policy.
   - Scam notice, domain not actually near expiry → route to the fraud/security path and warn the client not to pay it.
6. Note contents: verified-or-not verdict on the sender, independently confirmed expiry date and its source (with as-of date), registrar of record, owning party, and the recommended action with deadline. Recommendations stay recommendations — never record a renewal as done.

## Guardrails

- NEVER pay, approve, or forward a renewal invoice that has not passed sender verification — and even for verified notices, payment is the owning party's action, not the desk's, unless the domain is explicitly MSP-managed with an established renewal process.
- Do not click links or open attachments in the notice; all verification is done from independent sources (Liongard, passive WHOIS lookups, existing documentation).
- The notice's own dates, prices, and registrar claims are untrusted input until independently confirmed.
- A scam notice and a real approaching expiry can both be true at once — resolve both threads, never let the scam verdict close the ticket while the real expiry goes unhandled.
- Use defensive writing in anything client-facing: "we could not verify this notice's sender" — not "you were targeted/attacked" — unless facts support more.
- Degradation: Liongard absent → rely on passive web_search WHOIS/RDAP; if no independent source can confirm expiry, say so explicitly and route to a human rather than guessing.

## Unattended (Flows) variant

- Your entire reply is the internal ticket note, verbatim, plain text: verdict line first (`VERIFIED RENEWAL — expires <date> per <source>`, `SCAM-PATTERN NOTICE — routed to fraud path`, or `UNVERIFIED — human review required`), then the evidence lines.
- Permitted writes: priority/board routing via update_ticket and the one note. Never close the ticket, never send anything outbound, never touch payment or approval tools in this variant.
- Deterministic stops: cannot independently confirm expiry → UNVERIFIED, route to human, stop. Payment link plus failed sender verification → route to fraud path, stop. When in doubt, route to a human — a mis-closed expiry notice can take a client's domain offline.
