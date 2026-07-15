---
name: Skills Matrix Builder
description: Build a team capability inventory from resolved-ticket evidence — who has demonstrated what technology, where coverage is one-person-deep, and where the client stack demands skills nobody has shown yet. Evidence-based, never vibes-based; a coverage map, not a performance ranking.
category: MSP Business Operations
tools: [search_tickets, search_members, search_clients]
---

# Skills Matrix Builder

Builds the desk's skills matrix from what actually happened: resolved tickets are the evidence of who can do what. The output is a capability × person map with demonstrated-strength, some-exposure, and no-evidence levels, a bus-factor column showing where the desk is one resignation away from a gap, and a demand comparison against what the client stack generates. It exists to target training and hiring — not to grade people.

## When to use

- "Build a skills matrix for the team." / "Who can handle firewall work if <user> is out?"
- Planning training budgets: "where are our real capability gaps?"
- Before hiring: "what should the next hire be strong in?"
- After winning a client with an unfamiliar stack: "can we actually support this?"

## Steps

1. Agree the capability rows with the requester (a service manager or owner — this is a leadership tool). Default taxonomy, trimmed to what this desk sees: identity/M365 admin, networking (firewall/switch/wifi), server/virtualization, backup/DR, security incidents, macOS, line-of-business apps, telephony/VoIP, RMM/scripting/automation, cloud infrastructure.
2. Gather the evidence per member × capability from resolved tickets (search_tickets — split searches per member and per capability keyword family; disclose result caps rather than presenting capped counts as totals). Look at tickets they **resolved**, weighted by recency (last 6–12 months) and depth (an escalation they closed counts more than a password reset in the same category).
3. Score each cell on evidence, with the rule stated up front — a level requires tickets behind it:
   - **Demonstrated** — resolved multiple non-trivial tickets in the area recently.
   - **Exposure** — a few resolutions, or participation on escalations others led.
   - **No evidence** — nothing in the window. Explicitly NOT the same as "can't": new hires, project work, and internal roles all produce ticket-quiet competence. Mark it "no ticket evidence", never "lacks skill".
4. Compute the demand side: ticket volume per capability area across the client base in the same window, plus anything the stack implies is coming (a newly onboarded client's technologies). Compare supply to demand.
5. Flag the two findings that matter:
   - **Single-coverage risks:** capabilities where exactly one person is at Demonstrated — the desk's bus-factor list, ranked by that area's ticket volume.
   - **Demand gaps:** areas with real volume and thin coverage — these are the training targets (pair with certification-study-plan) or the next hire's profile.
6. Deliver as a matrix (members × capabilities, with the evidence window and caps noted) plus a half-page of findings. Present it to the requesting manager only, and recommend a validation pass: each member reviews their own row and adds what tickets don't show (prior-job experience, certs, project work) before anyone treats the matrix as truth.

## Guardrails

- **Coverage map, not a report card.** Never rank members, never present row totals as "who's best", and never let the matrix be the input to a performance conversation — that's tech-performance-review's territory, with its own canon. If the requester asks "so who's my weakest tech?", decline the reframe: this data answers coverage, not worth.
- Evidence-based, never vibes-based — but say what the evidence can't see. Ticket history under-measures new hires, leads who unblock others without owning tickets, and anyone on project/internal work. The member-validation pass in step 6 is mandatory framing, not optional politeness.
- Respect role context: a dispatcher's sparse technical row is their job design, not a gap.
- Split searches per signal; disclose caps. A capped count presented as a total corrupts the whole matrix.
- Manager's-eyes output, and the matrix is shared with the team only after the validation pass and with capability framing ("here's our coverage") — individual rows are discussed 1:1, not compared publicly.
- Keep client names out of the matrix itself ("high firewall demand", not "<client>'s firewall") — the artifact travels further than the analysis did.
