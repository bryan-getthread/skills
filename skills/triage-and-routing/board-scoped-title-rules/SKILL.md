---
name: Board-Scoped Title Rules
description: Apply each board's own title convention on ticket create — append site or company-id, strip redundant prefixes, enforce house format — using the per-board rule set. Generalizes alert-title normalization to every board. Answers the commonly requested "make every board's titles follow our own convention" partner workflow.
category: Triage & Routing
tools: [search_tickets, list_boards, update_ticket, add_ticket_note]
---

# Board-Scoped Title Rules

Different boards want different title shapes: the VIP board wants the company id appended, the alert board wants tool boilerplate stripped, the projects board wants a phase prefix. This skill reads the board's own documented title convention and rewrites the title on create to match — a generalization of alert-title normalization across all boards.

## When to use

- Each board has its own title convention and the desk wants them enforced automatically.
- "On the <board>, every title should end with the site name."
- Cleaning up inconsistent titles so a board's queue reads uniformly.

## Steps

1. Read the ticket with `search_tickets`: current title, board, client, site, and any identifiers the convention needs.
2. Look up this board's title convention in the desk's **per-board rule set** configured alongside the skill (`list_boards` to confirm the board). Rules are declarative, e.g. "append ` — <site>`", "strip leading `FW:`/`RE:`/tool prefixes", "prepend `[<company-id>]`". If the board has no rule, do nothing.
3. Compose the new title by applying the board's rules to the current title. Pull appended values (site, company id) from the ticket's real fields, never from guesswork. Keep it under ~90 characters; move any unique info that won't fit into a note rather than dropping it.
4. If the rewritten title differs from the current one, apply it with `update_ticket` and post a plain-text note via `add_ticket_note`: old title, new title, which rule applied. If unchanged, write nothing.

## Guardrails

- **Board-scoped and declarative.** Apply only the documented rule for the ticket's board; never improvise a convention or apply one board's rule to another.
- Never fabricate appended values — if the site or company id isn't on the ticket, skip that part of the rule and note why rather than inventing a placeholder.
- Never drop information that lives only in the title (unique error codes, hostnames) — move it to the note if it doesn't fit.
- Title + one note are the only writes. No status/priority/board/owner changes.
- Notes plain text (PSA compatibility).

## Cross-references

- Alert-specific version: `skills/triage-and-routing/alert-title-normalization`
- Preserving the original wording: `skills/documentation/title-audit-trail`

## Unattended (Flows) variant

- Fires on **ticket create** — a supported event; the flow can scope to the board via its board condition so each board's flow applies its own rule.
- Your entire reply is the note, verbatim: `TITLE FORMATTED. Old: <old>. New: <new>. Rule: <rule>.` or `TITLE UNCHANGED.`
- One rewrite per ticket: if the title already matches the board convention, stop without writing.
- Deterministic stops: board has no rule → unchanged; required appended value missing → apply what's available and note the gap; ambiguous rule → unchanged.
