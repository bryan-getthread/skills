---
name: Linear Spec Lookup
description: Answer "is this behavior intended?" by pulling the PRD/spec from Linear documents before escalating a suspected bug — cite the spec or confirm there isn't one. Use for "is this a bug or by design", "check the spec before I escalate", or "what does the PRD say about <feature>".
category: Connectors
tools: [list_documents, get_document, list_issues, get_issue, list_comments, search_tickets, add_ticket_note]
---

# Linear Spec Lookup

The cheapest escalation is the one you don't file: before a suspected bug goes to engineering, check whether the product documentation says the behavior is intentional — and either answer the ticket with a citation or escalate with "spec checked, not covered" attached.

## When to use

- "Client says <feature> behaves weirdly — is that intended?"
- Pre-escalation check inside an escalation-prep workflow.
- "Find the PRD for <feature>."

## Steps

1. Pin down the behavior in question from the ticket (`search_tickets`): exact action, expected vs actual, product area.
2. Search Linear documents: `list_documents` filtered/searched by the feature's terms; `get_document` on candidates. Check both PRDs and any design/decision docs.
3. Also check the issue record: `list_issues` for existing issues describing the same behavior — an open issue means it's a known bug (cite it); a closed "works as designed" issue is itself an answer (read its `list_comments` for the rationale).
4. Deliver one of three verdicts, always labeled:
   - **Intended** — quote the specific spec passage and link the document. Draft the client-facing explanation from the spec's substance in plain language.
   - **Known issue** — cite the existing Linear issue and its state; offer to attach this ticket's impact to it (see Linear Client Impact Queue).
   - **Not covered** — say clearly that no spec addresses it and no existing issue matches; proceed to the escalation path with "documentation checked: <docs searched>" included so engineering doesn't repeat the search.
5. Offer to `add_ticket_note` (internal, plain text) recording the verdict and citation on the ticket.

## Guardrails

- **Member-authenticated connector:** requires the member's Linear connection; degrade by telling the member what to search for in Linear and proceeding on their paste-back.
- Never claim "intended behavior" without a citable document or an explicit works-as-designed issue — absence of a spec is verdict three, not verdict one. This is the skill's whole reason to exist.
- Quote specs accurately; don't paraphrase a spec into saying more than it says. If the spec is ambiguous about the exact case, say "spec is ambiguous" and escalate with the quote attached.
- Internal spec content may not be client-shareable verbatim; the client-facing explanation is a plain-language translation, not a document dump.
- Search coverage honesty: list which documents were searched so "not covered" is a bounded claim.
