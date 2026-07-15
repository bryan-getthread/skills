---
name: Liongard Network Documentation Sync
description: Keep network documentation honest by diffing what Liongard inspectors actually see (firewalls, switches, wireless, hypervisors, servers) against what the doc platform says, then drafting corrections. Use for "is our documentation for <client> accurate", pre-onboarding/QBR doc audits, or after a project to true-up the docs.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_timeline, search_itglue, search_hudu, search_tickets, add_ticket_note]
---

# Liongard Network Documentation Sync

Uses inspector dataprints as ground truth to audit the documentation platform: what the docs claim vs what the inspectors observed, producing a diff and draft corrections. Documentation drifts after every project and every emergency change — this closes the loop. (The generic docs-refresh workflow lives in documentation/environment-facts-updater; this skill is its Liongard-powered network/infrastructure edition — cross-reference rather than duplicate.)

## When to use

- "Is <client>'s network documentation still accurate?" — periodic doc audit.
- After a project or a burst of emergency changes: true-up the affected docs.
- Onboarding a client whose docs came from a previous provider — verify before trusting.
- A tech got burned by stale docs on a ticket ("the doc said the firewall was X") — sweep for more of the same.
- QBR prep wants a documentation-health line.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, then `liongard_launchpoint` unfiltered to enumerate EVERY inspector the client runs. The launchpoint list itself is finding #1: systems inspected but undocumented, and documented systems with no inspector.
2. Pull the documented picture: `search_itglue` / `search_hudu` for the client's network/infrastructure docs — firewall configs, switch/VLAN docs, wireless docs, server lists, hypervisor docs. Note each doc's last-updated date where visible.
3. For each system family present in both, pull the live facts via `liongard_metric` / `liongard_query` using the matching skill's angles (liongard-meraki, liongard-unifi, liongard-fortigate, liongard-sonicwall, liongard-palo-alto, liongard-cisco-network, liongard-watchguard-config, liongard-vmware, liongard-hyperv, liongard-veeam-posture, liongard-windows-server, liongard-sql-server). Verify each inspector's last-run success and carry its dataprint age.
4. Diff, in three buckets:
   - **Doc wrong**: documented value contradicts the dataprint (documented VLAN map vs actual, documented firmware vs actual, server list missing hosts the inspectors see).
   - **Doc silent**: inspected facts with no documentation home (undocumented SSIDs, admins, VPN tunnels, servers).
   - **Inspector silent**: documented systems no inspector covers — a coverage gap to recommend, not proof the doc is wrong.
5. Use `liongard_timeline` to date the drift where useful — "doc last updated <date>; inspector detected the change <date>" makes the correction credible.
6. Draft corrections: per-document, the specific field-level updates with the inspector value, its as-of timestamp, and the source inspector named. Drafts only — the search_itglue/search_hudu surface is read-only; corrections are applied by a human in the doc platform (or via documentation/doc-platform-migration tooling where the partner has write paths).
7. Output: the diff table (doc / claim / observed / source inspector / as-of / severity), the coverage-gap list both directions, and the correction drafts. Offer a plain-text `add_ticket_note` or a ticket per correction batch via the requester's preference.

## Guardrails

- Inspector data is ground truth for *observed* state only — intent lives in docs. A mismatch means "drifted or intentional", so corrections are drafts with evidence, never silent rewrites of design intent.
- Never write to the documentation platform from this skill; produce drafts for human application.
- Every observed value carries its inspector + dataprint age; never diff a stale dataprint against docs without saying so — a failing inspector can make correct docs look wrong.
- Never copy credentials, PSKs, or secrets into diffs or drafts — reference their documented location only.
- Result caps: a large environment may exceed search/result limits — audit per system family and state coverage ("audited firewall + wireless docs; servers pending") rather than implying a full sweep.
- If neither IT Glue nor Hudu is enabled, invert the deliverable: generate a from-scratch documentation skeleton from the inspector data, clearly labeled as machine-observed and needing human review.
- Plain-text notes only.
