---
name: Skill Sharing Review
description: Pre-flight a personal skill before it becomes a shared/team skill — sanitization (no credentials or IDs), clarity for someone who isn't the author, and guardrail completeness. Use when asked to "review this skill before I share it", "make this skill team-safe", or "publish my skill to the team".
category: Automation & Flows
tools: [load_skill, list_skills]
---

# Skill Sharing Review

A gate between "works for me" and "installed for the whole desk": scan the skill for embedded secrets and tenant-specific hardcoding, verify a stranger could run it, and check the guardrails cover its blast radius. Verdict plus fix list — sharing stays the member's call.

## When to use

- "I want to share my closure-QA skill with the team — check it first."
- "Is this skill safe to publish?"
- A team lead standardizing personal skills into a shared library.

## Steps

1. Load the skill (`load_skill`) and run the **sanitization sweep** — any hit is a blocker:
   - Credentials of any kind: passwords, API keys, tokens, shared-secret URLs. (Partner corpora have shipped plaintext credentials in skills; assume it can happen here.)
   - Hardcoded person, board, client, or company ids and names — replace with placeholders (<client>, "the VIP board") or with lookups (`search_members`, `list_boards`) resolved at runtime.
   - Internal URLs or document links that other members can't reach.
2. **Clarity check** — read it as someone who isn't the author: is the description a trigger (when-to-use, not a summary)? Are steps imperative and unambiguous? Does the author's personal shorthand ("my board", "the usual template") survive transfer? Anything that only works with the author's private context gets flagged.
3. **Guardrail completeness** — for each write or outbound action the skill performs, verify: a confidence gate, confirm-before-destructive, result-cap honesty on searches, plain-text notes where PSA sync applies, and a when-in-doubt-do-nothing posture where a wrong action beats a missed one. List each missing guardrail with suggested wording.
4. Check `list_skills` for a shared skill that already covers this workflow — recommend merging rather than shipping a near-duplicate.
5. Output the verdict: **Ready to share** or **Fix first**, with a numbered blocker list (sanitization) and a numbered improvement list (clarity/guardrails). Offer the corrected text; never share or modify the skill yourself.

## Guardrails

- Sanitization findings are non-negotiable blockers; clarity findings are recommendations. Keep the two lists separate.
- Never quote a discovered credential back in full — name its location and type only.
- The skills family is in-app SuperAgent only. **Degradation:** when absent, have the member paste the skill text and review that.
- The share action itself always belongs to the member.
