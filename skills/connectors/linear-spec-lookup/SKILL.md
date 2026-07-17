---
name: Linear Spec Lookup
description: Answer "is this behavior intended?" by pulling the PRD/spec from Linear documents before escalating a suspected bug — cite the spec or confirm there isn't one. Use for "is this a bug or by design", "check the spec before I escalate", or "what does the PRD say about <feature>".
category: Connectors
tools: [search_tickets, add_ticket_note]
connectors: [Linear]
---

# Linear Spec Lookup

**When to use:** "Client says <feature> behaves weirdly — is that intended?" or "find the PRD for <feature> before I escalate."

## Prompt

```
The cheapest escalation is the one you don't file. Before a suspected bug goes to engineering,
check whether Linear's product documentation says the behavior is intentional — and either
answer the ticket with a citation or escalate with "spec checked, not covered" attached.

This needs the member's connected Linear (list_documents, get_document, list_issues, get_issue,
list_comments), scoped to their workspace access. If Linear isn't connected, degrade by telling
the member what to search for and proceeding on their paste-back — do not stop.

1. Pin down the behavior from the ticket (search_tickets): exact action, expected vs actual,
   product area.
2. Search Linear documents: list_documents filtered/searched by the feature's terms; get_document
   on candidates. Check both PRDs and any design/decision docs.
3. Also check the issue record: list_issues for existing issues describing the same behavior — an
   open issue means it's a known bug (cite it); a closed "works as designed" issue is itself an
   answer (read its list_comments for the rationale).
4. Deliver one of three verdicts, always labeled:
   - Intended — quote the specific spec passage and link the document; draft the client-facing
     explanation from the spec's substance in plain language. Never claim "intended" without a
     citable document or an explicit works-as-designed issue — absence of a spec is verdict three.
   - Known issue — cite the existing Linear issue and its state; offer to attach this ticket's
     impact to it.
   - Not covered — say clearly no spec addresses it and no existing issue matches; proceed to the
     escalation path with "documentation checked: <docs searched>" so engineering doesn't repeat it.
5. Quote specs accurately; don't paraphrase a spec into saying more than it says. If ambiguous
   about the exact case, say "spec is ambiguous" and escalate with the quote attached. Internal
   spec content isn't client-shareable verbatim — the client-facing text is a plain-language
   translation, not a document dump.
6. Offer to add_ticket_note (internal, plain text) recording the verdict and citation.
```
