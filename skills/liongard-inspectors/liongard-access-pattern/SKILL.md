---
name: Liongard Access Pattern
description: The base pattern for reading ANY system's config/state through Liongard — resolve environment, find the inspector launchpoint by systemType, verify it last ran successfully, then query the dataprint. Use it directly for systems without a dedicated skill, and follow it inside every liongard-inspectors skill.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, liongard_alert, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Access Pattern

Liongard is a universal read plane: there is no per-inspector tool, but the generic tools reach every inspector the partner runs (~300 system types — firewalls, switches, hypervisors, backup platforms, cloud tenants, LOB apps). This skill is the discipline for using it safely: find the launchpoint, verify freshness, query the dataprint, and degrade honestly when the inspector is absent or stale.

## When to use

- Any question of the shape "what is <system>'s config/state at <client>?" where Super Magic has no native integration for that system.
- As the shared foundation invoked by every other skill in this category (they assume these steps).
- "Does <client> even have a Liongard inspector for <system>?"
- A dedicated skill exists for the system family → prefer it; it layers system-specific questions on top of this pattern.

## Steps

1. **Resolve the environment.** `liongard_environment` with the client name; rank matches and state your pick. If no environment exists, Liongard cannot answer for this client — go to step 7.
2. **Find the launchpoint.** `liongard_launchpoint` filtered by systemType (e.g. "Meraki", "FortiGate", "Veeam", "VMware") within that environment. Note there may be multiple launchpoints (multiple firewalls, multiple hypervisor clusters) — enumerate them, don't grab the first.
3. **Verify before trusting.** Check each launchpoint's last-run status and timestamp. If the last run failed or the launchpoint is inactive, the dataprint is stale-as-of-last-success — every downstream answer must carry that caveat, and repeated failures are themselves a finding worth a ticket note. Cross-check with `liongard_timeline` if the launchpoint status is ambiguous.
4. **Query the data.** Two routes:
   - `liongard_metric` — evaluate a metric or JMESPath expression against the inspector's dataprint for precise values (counts, versions, lists). Verify field paths against the live dataprint — schemas vary by inspector version; probe with a broad expression first, then narrow.
   - `liongard_query` — natural-language questions across inspector data when you don't know the shape or the question spans systems. Good first pass; confirm load-bearing numbers via `liongard_metric` when they will drive action.
5. **Change history when the question is "what changed".** `liongard_detection` for detected changes on the system, `liongard_timeline` for run-over-run history, `liongard_alert` for anything escalated. (For full change-vs-ticket correlation, hand off to devices-and-infrastructure/liongard-change-review.)
6. **State freshness in every output.** Always include: inspector name, last successful run time, and the age of the dataprint ("as of <timestamp>, N hours/days ago"). A config answer without its age is wrong by omission.
7. **Degradation ladder** when the environment or inspector is absent, inactive, or failing:
   a. Documentation: `search_itglue` / `search_hudu` for the system's documented config — label it "documented, not inspected; may be stale".
   b. Ticket history: `search_tickets` for prior work on the system — label it "inferred from ticket history".
   c. If neither yields, say so plainly and recommend the tech verify on-device. Never fill the gap with typical/default values.
8. **Output**: the answer, the source line ("Liongard <inspector> inspector, last ran <when>"), caveats, and — if this feeds a ticket — a plain-text `add_ticket_note` on request.

## Guardrails

- Read-only. Liongard tools observe; they never change the target system. Remediation is a tech's job — this skill supplies evidence.
- Never trust a dataprint without checking the launchpoint's last-run state first. A confidently-stale answer is worse than "unknown".
- JMESPath field paths in the per-system skills are illustrative — schemas vary by inspector version. Verify against the live dataprint before presenting a value as fact.
- Result caps: if a metric or query result looks truncated, say so and narrow the expression rather than presenting a partial list as complete.
- Liongard sees what its inspectors inspect. Absence of data is not absence of the system — say "no inspector found for <systemType> in this environment", not "the client has no <system>".
- Inspector availability varies by Liongard subscription and version — if a systemType filter returns nothing, also try close name variants before concluding absence.
- If Liongard is not enabled for the tenant at all, state that up front and run the degradation ladder.
- Plain-text notes only when posting to tickets.
