---
name: Insider Risk Basics
description: A ticket smells like insider risk — data staging by a departing employee, sabotage signs, access abuse — the rule is DO NOT investigate solo: preserve evidence quietly, escalate to client leadership/HR per policy, and keep it confidential.
category: Security
tools: [search_tickets, add_ticket_note, update_ticket, search_itglue]
---

# Insider Risk Basics

Insider-risk situations are the one alert class where the desk's instinct to investigate is wrong. These cases carry employment-law, privacy-law, and evidence-integrity consequences that a service desk is not positioned to manage — the skill here is recognizing the smell, freezing the evidence, and handing it to the people with the authority to act.

## When to use

- A DLP, access, or activity alert suggests deliberate data movement by an employee — especially one who is departing, disgruntled, or recently disciplined
- A client contact reports suspicion about their own employee's system activity
- Any investigation (dlp-alert-triage, security-alert-response) surfaces signals of intentional insider misuse rather than external compromise or accident

## Steps

1. Recognize the category: external attacker → normal IR runbooks. Accident or policy ignorance → normal support/training path. Signs of *intentional* insider activity — bulk copies to personal storage around a resignation, access outside role scope with evasion behavior, sabotage indicators, credential sharing for concealment — → this skill, and the solo investigation STOPS here.
2. Preserve evidence, quietly and passively:
   - Record what has already been observed: alert contents, timestamps, systems involved — in a restricted plain-text note (see step 4 for where).
   - Direct the technician to preserve, not pull: ensure relevant logs won't age out of retention, hold existing exports as-is, and note (don't act on) accounts and devices involved. Recommend the client consider a litigation-hold process where mail/file preservation is needed (the litigation-hold skill covers the mechanics when formally requested).
   - Do NOT image machines, pull the user's mailbox, review their files, or expand log searches beyond what already fired — scope expansion is for whoever the client authorizes, under their legal guidance. Evidence gathered out of process can be unusable and can itself create liability.
3. Do not tip off: no confrontation, no questions to the suspected employee, no account changes they would notice (no sudden access revocations, password resets, or monitoring they can detect) unless the client's leadership directs it after escalation. Premature action destroys both the inquiry and, if the suspicion is wrong, the employee's standing.
4. Confidentiality discipline: the matter is discussed with the minimum set of people — the desk's management and the client's designated authority. Keep the ticket restricted per the desk's mechanism (internal-only notes; a generically-titled ticket rather than "Investigating <user>"). The suspected person's manager is NOT automatically in the loop — managers can be involved parties; the client's leadership/HR decides who knows.
5. Escalate per policy, fast: check the client's documented policy for insider/HR-security matters (search_itglue) and route to the named authority — otherwise to the client's senior leadership via the desk's management, as a call or restricted channel rather than a broadly-visible ticket thread. Present observations only: what was observed, when, what has been preserved. The client decides what happens next — HR process, legal counsel, formal investigation, law enforcement.
6. Then follow direction: after escalation the desk acts only on documented instructions from the client's authorized decision-maker (e.g. "revoke access at 5 p.m. Friday," "preserve this mailbox") — each instruction and its execution recorded with timestamps. If the verdict turns out to be external compromise after all, hand back to the standard runbooks.

## Guardrails

- DO NOT investigate solo — no evidence-gathering beyond passive preservation of what already exists, no content review, no user interviews. Recognizing and escalating IS the job done well.
- Never confront, alert, or visibly act against the suspected employee ahead of client direction.
- Observations, not accusations: notes record events and evidence state, never intent conclusions or the word "guilty" in any form — wrong accusations harm real people and create liability.
- Minimum-necessary confidentiality, and the suspect's manager is not presumed inside the circle.
- Everything after escalation happens on the client's written instruction, timestamped; nothing on verbal hallway authority.
- Degradation: no documented client policy found → escalate through the desk's management to the client's most senior appropriate contact; absence of policy never means the desk investigates instead.
