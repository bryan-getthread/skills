---
name: KB Article Draft
description: Turn a resolved ticket into a reusable knowledge-base article draft — title, symptoms, cause, numbered resolution, and a "when this applies" section — with all client specifics stripped.
category: Documentation
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu]
---

# KB Article Draft

Convert the fix that actually worked on a ticket into a clean, reusable KB article draft.
The output is always a **draft for human review — never publish or file it automatically**.

## When to use

- "Turn this ticket into a KB article" after a resolution worth reusing.
- "Write this up so the rest of the team can fix it next time."
- A recurring issue just got solved properly for the first time and deserves documentation.
- A senior tech resolved something only they knew how to fix.

## Steps

1. Read the full ticket thread with `search_tickets`: notes, time entries, and the final resolution. Identify the fix that **actually resolved** the issue — not the first thing tried.
2. Check `search_knowledge_base` (and `search_itglue` / `search_hudu` where enabled) for an existing article on the same issue. If one exists, propose an update to it instead of a duplicate.
3. Draft the article in this structure:
   - **Title** — symptom-first, searchable (what a tech would type when hitting this).
   - **When this applies** — the environment, product versions, and symptom pattern that make this the right article; and when it is NOT the right fix.
   - **Symptoms** — what the user reports and what the tech observes.
   - **Cause** — root cause in one or two sentences.
   - **Resolution** — numbered, imperative steps, each independently verifiable.
   - **Prerequisites / access needed** — roles, admin portals, tools.
4. Sanitize for reuse: replace client names, usernames, hostnames, IPs, tenant IDs, and serial numbers with placeholders (`<client>`, `<user>`, `<device>`, `<server>`). Remove any credentials entirely — do not placeholder them, delete them.
5. Output the draft in chat, ready to paste into the KB, and note the source ticket number for the reviewer. Do not create or update any KB entry yourself.

## Guardrails

- **Draft only.** Never auto-publish, auto-file, or write the article into any documentation platform. A human reviews and publishes.
- Never include credentials, MFA secrets, license keys, or client-identifying detail in the draft — even redacted-looking fragments.
- Base steps only on what the thread shows resolved the issue. Do not pad with generic troubleshooting the ticket does not support, and do not invent links or article references.
- If the resolution is ambiguous or the ticket closed without a confirmed fix, say so and stop — do not write an article around a guess.
- If `search_itglue` / `search_hudu` are not enabled for this tenant, check only the Thread KB and note the narrower duplicate check.

## Consolidates

Draft-KB-article-from-ticket and SOP-creation-helper skills. Finding tickets worth documenting is now its own skill: **sop-candidate-finder**. Full procedural SOPs: **sop-builder**.
