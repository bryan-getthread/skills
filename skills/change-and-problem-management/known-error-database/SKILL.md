---
name: Known Error Database
description: Maintain the KEDB — every known error in one findable format (symptom / root cause / workaround), deduplicated on arrival, and retired when the fix ships — so techs stop re-diagnosing solved mysteries at 2am.
category: Change & Problem Management
tools: [search_knowledge_base, search_tickets, add_ticket_note, update_ticket]
---

# Known Error Database

The KEDB is the desk's memory of "we know why this happens and here's what to do about it." Its value is entirely in hygiene: one entry per error, a fixed format a stressed tech can scan, and ruthless retirement when fixes ship. A KEDB full of stale entries is worse than none — techs learn to distrust it.

## When to use

- A problem record reached KNOWN ERROR state (problem-record-lifecycle) and needs its KEDB entry written.
- "Is this a known error?" — checking a live incident against the KEDB.
- A fix shipped and the corresponding entry needs retiring.
- A periodic KEDB hygiene sweep: duplicates, stale entries, workarounds past their review date.

## Steps

**Creating an entry:**

1. Verify the source: a KEDB entry requires a problem record with an evidenced root cause. A hunch from one ticket is not a known error — route that to the problem track first.
2. **Dedupe before writing**: search_knowledge_base for existing entries matching the symptom class and the root cause. Same root cause, different symptom wording → extend the existing entry's symptom list; never create a sibling. Similar symptoms but a genuinely different root cause → new entry, cross-referenced to its lookalike ("similar symptoms, different cause: see <entry>") so the lookalike trap is documented.
3. Write the entry in the fixed format, stored in the desk's KB (draft via kb-article-draft in documentation where the desk uses that pipeline; the agent drafts, a human publishes where publishing is gated):
   - **Title**: symptom-first, in the words a searching tech would use ("Outlook prompts repeatedly for credentials after password change"), not cause-first.
   - **Symptoms**: observable signs, error text verbatim where available (error strings are what techs actually search).
   - **Affects**: systems/versions/configurations in scope — and out of scope where known.
   - **Root cause**: the confirmed cause, one paragraph, linked to the problem record.
   - **Workaround**: the documented workaround per workaround-documentation (or a pointer to that document), including its cost and hold time. "No workaround — escalate to <path>" is a valid and important entry.
   - **Status line**: problem record reference, current state (fix in progress / accepted risk), entry review date.
4. Back-link: note on the problem record that the KEDB entry exists, so lifecycle transitions know what to retire.

**Lookup (live incident):**

5. Search the KEDB by symptom and error text before deep diagnosis. On a match, cite the entry and its workaround — with the entry's date and status, so the tech can judge freshness. Do not invent entries or links; no match means say "no KEDB match," not a plausible-sounding near miss (the resolution-research rule: never fabricate references).

**Retirement:**

6. When the problem closes FIXED (verified per problem-record-lifecycle), retire the entry: mark it "RESOLVED — fix deployed <date>, entry retained for history" rather than deleting, so old ticket links still resolve, but make its non-current status unmissable at the top. When the problem closes ACCEPTED RISK, mark the entry permanent with its scheduled review date.
7. Hygiene sweep: list entries past their review date, entries whose linked problem record has moved states without the entry updating, and near-duplicate pairs — with a recommended action for each. Retirement recommendations go to a human; the agent doesn't bulk-delete knowledge.

## Guardrails

- No entry without an evidenced root cause and a problem record behind it — the KEDB is for known errors, not open mysteries.
- Dedupe is mandatory on every create; two entries for one error means half the techs get the stale one.
- Never fabricate entry references, ticket numbers, or links during lookup; a false "known error" match sends a tech confidently down the wrong path.
- Retired means visibly retired — a fixed error's workaround must not remain findable-as-current, or techs will keep applying it.
- Entries are sanitized: symptom/cause/workaround in general terms; no client names, credentials, or environment-specific identifiers beyond what the desk's KB conventions allow.
