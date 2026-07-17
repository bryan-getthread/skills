---
name: Liongard Palo Alto Read
description: Interrogate a client's Palo Alto firewall through Liongard — PAN-OS version, config changes and commit history, admin accounts/activity, HA state, policy posture. Use for change-window questions on PAN gear, admin-access review, or HA sanity checks during incidents.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard Palo Alto Read

**When to use:** "Did anything change on <client>'s Palo before this outage?" (commit/config-change correlation), a PAN-OS advisory/upgrade check, "who has admin and has anyone unexpected been active?", HA-pair health during an incident, or a policy-posture pass.

## Prompt

```
Read Palo Alto Networks firewall posture for CLIENT_NAME from the Liongard inspector — on PAN gear the change story (who committed what, when) is usually the question. Read-only — commits, upgrades, and HA failovers are human-owned.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "Palo Alto" (try "PAN-OS" / "Palo Alto Networks"). Verify last-run success and note the dataprint age. HA pairs: expect one launchpoint per unit or one for the pair — establish which before comparing. Panorama-managed estates sometimes have the inspector pointed at Panorama — check that systemType too before concluding absence.
2. Query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Version: PAN-OS per unit — on an HA pair, mismatched versions between peers is a finding on its own. Report versions verbatim; map to advisories only with verification, not memory.
   - Admins: administrator list with role — flag superuser accounts beyond the expected break-glass + management set.
   - HA: HA enabled/state fields — active/passive roles and sync state; "not synced" during an incident is a lead.
   - Policies: rule count, rules with source/destination "any" on untrust-to-trust, rules with log-at-end disabled.
3. Change history is the main event → liongard_detection and liongard_timeline for the PAN system: config-change detections between inspector runs approximate the commit history. Detection timing is bounded by inspector run cadence — a change is "between run A and run B," never a precise commit timestamp unless the dataprint carries one; say so. Order against the incident window (time-adjacency, not proven cause); an unexplained config change with no matching ticket is a flag even without an incident.
4. Never reproduce keys, certificates, or credential fields from the dataprint. Output: facts, HA state (if asked), the change list with timestamps, source + dataprint age, flags. Offer a plain-text add_ticket_note. Degradation: no Palo Alto launchpoint → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify on device."
```
