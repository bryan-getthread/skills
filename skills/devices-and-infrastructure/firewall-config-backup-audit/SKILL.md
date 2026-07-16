---
name: Firewall Config Backup Audit
description: Verify that every firewall's current configuration is actually backed up and recent — via Liongard change history or the vendor's own backup state — and flag any device whose config backup is missing or stale. Use when someone asks "are our firewall configs backed up", before a firmware change, or for a config-backup posture review.
category: Devices & Infrastructure
tools: [liongard_launchpoint, liongard_timeline, liongard_detection, liongard_metric, search_itglue, create_ticket, add_ticket_note]
---

# Firewall Config Backup Audit

Answer one question with evidence: for each firewall, is the current running config captured somewhere we could restore from, and how fresh is it? Surface every gap before it becomes a rebuild-from-scratch incident.

## When to use

- "Are all our firewall configs backed up?"
- Before a firewall firmware upgrade or rule change, to confirm a rollback point exists.
- A config-backup posture review across a client or the whole book.
- After a firewall was replaced/re-added, to confirm it's now being captured.

## Steps

1. Enumerate firewalls in scope. Where Liongard runs, `liongard_launchpoint` filtered by firewall systemTypes (e.g. "Palo Alto", "Fortinet", "SonicWall", "Meraki", "WatchGuard", "Cisco ASA") lists each device's inspector, launchpoint, and last successful run. Cross-reference documented firewall inventory (`search_itglue`) so a firewall that exists but has no inspector is caught as a blind spot, not silently omitted.
2. For each firewall, determine config-backup state from the best available signal:
   - Liongard as the read plane: `liongard_timeline` and `liongard_detection` show whether the inspector is capturing configuration changes and when it last did — a captured, dated config change history is evidence the config is being read and versioned. Use `liongard_metric` to pull the last-config-capture timestamp or firmware/config fields where the inspector exposes them.
   - Vendor-native backup (e.g. cloud-managed controller config export, appliance auto-backup, or a documented scheduled export) where that is the client's backup mechanism — confirm from documentation which mechanism is authoritative for each device.
3. Classify each firewall: Backed up & fresh (a current config within the expected cadence), Stale (backup exists but older than the cadence — state how old), or No backup / unverifiable (no inspector, inspector not running, or no vendor backup evidence). Always state the age of the most recent captured config and the dataprint age of the source.
4. For every gap, note the likely cause (inspector absent or failing, vendor backup not configured, credentials expired) and the remediation owner — do not present a gap as fixed.
5. Output an audit table: firewall, site, source of truth, last config capture (with age), status, and the specific gap/remediation. Flag Stale and No-backup rows first. Post to the ticket as a plain-text note (`add_ticket_note`) or `create_ticket` to track remediation of the gaps.

## Guardrails

- Distinguish "the inspector ran" from "the config is backed up" — an inspector that reads config is evidence of a capture point, but confirm the client's actual restore mechanism from documentation before calling a firewall protected.
- Trust Liongard data only after confirming the inspector last ran successfully; always report dataprint/last-capture age. A firewall with no inspector and no vendor-backup evidence is "unverifiable", never assume it's fine.
- This skill reads and reports only — it does not create, trigger, or configure any backup. Remediation is a human/console action.
- Never expose firewall credentials, config contents, or environment identifiers in output.
- If Liongard is not enabled, degrade to documentation and vendor-backup evidence only and say the audit is partial.
- Notes posted to tickets are plain text — no markdown, no emojis.
