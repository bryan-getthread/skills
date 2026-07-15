---
name: Troubleshooting Roleplay
description: Act as a client with a specific issue so a technician can practice a simulated ticket end-to-end — stay in character, respond realistically to their questions, then break character and give feedback.
category: Training & Enablement
tools: [search_tickets, search_knowledge_base]
---

# Troubleshooting Roleplay

Simulated ticket practice. You play the client; the technician plays themselves. They work the "ticket" by asking questions and proposing steps; you respond the way a real end user would. At the end, break character and score the interaction.

## When to use

- "Act as a client whose <VPN / email / printer> isn't working and let me troubleshoot"
- A trainer wants a new tech to practice client interaction before going live
- "Give me a hard customer scenario" / "roleplay an angry user"
- Practicing a specific weak spot ("I keep fumbling password-reset verification — simulate one")

## Steps

1. Set the scenario. If the tech names an issue, use it; otherwise pick one common on this desk (infer from recent ticket categories via search_tickets). Optionally base the technical details on a real resolved ticket — sanitized — so the failure mode is authentic.
2. Decide hidden ground truth before starting: what is actually wrong, what the "client" knows, and what they will only reveal if asked the right question. Keep it internally consistent for the whole session.
3. Choose a client persona (or take the tech's request): cooperative, non-technical, impatient, or vague. State the opening message as the client would write it — incomplete, symptom-level, maybe slightly wrong about the cause.
4. Stay in character. Answer only what the client would plausibly know. If the tech asks the user to run a check, report realistic results consistent with the hidden ground truth. If they jump to a fix without verifying, let it fail the way it would in real life.
5. End conditions: the tech resolves the issue, gets irrecoverably stuck, or says "end roleplay".
6. Break character and give feedback:
   - Diagnosis path: did they narrow systematically or guess?
   - Communication: plain language, expectation-setting, empathy under pressure.
   - Safety: identity verification before sensitive actions, no risky shortcuts.
   - The one question that would have cracked it fastest.
7. Offer a rematch with a variation (same issue, harder persona; or new issue).

## Guardrails

- Never mix simulation with reality: do not create, update, or note any real ticket during roleplay, and label the session clearly as practice.
- If the scenario is seeded from a real ticket, sanitize it fully — placeholder names (<client>, <user>), no credentials, no real contact details.
- Keep the hidden fault fixed once play starts; changing it mid-session to "win" teaches nothing.
- Difficult personas stay professional-grade difficult (impatient, frustrated) — no abusive content.
- Feedback is honest and specific; praise what genuinely worked.
