---
name: Phishing Simulation Program
description: Plan or coordinate a phishing-awareness simulation campaign for a client — scope, cadence, lure difficulty, a no-shame reporting culture, and keeping the desk's triage from colliding with the simulation.
category: Security
tools: [search_tickets, search_clients, search_contacts, search_knowledge_base, add_ticket_note, create_ticket, schedule_ticket]
---

# Phishing Simulation Program

Turns "we should run phishing tests" into a coordinated program: who gets simulated and how often, how difficulty ramps, how results are reported without shaming anyone, and — critically — how the desk's phishing-triage keeps treating real phish as real while the campaign runs.

## When to use

- A client asks to start (or restart) phishing simulations, often driven by insurance or compliance requirements
- A campaign is being planned and the desk needs scope, schedule, and coordination tickets
- Simulation results are in and the client wants the readout and next-cycle plan

## Steps

1. Scope the program with the client's stakeholder: which user groups (all staff; ramping by department is fine, exempting executives is not — leadership is the most-targeted group in real attacks), cadence (monthly or quarterly is typical; more frequent breeds alarm fatigue), and difficulty progression (start with obvious lures, ramp toward role-relevant ones). Record the agreed scope in a plain-text note.
2. Culture rule, set before the first send: the program measures the organization's resilience, not individuals' failures. Reported-rate is the headline metric to improve; click-rate trends matter, but named-individual click lists go only to the client's designated program owner, never into general reporting. Clicking earns a short training moment, not a reprimand — this is the no-shame commitment, and it should be stated in the client's program charter.
3. Simulation-detection interplay — the coordination step that protects the desk: document the simulation platform's sending domains and link domains in the client's knowledge record (search_knowledge_base to check what's already recorded; flag gaps for the tech to fill). The phishing-triage skill's simulation branch matches against exactly this record — if it isn't documented, every simulation email becomes a real investigation and every reply skews the client's metrics.
4. Whitelisting and delivery checks: the simulation domains need mail-gateway/filter allowances so results measure users, not the spam filter. Create a coordination ticket (create_ticket) for the gateway work, and schedule the campaign window (schedule_ticket) so the desk knows when simulation-driven report volume is expected.
5. During the campaign: user reports of simulation emails are GOOD outcomes — phishing-triage closes them internally without replying (a reply skews metrics). Real phishing does not pause for campaigns: anything not matching the documented simulator domains gets the full real-phish treatment. Never assume "it's probably the simulation."
6. Readout: report cohort-level metrics — report rate, click rate, credential-entry rate, trend versus prior cycles — with result-cap honesty on any ticket-derived counts. Recommend the next cycle's difficulty and any targeted training for high-risk patterns (not named individuals in general reporting). Feed repeat-pattern findings to the security-awareness-coordination skill for training follow-up. Output is the campaign plan or readout as chat/note; the simulation platform itself is operated by the technician or the client's program owner.

## Guardrails

- No-shame is non-negotiable: named-individual results go only to the designated program owner; general reporting stays cohort-level. Never publish "wall of shame" lists.
- Never let a live campaign lower the desk's guard: only an exact match against documented simulator domains closes a report as simulation — partial matches get investigated as real.
- Do not send or schedule simulation emails yourself; this skill plans and coordinates — the platform owner executes sends.
- Lure content stays professional: no fake HR terminations, bonus revocations, or health-scare lures — realistic is the goal, cruel is a program-killer.
- Simulation domains and scope belong in the client's documentation before the first send; refuse to mark a campaign "ready" without that record.
- Degradation: without search_knowledge_base, confirm the simulator-domain record with the tech directly and note where it lives.
