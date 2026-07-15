---
name: Anti-Spam Policy Tuning
description: Adjust Exchange Online Protection / Defender anti-spam policies from evidence, never from complaints alone — scoped overrides over broad allow-lists, time-limited exceptions, and the verdict data to justify each change. Use when a client says "too much spam getting through" or "legitimate mail keeps getting filtered."
category: M365 Administration
tools: [search_tickets, search_clients, search_knowledge_base, search_itglue, add_ticket_note, send_approval, log_time_entry, web_search]
---

# Anti-Spam Policy Tuning

Changes filter behavior only where evidence shows the filter is wrong, with
the narrowest override that fixes it — because every allow-list entry is a
hole an attacker can drive through.

## When to use

- "Spam is flooding in" or "good mail keeps landing in junk/quarantine"
  as a pattern (single-message release requests are
  quarantine-release-request; single-delivery diagnosis is
  mail-flow-delivery).
- "Whitelist this vendor's domain" requests.
- Reviewing a tenant's anti-spam posture after repeated incidents.

## Steps

1. Demand evidence before touching policy. Collect for the complained-about
   pattern: message traces with filter verdicts (mail-trace-investigation),
   headers from example messages (email-header-analysis reads X-Forefront-
   Antispam-Report: SCL, BCL, SFV codes), and quarantine records. The header
   says WHY the filter acted — tune against that reason, not the complaint.
2. Diagnose the actual failure mode:
   - Good mail marked spam because the sender fails SPF/DKIM/DMARC → the fix
     is on the SENDER's side (their DNS), not an allow-list. Route to
     dmarc-spf-dkim-setup guidance and tell the client why an override would
     mask a real authentication failure.
   - Bulk/graymail (newsletters) filtered → adjust the bulk complaint level
     (BCL) threshold or user-level safe senders, not org policy.
   - Genuine false positive verdicts from filtering heuristics → a scoped
     override is legitimate (step 3).
   - Spam getting through → check whether the policy is at Microsoft's
     Standard/Strict preset baseline first; recommend presets or tightened
     thresholds before bespoke tinkering.
3. When an override is justified, use the narrowest instrument, in this
   order of preference:
   - Tenant Allow/Block List entry for the specific sender or spoofed pair —
     time-limited where the portal supports it, and calendar a review date
     regardless.
   - Anti-spam policy exception scoped to the affected recipients only.
   - NEVER "allowed sender domains" containing the client's own domain, any
     free-mail domain (gmail.com, outlook.com), or a broad vendor domain —
     domain-level allow entries bypass filtering and are the canonical
     spoofing hole. Refuse and explain; offer the scoped alternative.
4. Any change that alters what lands in user inboxes is user-visible: get the
   client's approval (send_approval) with the evidence summary and the scope
   of the override stated plainly.
5. Prepare execution for the tech: Defender portal (Policies & rules >
   Threat policies > Anti-spam) or PowerShell (`Set-HostedContentFilterPolicy`,
   `New-TenantAllowBlockListItems` — verify against current module versions).
6. Verify via evidence over a defined window: re-trace the affected pattern
   after the change; verdicts should flip for the target mail and ONLY the
   target mail. Check the spam catch-rate hasn't visibly degraded
   (mail-flow-reports covers the ongoing watch).
7. Post a plain-text note: the evidence (verdict codes, trace refs), the
   diagnosis, exact change made and its scope, review/expiry date for
   overrides, approver, and rollback (remove entry / restore prior threshold
   values captured before the change). Log time.

## Guardrails

- No policy change without verdict evidence attached to the ticket.
  "The client is annoyed" is a reason to investigate, not to allow-list.
- Broad allow-lists are refused, with the scoped alternative offered —
  especially the client's own domain and free-mail domains.
- Sender-side authentication failures are fixed at the sender; overriding
  them locally hides breakage and invites spoofing.
- Every override carries a review date; allow-lists only ever grow unless
  someone owns pruning them.
- Capture prior policy values before changing them — that is the rollback.
