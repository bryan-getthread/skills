---
name: Network Share Slowness
description: Diagnose slow file shares — copies crawl, folders take forever to open, one office is fine and another isn't — by laddering SMB version negotiation, signing/encryption overhead, antivirus filter drivers, DFS referrals, and the physical path.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# Network Share Slowness

"The share is slow" has no fix until it has a number and a layer. The ladder: measure it, confirm what SMB dialect actually negotiated, then walk the overhead suspects (signing/encryption, antivirus filter drivers, DFS referral choice) before blaming the network — and the network last, with evidence. For access-denied and permission tickets, use the File Share Permissions playbook; this one is purely about speed.

## When to use

- "Opening folders on the shared drive takes 30 seconds" / file copies crawl
- Slowness that appeared after server hardening, a security-tool rollout, or a migration
- One site or one VLAN slow against a share that is fast elsewhere
- "Excel files on the share take forever to open" (often not the network at all)

## Steps

1. **History first.** search_tickets for this client + the share/server + slowness. A start date is gold: slowness that began the week signing was enforced, AV was replaced, or the file server migrated is already half-diagnosed.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the file-service layout: server(s), DFS namespaces and their targets per site, SMB hardening decisions on record (signing/encryption requirements), AV product on server and endpoints.
3. **Measure before theorizing.** Get a number: a timed copy of a known-size file (both directions — read and write can differ), and the same test from a second machine and, if possible, a second site. "Slow" becomes MB/s, and the comparisons scope it: one user, one machine, one site, or everyone.
4. **Identify versions.** Client OS and server OS (they bound the SMB dialect), and what actually negotiated: guide the tech to check the active connection's dialect (on Windows, Get-SmbConnection alongside an open file) rather than assuming. A modern client stuck on an old dialect — or worse, SMB1 to a legacy device — explains a lot by itself.
5. **Evidence before theory.** The measured rates, the negotiated dialect, whether latency is in bulk transfer (copies slow) or metadata (folder listings and file-open slow, copies fine) — those point at different branches.
6. **Branch:**
   1. **SMB negotiation** — old dialect negotiated, or dialect varies by client. Find why: legacy OS, a NAS capped at an old dialect, or policy pinning. The fix is bringing the endpoint or storage device up to a modern dialect, never downgrading the healthy side. Escalate when: the storage device can't speak a modern dialect — replacement/firmware conversation with the client; do not re-enable SMB1 anywhere without recorded security-owner sign-off.
   2. **Signing / encryption overhead** — slowness began when SMB signing or encryption was enforced, and rates dropped broadly but uniformly. This is a real cost on weak CPUs and old NICs. The honest framing: the security control is likely staying; the remediation is capacity (server CPU, NIC offload support, updated drivers) or accepting the new baseline. Escalate when: someone proposes disabling signing/encryption to restore speed — that is a security decision for the security owner, in writing, not a performance tweak.
   3. **Antivirus / filter drivers** — metadata operations crawl, or slowness tracks an AV rollout. On-access scanning on the server share path or aggressive endpoint filtering multiplies every file-open. Diagnose by timestamp correlation and vendor guidance — via the vendor's documented exclusion/tuning process routed through the security owner. Never test by disabling AV. Escalate when: exclusions are warranted — security owner approves the specific paths/processes; the desk proposes, doesn't apply.
   4. **DFS referrals** — one site slow, share is DFS-hosted. The client may be referred to a target in the wrong site (WAN instead of local). Guide: check which target the client actually resolved (dfsutil / the client's referral cache) against the site's intended target, and verify site/subnet definitions are current — a new subnet missing from sites-and-services sends a whole office across the WAN. Escalate when: referral costs or site topology need redesign — the infrastructure owner, with the referral evidence.
   5. **Physical/network path** — bulk rates low for a subset, everything else ruled out. Classic culprits: a NIC negotiated to 100Mb half-duplex, Wi-Fi vs wired (test the same copy wired), a saturated uplink or WAN link at the slow site. Escalate when: the bottleneck is a switch port, uplink, or circuit — network resource, with the measured rates per location attached.
   6. **False network positive** — "the share is slow" but only certain files: huge Excel workbooks with cross-file links, databases-on-a-share (Access and friends), or an app scanning the whole directory. The share is the victim, not the cause. Route to the app/data owner with the measurements proving transfer rates are healthy.
7. **Close the loop.** Re-run the same timed copy and metadata test that established the baseline; resolution means the number moved, not that it "feels better." Post a plain-text note: baseline vs after, negotiated dialect, branch, change made or handoff, verification numbers.

## Guardrails

- Never trade security for speed unilaterally: SMB1 re-enable, signing/encryption disable, and AV exclusions are all security-owner decisions — propose with evidence, don't apply.
- No script or remote execution — measurement commands and checks are guidance for the tech; use the RMM deep link for hands-on server work when that integration is enabled.
- No number, no diagnosis: refuse to conclude from "feels slow" — always establish the measured baseline first, and re-measure to verify.
- Do not invent dialect capabilities of NAS/storage devices — web_search the exact model and cite.
- search_itglue / search_hudu may be absent for this tenant — fall back to search_knowledge_base and say what you could not check.
- Notes destined for a PSA sync: plain text, no markdown or emojis.
