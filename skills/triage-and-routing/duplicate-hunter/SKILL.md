---
name: Duplicate Hunter
description: Check whether a ticket duplicates an existing open ticket for the same client, contact or asset, and symptom — and merge only on an exact reference match.
category: Triage & Routing
tools: [search_tickets, merge_ticket, add_ticket_note]
---

# Duplicate Hunter

Hunt for a true duplicate of a ticket — same client, same contact or asset, same symptom, inside a recent window — and merge it under the original only when a strong identifier proves the match. A confident "no duplicate found" is a valid, useful answer.

## When to use

- A new ticket looks like something already open ("didn't we just get this?").
- A flow runs on ticket creation to catch double-submissions and re-sent alerts.
- Dispatch wants a duplicate sweep of today's intake.

## Steps

1. Read the candidate ticket with `search_tickets`: client, contact, referenced asset/device, and the specific symptom or error.
2. Search for open tickets from the same client within the configured window (default: 7 days). Run separate `search_tickets` queries per strong identifier — contact email, device/asset name, alert or error reference — rather than one broad text search. If any query may have hit a result cap, state that.
3. For each candidate, require an exact-reference match before calling it a duplicate: same alert/monitoring ID, same device + same error code, or same contact + verbatim-same request. Similar wording alone is never a duplicate.
4. If exactly one older open ticket matches at exact-reference strength: merge the newer ticket into it with `merge_ticket`, and post a plain-text note on the surviving ticket naming both numbers and the matching identifier.
5. If matches are plausible but not exact, report them as "related, not merged" with reasons.
6. If nothing matches, state plainly that no duplicate was found and which identifiers were checked — that is a successful outcome, not a failure.

## Guardrails

- Never merge on wording similarity — require an exact identifier match (the strongest guardrail partners themselves wrote).
- Merge direction is fixed: newer merges into older; never merge into a closed ticket.
- Two or more candidate parents → do not merge; list them.
- Do not merge tickets from different clients or different contacts even if the symptom text is identical — that is a pattern for the cross-client outage detector, not a duplicate.
- Report result-cap honestly; an incomplete search cannot support a confident "no duplicate".
- Notes are plain text.

## Unattended (Flows) variant

- Your entire reply is the internal note, verbatim. One of exactly three forms: `DUPLICATE: merged #<new> into #<old>. Match: <identifier>.` / `POSSIBLE DUPLICATE of #<n>: <reason>. Not merged.` / `NO DUPLICATE FOUND. Checked: <identifiers>.`
- Merge only on exact-reference match with a single candidate parent; otherwise no write. When in doubt, do nothing.
- Never merge unattended if either ticket has a human reply newer than the alert content.
