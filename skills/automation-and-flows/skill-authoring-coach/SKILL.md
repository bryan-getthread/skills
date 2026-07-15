---
name: Skill Authoring Coach
description: Help a member write a good Super Magic skill — sharpen the description into a real trigger, structure the steps, add the guardrails the workflow needs. Use when asked to "help me write a skill", "improve my skill", "why doesn't my skill trigger", or to review a draft skill's wording.
category: Automation & Flows
tools: [list_skills, load_skill, create_skill, update_skill, search_tickets]
---

# Skill Authoring Coach

Workshop a member's skill draft against the authoring standard this library uses: description-as-trigger, one workflow per skill, imperative steps, and guardrails that match the blast radius of what the skill does.

## When to use

- "Help me write a skill that does <workflow>."
- "My skill exists but Magic never picks it — fix the description."
- "Review this skill draft before I start using it."

## Steps

1. Load the draft (`load_skill` if it exists; otherwise take the pasted text) and check `list_skills` for an existing skill that already covers the workflow — improving one beats duplicating it.
2. Workshop against the standard, in this order:
   - **Description = trigger.** Rewrite it as *when to reach for this*, using the words the member would actually type. Test it: "if I typed <phrase>, would this description match?" A description that summarizes instead of triggering is the #1 authoring failure.
   - **One workflow.** If the draft does three unrelated things, split it and say why.
   - **Steps.** Numbered, imperative, tool names where a call happens, branches inline ("If X → Y; otherwise → Z"), and an explicit final step saying what to output and in what format.
   - **Guardrails.** Match them to what the skill touches: any write needs a confidence gate and confirm-before-destructive; searches need result-cap honesty; PSA-bound notes need plain text; when-in-doubt-do-nothing where a wrong action is worse than none.
3. If the skill will run unattended inside a flow, apply the Unattended Output Discipline base skill and add its contract to the draft.
4. Show the revised skill side-by-side with a one-line rationale per change. On explicit confirmation, `create_skill` or `update_skill`.
5. Suggest a quick test: 2–3 prompts that should trigger it and one that shouldn't.

## Guardrails

- The skills family is in-app SuperAgent only. **Degradation:** when absent, produce the finished skill text for the member to paste into the skill editor.
- Never create or update a skill without showing the full revised text and getting confirmation.
- Strip anything unshareable while editing: credentials, API keys, person/board ids, client names — replace with placeholders and tell the member you did.
- Coach, don't hijack: keep the member's intent and vocabulary; change structure and guardrails, not the workflow's goal.
