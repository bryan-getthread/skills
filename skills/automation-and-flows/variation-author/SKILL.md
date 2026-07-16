---
name: Variation Author
description: Interview the admin about how troubleshooting differs per client (printers, VPN, LOB apps…), then encode those differences as intent variations — arguments and replies per variation, with a test plan before activation. Answers the commonly requested "the bot should answer differently for each client" workflow.
category: Automation & Flows
tools: [create_intent, update_intent, get_intent, list_intents, update_variation, set_variation_arguments, set_variation_replies]
---

# Variation Author

Turn "well, for <client A> the printers work like X but for <client B> it's Y" into properly structured intent variations. This is the variation-specialized sibling of `automation-and-flows/intent-builder`: intent-builder builds the whole intent (trigger phrases, base replies, everything); this skill assumes the intent concept exists and focuses on the interview → variation → test loop for client- and context-specific differences.

## When to use

- An admin says client-specific answers are needed: "our VPN steps differ per client", "each client has a different printer setup", "the intent gives the wrong instructions for <client>".
- After `intent-mining` or `intent-builder` ships a base intent and real traffic shows per-client divergence.
- An admin wants existing variations reviewed, cleaned up, or extended.

## Steps

1. Locate or create the base intent: `list_intents` / `get_intent` to find it; if the concept doesn't exist at all, build the base with `create_intent` (or hand off to intent-builder for a full build) before authoring variations.
2. **Interview the admin** — short, concrete questions, one dimension at a time:
   - Which situations need different answers? (per client, per site, per device model, per app version)
   - For each: what exactly differs — the steps, the contact to call, the tool name, the URL?
   - What should happen when the differentiating fact is unknown mid-conversation? (ask the user, or fall back to the base reply)
   - Capture answers verbatim where they'll become reply text; don't paraphrase technical steps.
3. Draft the variation set and show it as a table before touching anything: variation → discriminating condition → arguments it needs → reply text. Get an explicit "yes" on the table.
4. Encode: `update_variation` for each variation's condition, `set_variation_arguments` for the facts the intent must collect (e.g. <client>, <device model>), `set_variation_replies` for the reply text. Keep the base/default variation as the safe fallback — never leave the unknown case answerless or, worse, answered with one client's specifics.
5. **Test plan before activation:** for each variation, write 2–3 sample end-user utterances plus the expected variation match and reply; include at least one deliberately ambiguous utterance that must fall back to the default. Walk the admin through the plan and have them run it (or run it together) before considering the work done. Update via `update_intent` only after the plan passes.
6. Output: the final variation table, the test plan with results, and any follow-ups (variations the interview surfaced but the admin deferred).

## Guardrails

- **Attended and admin-only.** Intent/variation tools are admin-scoped; if they are absent from the tool surface (non-admin token, external MCP without admin rights), do not attempt writes — deliver the interview results and variation table as a spec the admin can apply, and say that's the degradation.
- Nothing goes live without the reviewed table (step 3) and a test plan (step 5). An untested variation ships wrong instructions to a client's end users with a straight face.
- Client-specific reply text must come from the admin's own words or documentation — never invent client procedures, URLs, or contact instructions to fill a variation.
- Every variation set keeps a working default: the reply for "we don't know which case this is" must be safe for *all* clients.
- Replies are end-user-facing: plain language, no internal jargon, localizable (no idioms). Flag replies that embed environment-specific identifiers.
- One intent per concept — if the interview reveals two genuinely different intents (VPN setup vs VPN broken), split them rather than overloading variations.
