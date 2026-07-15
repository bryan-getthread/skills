---
name: Unattended Output Discipline
description: BASE skill — the output contract for any prompt or skill that runs unattended inside a flow, where the agent's entire reply is posted verbatim as the artifact. Load whenever writing or reviewing a flow-embedded agent prompt, or when a skill has an "Unattended (Flows) variant".
category: Automation & Flows
tools: []
---

# Unattended Output Discipline

When Super Magic runs inside a flow, nobody is reading the conversation — the reply IS the product. It lands verbatim as a ticket note, a status change rationale, or an API response. This base skill defines the contract every unattended prompt must obey. Other skills reference it instead of restating it.

## When to use

- Writing or reviewing any agent prompt that a flow will execute automatically.
- Adding an "Unattended (Flows) variant" section to another skill.
- Debugging a flow whose notes are full of "Sure! Here's the summary you asked for…".

## The contract

1. **The entire reply is the artifact.** No greeting, no narration ("Let me check the ticket…"), no closing offer of further help, no markdown headers unless the destination renders them. If the artifact is a ticket note bound for a PSA, it is plain text: no markdown, no emojis.
2. **Empty output when unsure.** If the confidence bar isn't met — ambiguous company match, low-signal classification, missing data — output nothing at all (or the designated no-op token the flow expects). A wrong artifact posted to a ticket is worse than a skipped run. WHEN IN DOUBT, DO NOTHING.
3. **Deterministic stop conditions.** The prompt must enumerate exactly when the agent acts and when it stops: which statuses it touches, how many items it processes, what it does on zero matches. "Use your judgment" is not a stop condition in an unattended run.
4. **No fabrication, ever.** No invented ticket numbers, links, names, or documentation. If a value can't be verified from tool output, it does not appear in the artifact.
5. **One artifact per run.** The flow expects one thing (one note, one JSON object, one status decision). Never emit two candidate answers or a hedged both-ways reply.
6. **State assumptions inside the artifact only if the destination is human-read** (e.g., "count is at least 50 — search capped"). Machine-read destinations get schema-valid output only — see the JSON API Response Pattern base skill.

## The VERBOSE test-flag pattern

Debugging an unattended prompt without breaking production discipline: build the prompt so that IF the triggering ticket (or invocation) contains an agreed test marker — e.g., the literal token VERBOSE in the subject or note — the agent appends its reasoning, tool results, and decision trace after the artifact. Without the marker, never explain, never narrate. This gives admins a switchable trace without a second flow.

## Guardrails

- Apply this contract to the *embedded prompt text* when building flows (Flow Builder step for agent actions) and when authoring skills that declare an unattended variant.
- Test unattended prompts on a low-traffic board or test ticket with the VERBOSE marker before wiring them to production triggers.
- Re-check the destination's formatting reality: PSA-synced notes are plain text; Thread-only notes may render markdown; API destinations need the JSON pattern.
