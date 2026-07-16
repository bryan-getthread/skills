---
name: Doc Gap Detector
description: Find tickets where a security- or configuration-impacting change (firewall, MFA, DNS, admin access) was made with no linked or matching documentation update.
category: Documentation
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note]
---

# Doc Gap Detector

Sweep completed tickets for changes that alter a client's security posture or configuration
and verify each one left a documentation trail. Report the gaps; do not fix them silently.

## When to use

- "Which changes last month never got documented?"
- After a busy change window: "audit the firewall/MFA/DNS work for doc gaps."
- A compliance or audit prep pass needs proof that config changes are documented.
- A new engineer keeps finding the environment doesn't match the docs.

## Steps

1. Pull resolved tickets for the window (default: last 30 days) with `search_tickets`, running **separate searches per change class**: firewall/network rules, MFA/conditional access, DNS records, admin/privileged access grants, VPN configuration, backup configuration, licensing/tenant changes.
2. For each hit, confirm from the thread that a change was actually **made** (not merely proposed or recommended) — zero-assumption: a recommendation is not a change.
3. For each confirmed change, look for a documentation trail:
   - A doc reference or link in the ticket notes, or
   - A matching, recently updated entry via `search_itglue` / `search_hudu` (where enabled) and `search_knowledge_base`.
4. Classify each change: **Documented** (trail found), **Gap** (no trail), or **Unverifiable** (docs platform not searchable for this tenant or result cap hit).
5. Output the gap report grouped by <client>: change made, ticket number, date, engineer, change class, and what documentation is missing. Rank security-impacting gaps (MFA, admin access, firewall) first.
6. If asked, post a plain-text internal note on each gap ticket via `add_ticket_note` ("Documentation gap: <change class> change has no doc update — please document") — confirm the ticket list with the requester before posting any notes.

## Guardrails

- Report only changes the thread confirms were executed. Never list a recommendation or quote as an undocumented change.
- Absence of evidence is reported as a **gap to verify**, not an accusation — the doc may live somewhere unsearchable; say so.
- **Result-cap honesty:** if searches cap out, state the sweep is partial and name the classes affected.
- Read-mostly: the only write is the optional internal gap note, and only after explicit confirmation. Never edit documentation or tickets beyond that.
- If neither `search_itglue` nor `search_hudu` is enabled, check ticket notes and the Thread KB only, and state that external doc platforms were not checked.

## Unattended (Flows) variant

- Follows the Unattended Output Discipline contract: the entire reply is the plain-text gap report — per <client>: change made, ticket number, date, engineer, change class, and what documentation is missing — security-impacting gaps (MFA, admin access, firewall) first. No narration.
- Deterministic inputs from the flow: the window and the change classes to sweep. Capped or unsearchable classes appear under an UNVERIFIABLE heading inside the report, never silently dropped.
- Only thread-confirmed executed changes appear; recommendations and quotes never make the report, in any mode. Gaps stay worded as "gap to verify", not accusations.
- No gaps found → reply exactly `NO DOC GAPS FOUND.` (append ` (SWEEP PARTIAL)` if any search capped).
- Permitted writes: none. The per-ticket gap notes require the requester to confirm the ticket list — that stays attended.
