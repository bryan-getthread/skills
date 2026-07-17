---
name: Liongard Access Pattern
description: The base pattern for reading ANY system's config/state through Liongard — resolve environment, find the inspector launchpoint by systemType, verify it last ran successfully, then query the dataprint. Use it directly for systems without a dedicated skill, and follow it inside every liongard-inspectors skill.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, liongard_alert]
connectors: [Liongard]
---

# Liongard Access Pattern

**When to use:** Any "what is <system>'s config/state at <client>?" question where Super Magic has no native integration for that system, "does <client> even have an inspector for <system>?", or as the shared discipline every other liongard-inspectors skill assumes. If a dedicated skill exists for the system, prefer it — it layers system-specific angles on top of this pattern.

## Prompt

```
Read a system's configuration/state at CLIENT_NAME through Liongard. This is a read-only observation — Liongard never changes the target system.

1. Resolve the environment: liongard_environment for CLIENT_NAME. Rank matches and state your pick. If no environment exists, Liongard cannot answer for this client — go straight to the degradation step.
2. Find the launchpoint: liongard_launchpoint filtered by the target systemType (e.g. "Meraki", "FortiGate", "Veeam", "VMware") within that environment. There may be several (multiple firewalls, multiple hypervisor clusters) — enumerate them all, don't grab the first. If a filter returns nothing, try close name variants before concluding absence; inspector availability varies by Liongard subscription and version.
3. Verify before trusting: check each launchpoint's last-run status and timestamp. If the last run failed or the launchpoint is inactive, the dataprint is stale-as-of-last-success — carry that caveat on every downstream answer, and treat repeated failures as a finding worth a ticket note. Cross-check liongard_timeline if the status is ambiguous.
4. Query the dataprint two ways: liongard_metric for precise values (counts, versions, lists) via a metric or JMESPath expression — probe broadly first, then narrow, and verify every field path against the live dataprint since schemas vary by inspector version; liongard_query for natural-language questions when you don't know the shape or it spans systems (confirm load-bearing numbers with liongard_metric before they drive action).
5. "What changed?" → liongard_detection for detected changes, liongard_timeline for run-over-run history, liongard_alert for anything escalated.
6. State freshness in every output: inspector name, last successful run time, and dataprint age ("as of <timestamp>, N hours/days ago"). A config answer without its age is wrong by omission.
7. If a result looks truncated, say so and narrow the expression rather than presenting a partial list as complete. Absence of data is not absence of the system — say "no inspector found for <systemType> in this environment," never "the client has no <system>."
8. Degrade honestly when the environment/inspector is absent, inactive, or failing: search_itglue / search_hudu for documented config (label "documented, not inspected; may be stale") → search_tickets for prior work (label "inferred from ticket history") → else say so plainly and recommend the tech verify on-device. Never fill the gap with typical/default values.
9. Output: the answer, a source line ("Liongard <inspector> inspector, last ran <when>"), caveats, and — if this feeds a ticket — a plain-text add_ticket_note only on request.
```
