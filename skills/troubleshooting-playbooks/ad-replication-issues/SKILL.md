---
name: AD Replication Issues
description: Diagnose Active Directory replication failures — password changes not propagating, GPO version mismatches, logon oddities on one site, repadmin errors — by reading repadmin output first and treating destructive fixes as escalations.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, liongard_launchpoint, liongard_metric, liongard_timeline, web_search]
---

# AD Replication Issues

Replication problems present as everything else: stale passwords, missing users on one site, GPO weirdness, Kerberos errors. The rule: read `repadmin /replsummary` before theorizing, and treat anything destructive (metadata cleanup, lingering-object removal, non-authoritative restores) as a stop-and-escalate — a wrong move here can force a forest recovery.

## When to use

- Changes (password resets, new users, group membership) appear on some DCs but not others
- Event IDs 1311, 1388, 1988, 2042, 2087 or "target principal name is incorrect" between DCs
- The group-policy-troubleshooting playbook found GPO version mismatch across DCs
- A DC was restored from backup/snapshot, or was offline for a long period

## Steps

1. **History first.** search_tickets for this client on replication/DC symptoms and — critically — for any recent DC restore, snapshot revert, P2V/V2V, or long outage. A reverted VM snapshot of a DC is the USN-rollback headline; knowing it happened changes everything.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the DC inventory: how many DCs, sites, which is PDC emulator, virtualization platform, backup product. If Liongard's Active Directory inspector runs, use liongard_launchpoint + liongard_metric for DC list and replication-related metrics, and liongard_timeline for recent directory changes — state dataprint age.
3. **Get the numbers before theorizing.** Guide the tech to run on any DC: `repadmin /replsummary` (the triage view: largest delta and fails/total per DC), then `repadmin /showrepl <failing-DC>` for the exact error per partner and naming context. Also `dcdiag /test:replications`. Capture exact error codes — 8524, 8606, 8614, 8456/8457, 1722, -2146893022 each mean different things; do not paraphrase.
4. **Check the delta against the tombstone lifetime.** If the largest delta approaches or exceeds the tombstone lifetime (default 180 days on modern forests, 60 on some legacy — verify, don't assume), replication is deliberately blocked (error 8614 / event 2042) to protect against lingering objects. **This is a stop point:** do NOT set "Allow Replication With Divergent and Corrupt Partner" to unblock it — that invites deleted objects back into the forest. Escalate to the senior AD resource with the delta, both DCs, and the tombstone lifetime value.
5. **Branch on the error:**
   1. **DNS/connectivity (8524 "DSA operation unable to proceed because of a DNS lookup failure", 1722 RPC unavailable)** — most common and most benign. Verify each DC points at working AD DNS (never public DNS first), the failing DC's CNAME (`<DSA-GUID>._msdcs.<forest>`) resolves, and required ports are open between sites. Pair with the internal-dns-server-issues playbook. Escalate when: firewall/VPN between sites is outside your control.
   2. **Security/Kerberos (8456/8457 source/destination "rejecting replication requests", -2146893022 target principal name incorrect, event 1388)** — often a secure-channel or time-skew issue, but 8456/8457 also appear *by design* after USN rollback. Check time sync across DCs first (Kerberos tolerates ~5 min). If time is fine, check for event 2103/2095 (USN rollback detected) before touching secure channels.
   3. **USN rollback (events 2095/2103, DSA "paused", 8456/8457 with a restored/reverted DC in history)** — a DC was brought back via snapshot or unsupported restore and its replication partners have seen "the future". The quarantine is intentional. Do NOT restart NTDS with the pause registry keys cleared, do NOT force-replicate. The standard remedy is demote-and-repromote (or supported restore) of the rolled-back DC — senior-resource work. Your job: identify it, freeze it, escalate with evidence.
   4. **Lingering objects (events 1388/1988, error 8606)** — one DC holds objects deleted elsewhere longer ago than the tombstone lifetime. Removal (`repadmin /removelingeringobjects`) must be run advisory-mode first and per naming context, against a known-clean reference DC. Escalate to the AD owner; this playbook does not drive the removal, it assembles the evidence: which DC, which NC, which reference DC is clean.
   5. **SYSVOL out of sync but AD replication clean** — DFSR problem (event 2213 dirty shutdown, 4012 content frozen past MaxOfflineTimeInDays), not AD replication. Authoritative vs non-authoritative DFSR sync is a decision with data-loss consequences (which DC's SYSVOL wins?) — escalate with the event IDs and which DC has the content the client considers current.
6. **Verify and note.** Success is `repadmin /replsummary` showing 0 fails and deltas in minutes, plus a test change (e.g., a group description) appearing on all DCs. Plain-text note: symptom, repadmin/dcdiag errors verbatim, branch, action or escalation, verification result and time.

## Guardrails

- Never run metadata cleanup, lingering-object removal, authoritative restores/SYSVOL syncs, or registry unblocks (divergent-partner, DSA-not-writable keys) from a ticket. This playbook diagnoses and escalates those; it does not execute them.
- Never "fix" a quarantined DC by forcing replication — the quarantine is protecting the forest.
- All repadmin/dcdiag commands are read-only guidance for the tech; no remote execution from here.
- Verify the forest's actual tombstone lifetime before reasoning about it; do not assume 180 days.
- Do not decode replication error numbers from memory when the decision hinges on them — confirm against Microsoft's docs (web_search).
- If only one DC exists, there is no replication problem — redirect to the actual symptom's playbook.
- Docs and Liongard coverage vary per tenant — note anything you could not check.
