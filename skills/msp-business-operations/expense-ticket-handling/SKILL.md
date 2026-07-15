---
name: Expense Ticket Handling
description: Work a staff expense/reimbursement ticket — look up the expense policy, check receipt completeness, flag policy questions neutrally, and route the approval. Use for mileage, tools, certification fees, meals, and other staff reimbursement requests hitting the internal board.
category: MSP Business Operations
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, update_ticket, add_ticket_note, send_approval]
---

# Expense Ticket Handling

Runs staff expense tickets through a consistent intake: policy checked, receipts complete, questions flagged without judgment, approval routed to the right person with everything they need to decide in one read. The skill validates process, not people — it never approves money and never editorializes about what someone spent.

## When to use

- A ticket arrives: "reimbursement request — <amount> for <thing>, receipt attached."
- "Does our policy cover home-office equipment?" from a tech about to buy something.
- A manager wants expense requests to stop arriving half-complete.
- Pre-purchase checks: "am I OK to expense the exam voucher?"

## Steps

1. Read the request and extract the claim: what was purchased, amount, currency, date, and the business reason if given.
2. Look up the expense policy (search_knowledge_base, search_itglue, search_hudu — "expense policy", "reimbursement", "travel policy"). If no written policy exists, say so explicitly, route to the approver anyway, and suggest the policy gap get closed — do not improvise policy from what "seems reasonable".
3. Receipt discipline — check the ticket for:
   - A receipt attachment present and legible.
   - Amount, date, and vendor on the receipt matching the claim.
   - Itemization where the policy requires it (typically meals and mixed purchases).
   If anything is missing, reply to the requester with a specific, friendly list of what to attach ("please add the itemized receipt — the card slip alone doesn't show the line items") and set the ticket to waiting. One complete round-trip beats three partial ones.
4. Policy comparison, stated neutrally: does the category exist in policy, is the amount inside any stated limit, was pre-approval required for this category and is it referenced? Flag mismatches as facts for the approver — "policy lists a <limit> limit for this category; this claim is <amount>" — never as accusations, never as a verdict.
5. Route the approval (send_approval) to the right approver per policy (usually the requester's manager; above a threshold, whoever the policy names). Attach a one-paragraph plain-text summary: claim, category, policy fit, receipt status.
6. After the decision: record outcome and approver on the ticket, and hand off to whatever pays it (accounting queue, payroll batch) per the tenant's process. Close when the handoff is confirmed, not when the approval lands.

## Guardrails

- **Never approve, deny, or promise reimbursement** — including "this clearly fits policy, you're fine." Policy fit is a fact you report; the approval is a human's.
- Neutral language throughout. An over-limit claim is a policy mismatch to flag, not a judgment about the person; there may be context (manager pre-approved verbally, client-caused expense) you cannot see.
- What staff spend money on can be sensitive (medical-adjacent travel, accommodations). Keep notes to category + amount + policy fit; do not restate purchase details beyond what the approver needs.
- Do not fabricate policy. No written policy → say so; never present a guess or an industry norm as the tenant's rule.
- Internal board only; expense details never touch client-visible tickets.
- Amounts and currency are copied exactly from the receipt — never rounded, converted, or "corrected".

## Unattended (Flows) variant

- Trigger: new ticket on the internal board matching expense/reimbursement intent.
- Do intake only: extract the claim, run the receipt-completeness check, and either (a) post the specific missing-items request and set waiting, or (b) post the plain-text summary note and send the approval to the mapped approver.
- Your entire reply is posted verbatim as the ticket note — plain text, no markdown, no emojis, no verdict language.
- If the amount, category, or approver mapping is ambiguous, post a clarification request and stop. When in doubt, do nothing.
