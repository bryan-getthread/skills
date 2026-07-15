---
name: Training Video Outline
description: Outline an internal or client-facing enablement video from the topics that actually generate tickets — scene-by-scene structure, talking points, and demo steps.
category: Training & Enablement
tools: [search_tickets, search_knowledge_base, search_thread_docs]
---

# Training Video Outline

Turns a recurring ticket topic into a ready-to-record video outline: audience, runtime target, scene-by-scene structure with talking points and demo steps. Works for internal tech training and client-facing how-to content.

## When to use

- "Outline a training video on <topic>"
- "What should we make how-to videos about?" (mine the ticket data for candidates)
- A lead wants client-facing self-service content to deflect repeat tickets
- Turning a new SOP or KB article into video form

## Steps

1. Establish topic and audience. If the topic is given, confirm whether the video is for internal techs or end clients — the script differs completely. If asked what to make, search recent tickets for high-recurrence, low-complexity topics (the classic deflection candidates: password self-service, MFA re-enrollment, shared-mailbox access, printer mapping) and present the top 3–5 with rough recurrence counts, noting any result caps.
2. Gather source material: the relevant KB articles (search_knowledge_base) and, for internal videos, how recent real tickets on the topic were actually resolved — the outline should teach the desk's real procedure, not a generic one.
3. Set format targets: client-facing = 2–4 minutes, one task, zero jargon; internal = 5–10 minutes, includes edge cases and where it goes wrong.
4. Produce the outline:
   - Title and one-line promise ("After this video you can X")
   - Audience, runtime target, prerequisites
   - Scene-by-scene structure: for each scene, the on-screen action (what to show/demo), the talking points (bulleted, not a verbatim script unless asked), and an estimated duration
   - Cold-open hook: the symptom the viewer recognizes ("Getting this error? Here's the fix")
   - For internal videos: a "where people go wrong" scene sourced from real ticket mistakes (sanitized)
   - Closing scene: recap + what to do if it didn't work (how to open a ticket properly)
5. Add a production checklist: what test account/environment the recorder needs, screens that must be redacted (client data, credentials), and any steps that cannot be shown live.
6. Offer follow-ups: a full word-for-word script for one scene, a companion KB article, or outlines for the next topic on the candidate list.

## Guardrails

- Demo steps must match the team's documented procedure or observed real resolutions; if neither exists, mark steps as "verify before recording" rather than presenting guesses as procedure.
- Client-facing outlines never include internal tooling, escalation paths, or anything that reveals other clients.
- Sanitize any real-ticket examples used for the "where people go wrong" scene.
- Flag any step in the procedure that involves credentials or elevated access so the recorder knows to blur or stage it.
- Recurrence counts from ticket mining are estimates when result caps may apply — say so.
