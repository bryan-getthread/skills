---
name: Interview Scheduling Coordination
description: Run the logistics of hiring interviews — candidate communications, panel scheduling against the team's real availability, and a feedback-collection cadence that closes before memories fade. Use for the coordination; use interview-simulation for the assessment itself.
category: MSP Business Operations
tools: [search_members, search_tickets, create_ticket, update_ticket, add_ticket_note, schedule_ticket, update_schedule_entry, create_timezest_scheduling_request, list_timezest_appointment_types, list_timezest_resources]
---

# Interview Scheduling Coordination

Handles the moving parts of interviewing that make MSPs lose good candidates: prompt, warm candidate emails; panel slots that don't collide with on-call and client commitments; and structured feedback collected within a day of each interview. The content of the interview — questions, rubric, scoring — lives in the interview-simulation skill (training-and-enablement); this one makes sure the interview happens and the signal gets captured.

## When to use

- "We're interviewing 3 candidates for the L2 role — coordinate the schedule."
- "Draft the interview-confirmation email for <candidate>."
- "Get me panel feedback from everyone who interviewed <candidate> yesterday."
- A hiring manager wants one tracked place for a role's interview logistics instead of scattered chat threads.

## Steps

1. Set up (or find) the coordination ticket per role on the internal board — one ticket per requisition, not per candidate email. Track candidates by first name + initial in notes; the full record lives in whatever ATS/HR system the tenant uses.
2. Confirm the interview structure with the hiring manager: stages (screen, technical, panel, final), duration each, who must attend which, and the decision deadline. Point the technical-stage interviewers at interview-simulation so every candidate gets the same rubric.
3. Build panel availability honestly: check interviewers' schedules and standing commitments (search_members, schedule entries) — on-call rotations, client onsites, maintenance windows. Never book a tech's interview slot over a client commitment; the desk still runs during hiring. If TimeZest is connected, offer candidate self-scheduling against the panel's real availability (create_timezest_scheduling_request); otherwise propose 2–3 concrete slots per stage.
4. Draft candidate communications for the hiring manager or recruiter to send — warm, specific, and prompt:
   - Confirmation: date/time with timezone, format (video link/office address), who they'll meet and their roles, duration, and anything to prepare.
   - Reschedule and reminder notes as needed; a day-before reminder measurably reduces no-shows.
   - Keep every candidate message professional and equal — same template, same tone, for every candidate at a stage.
5. Feedback cadence: within 24 hours of each interview, prompt each panelist for structured feedback (their rubric scores if they used interview-simulation, plus hire/no-hire inclination and one paragraph of evidence). Track submissions on the ticket and chase gently once; escalate silence to the hiring manager rather than nagging forever.
6. Package the stage summary for the hiring manager: who has interviewed, feedback in vs outstanding, and scheduling status for remaining stages. The summary reports; it does not tally votes into a recommendation.

## Guardrails

- **No hiring judgments.** You collect and organize panel feedback; you never aggregate it into a verdict, rank candidates, or characterize a candidate beyond what panelists wrote. The hiring decision is entirely human.
- Candidate PII discipline: minimal identifying detail in tickets (no home addresses, no salary expectations, no visa/health/family circumstances even if mentioned in emails). If sensitive context arrives, leave it out of notes and hand it to the hiring manager directly.
- Equal process per stage: same communications, same lead time, same panel structure for every candidate — consistency is both fairness and legal hygiene.
- Interview feedback is need-to-know: hiring manager and panel only. Never on shared boards, never in team channels.
- Candidate communications are drafts the recruiter/hiring manager sends from their own address — an unknown desk address emailing candidates reads as spam and leaks the process into the ticket system.
- If a panelist's only free slots are during their on-call window, say so and let the hiring manager choose the trade-off — don't silently double-book.
