---
name: Teams External Guest Guide
description: Draft reply-ready instructions for an end user to join an external Teams meeting or accept guest access to another organization's Teams — "tell the user how to join the client's Teams meeting."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Teams External Guest Guide

Produces a client-ready instruction block for a user invited to a Teams meeting or workspace run by another organization — joining an external meeting as a guest, or accepting guest access to a partner's tenant — the two situations users most often stumble on.

## When to use

- "User needs to join a client's Teams meeting and can't get in — send them how."
- "User got a guest invite to another company's Teams and doesn't know how to accept it."
- "How do I join a Teams meeting from an outside company?"

## Steps

1. **Confirm the scenario and the client's external-access posture** (search_itglue / search_hudu / search_knowledge_base / search_tickets). Two distinct cases: (a) **joining an external Teams *meeting*** (usually easy — click the link, join in browser or app) vs. (b) **accepting *guest access* to another org's Teams team/workspace** (an invite email, an "accept" flow, and switching tenants inside Teams). Note whether the client restricts external/guest access, which changes what's even possible. If the scenario or policy is unknown, ask the technician ONE question.
2. **Draft the branch that matches** — a meeting-join reply and a guest-access reply are different; don't merge them.
3. **Draft the instruction block** to end-user rules, one action per step with what-you'll-see cues:
   - **Join an external meeting:** click "Join the meeting" in the invite → choose "Open Teams" (if they have it) or "Continue on this browser" → they may land in a **lobby** ("please wait, someone will let you in" — that's normal, the host admits you) → set camera/mic before entering. Reassure that joining someone else's meeting doesn't require an account and doesn't give them access to anything else.
   - **Accept guest access:** open the invite email → click Accept/Get Started → sign in with their work account → approve the permission prompt → then, to reach it later, use the **tenant/organization switcher** in Teams (cue where it is, usually near their profile) to switch between their own org and the guest org. Explain that guest access shows them only what that org shared, and switching back returns them home.
   - The common gotcha: if they have both the "new" and "classic" Teams, or Teams open to the wrong org, meetings/teams can look missing — cue checking the org switcher.
   - Off-ramp: "If the link says you're not allowed to join, or the guest invite errors out, stop and reply — that's usually an access setting on one side, not something you did wrong, and it may need us or the other company's IT."
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Scenario match matters** — meeting-join and guest-access are different flows; sending the wrong one wastes a round-trip.
- Reassure on the privacy question users worry about: joining an external meeting or accepting guest access doesn't expose their own files/mailbox to the other org; guest access is scoped to what that org shared.
- External/guest access can be blocked on either the client's side or the host's — the draft flags that a failure may be a policy on either end and routes to the desk rather than blaming the user.
- No admin steps (external-access/guest policy in the Teams admin center, cross-tenant settings) in the user block — those are tech actions, and a block may require the *other* org's admin.
- Localizable; version-cautious UI cues (new vs. classic Teams differ).
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled; fall back to KB and ticket history.

## Cross-references

- end-user-guides/teams-meeting-guide — the internal-meeting counterpart.
- end-user-guides/bluetooth-device-pairing-guide — when the real blocker is headset audio in the meeting.
- communication/email-baseline-standard.
