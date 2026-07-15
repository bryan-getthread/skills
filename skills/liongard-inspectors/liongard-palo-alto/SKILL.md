---
name: Liongard Palo Alto Read
description: Interrogate a client's Palo Alto firewall through Liongard — PAN-OS version, config changes and commit history, admin accounts/activity, HA state, policy posture. Use for change-window questions on PAN gear, admin-access review, or HA sanity checks during incidents.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Palo Alto Read

Reads Palo Alto Networks firewall posture — PAN-OS version, security policy set, administrator accounts, HA pair state, and above all the change record — from the Liongard inspector. On PAN gear the change story (who committed what, when) is usually the question being asked.

## When to use

- "Did anything change on <client>'s Palo before this outage?" — commit/config-change correlation.
- "What PAN-OS are they on?" — advisory or upgrade-planning check.
- "Who has admin on the firewall, and has anyone unexpected been active?"
- Incident on an HA pair: "which unit is active, and is HA healthy?"
- Policy posture pass: rule counts, overly-broad rules, disabled logging.

## What the inspector captures

Device identity and PAN-OS version, security/NAT policy configuration, administrator accounts and roles, HA configuration and state, and configuration snapshots that drive change detections between runs. Panorama-managed estates may look different from standalone units. Inspector availability and coverage vary by Liongard version — confirm the launchpoint exists rather than assuming (per the access pattern).

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve environment, `liongard_launchpoint` with systemType "Palo Alto" (try "PAN-OS"/"Palo Alto Networks" variants), verify last-run success, note dataprint age. HA pairs: expect one launchpoint per unit or one for the pair — establish which before comparing.
2. Query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Version: PAN-OS version per unit — on an HA pair, mismatched versions between peers is a finding on its own.
   - Admins: administrator list with role — flag superuser accounts beyond the expected break-glass + management set.
   - HA: HA enabled/state fields — active/passive roles and sync state; "not synced" during an incident is a lead.
   - Policies: rule count, rules with source or destination "any" on untrust-to-trust, rules with log-at-end disabled.
3. **Change history is the main event**: `liongard_detection` and `liongard_timeline` for the PAN system — config-change detections between inspector runs approximate the commit history. Order them against the incident window; an unexplained config change with no matching ticket or change record is a flag even without an incident.
4. Output: facts, HA state (if asked), the change list with timestamps, source + dataprint age, flags. Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Incident correlation**: "traffic stopped passing at 14:02, config change detected between the 13:00 and 15:00 inspector runs" — that's the tee-up for the tech. Always time-adjacency, not proven cause, and note the detection granularity is bounded by inspector run frequency.
- **Change/audit**: unexplained changes feed change-and-problem-management review; admin-activity findings feed access review.
- **QBR**: PAN-OS currency and HA health are stable QBR posture lines.

## Guardrails

- Read-only; commits, upgrades, and HA failovers are human-owned.
- Detection timing is bounded by inspector run cadence — a change is "between run A and run B", never a precise commit timestamp unless the dataprint carries one. Say so.
- Report PAN-OS versions verbatim; map to vendor advisories only with verification, not from memory.
- Never reproduce keys, certificates, or credential fields from the dataprint.
- If no Palo Alto launchpoint exists, degrade per the access pattern and note that Panorama-managed estates sometimes have the inspector pointed at Panorama instead — check that systemType too before concluding absence.
- Plain-text notes only.
