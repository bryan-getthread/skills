---
name: EOL Product Notice
description: Draft the client notice that a product or OS they run is reaching end-of-life — verified EOL date, honest risk framing, concrete upgrade paths, and a decision deadline — without fear-selling.
category: Communication
tools: [search_tickets, search_itglue, search_hudu, web_search, view_openDraft]
---

# EOL Product Notice

Drafts the message that turns a vendor's EOL calendar into a client decision: what stops on which date, what that actually exposes them to, what their options are, and when the desk needs an answer to act in time.

## When to use

- "<product/OS version> goes end-of-life on <date> — draft the client notice."
- A vendor announced EOL/EOS and affected clients need to be told with options.
- Lifecycle review found devices on soon-unsupported software: "write the letter."

## Steps

1. **Verify the EOL facts before drafting.** Confirm via web_search against the vendor's official lifecycle documentation: the exact date, and what actually ends on it (security updates vs feature updates vs paid extended support). EOL, end-of-support, and end-of-sale are different events — name the right one. If vendor sources conflict or can't be confirmed, say so and ask the requester to confirm rather than publishing a guessed date.
2. Establish this client's exposure: which of their systems run the affected product — from the requester, or from documentation (search_itglue / search_hudu where enabled) and ticket history (search_tickets). Name the affected scope in the letter in client terms ("your 4 office desktops," "the server running <application>") — a generic EOL notice with no "this affects you because" gets ignored.
3. Draft per the Email Baseline Standard skill (communication/email-baseline-standard), with risk language per the Defensive Writing Standard skill (security/defensive-writing-standard):
   - What is reaching end-of-life and the **verified date**, first line.
   - **What changes on that date, factually:** "no further security updates" — and what that means: newly discovered vulnerabilities will remain unpatched, and running unsupported software may affect compliance or cyber-insurance requirements *where the client has such requirements*. No doom scenarios, no "hackers are waiting."
   - **Upgrade paths as options**, each with one line of what it involves: upgrade in place, replace hardware, migrate to <successor product>, or (where the vendor offers it) paid extended support as a bridge. Only include paths that actually exist for this product. Include cost/effort figures only if the requester supplies them.
   - **Decision deadline:** the date by which the client must choose so the work can be scheduled before EOL — worked back from lead time the requester confirms, and clearly distinct from the EOL date itself.
4. Close with the concrete next step: reply with a preferred option, or book a short call to walk through them.
5. Present as an open draft via view_openDraft. If many clients are affected, produce per-client variants (scope differs per client); a mass generic blast is the fallback, not the default.

## Guardrails

- **Never publish an unverified EOL date.** Vendor lifecycle pages are the source of truth; verify against the vendor's current documentation at draft time — cached knowledge of lifecycle dates goes stale.
- Risk framing per defensive-writing: factual consequences, no absolutes ("you will be breached"), no fear-selling. The letter informs a decision; it does not manufacture panic to close an upgrade sale.
- Never state compliance or insurance impact as fact for a specific client unless their requirements are known — frame as "may affect, worth confirming with your <compliance/insurance> contact."
- Quotes, prices, and project timelines come from the requester only. Recommending a path is fine when asked; inventing its cost is not.
- The affected-systems list must trace to documentation or ticket evidence — mark it "based on our records, please confirm" if the inventory may be incomplete.
- Degradation: without IT Glue/Hudu, build scope from the requester and search_tickets and say the inventory basis is partial. view_openDraft is in-app only — over external MCP, output in chat labeled "EOL NOTICE DRAFT."
