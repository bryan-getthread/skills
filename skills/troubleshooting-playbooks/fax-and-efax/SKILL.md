---
name: Fax & eFax
description: Work fax tickets — analog line dead, ATA-based fax failing or corrupting pages, eFax/cloud-fax not sending or receiving — across the line / ATA-settings / service-portal matrix, with healthcare-grade reliability expectations taken seriously.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# Fax & eFax

Fax is not dead — in healthcare, legal, and finance it is a compliance-anchored workflow with hard reliability expectations. Modern fax fails at one of three layers: the line itself (analog or its VoIP replacement), the ATA that bridges a physical fax machine onto VoIP, or the eFax service that replaced the machine entirely. This playbook identifies the layer before anyone re-sends the same document a sixth time.

## When to use

- "Faxes aren't sending / aren't being received" on a physical fax machine
- Faxes partially transmit, arrive garbled, or fail only on long documents (classic ATA symptoms)
- "Our eFax portal / fax-to-email stopped working" or inbound faxes stopped arriving
- A healthcare or legal client reporting fax failure — treat as workflow-critical, not legacy

## Steps

1. **History first.** search_tickets for this client + fax. Fax setups are bespoke; the prior ticket usually documents which architecture this client actually has, which is half the diagnosis.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the fax architecture: true analog line (POTS), fax-over-VoIP through an ATA, or a cloud eFax service — plus the carrier or eFax vendor, the fax number(s), and where they terminate.
3. **Set expectations by vertical.** For healthcare and similar clients, ask what the fax carries (referrals, orders, prescriptions) and whether there is an interim path (the eFax portal, a second line) — the workflow may need a bridge before the fix.
4. **Identify the architecture and versions.** Which of the three layers exists here, ATA make/model/firmware if present, and whether anything changed: carrier migration (POTS lines are being retired and migrated to VoIP widely — a "line" that used to work may not be analog anymore), phone-system replacement, or an eFax vendor change.
5. **Evidence before theory.** The failing direction (send, receive, both), the machine's error code or transmission report, whether failures are total or partial (fails mid-page, long documents only), and one concrete failed fax: destination number, time, page count.
6. **Branch:**
   1. **Analog line** — total failure on a POTS-connected machine. Guide on-site: plug a handset into the wall jack — dial tone or not is the fork. No dial tone → carrier ticket (line fault or, increasingly, a quiet POTS retirement); the desk's role is opening it with the carrier and tracking. Dial tone but no fax → the machine or its settings; test with a known-good fax number. Escalate when: line is dead — carrier only; nobody on the desk can fix copper.
   2. **ATA / fax-over-VoIP** — partial pages, garbled output, long-document failures, intermittent success. This is the signature of fax on a compressed voice path. Check the ATA's fax handling per its docs: T.38 fax relay vs G.711 pass-through, and error correction (ECM) settings on the machine; a mismatch between ATA config and what the carrier's platform supports is the usual cause. Baud step-down (9600) is a legitimate stabilizer. Escalate when: settings match vendor guidance and failures persist — the carrier must confirm T.38 support end-to-end; this is a carrier/VoIP-vendor conversation with the failed-call examples attached.
   3. **eFax service** — portal or fax-to-email failing. Check the vendor's status page first (web_search). Then the boring causes: inbound notification emails landing in spam/quarantine, a changed user password breaking the portal login, sender not on the account's authorized list, attachment format/size outside the vendor's limits. Escalate when: the vendor's platform is failing or a number was ported/misrouted — vendor case with fax IDs and timestamps from the portal's own log.
   4. **Receive-only failure** — sends fine, nothing arrives. Verify the number still routes correctly: a recent port, carrier migration, or forwarding change can silently send inbound faxes elsewhere. Test by sending to the number from a known source and tracing where it lands. Escalate when: routing is wrong at the carrier level — carrier ticket; number routing is theirs.
7. **Close the loop.** A successful test fax in the failing direction — with a transmission confirmation or portal delivery record — is the only verification that counts here; for compliance-sensitive clients, note the confirmation artifact in the ticket. Post a plain-text note: architecture, direction, branch, evidence, fix or carrier/vendor case reference, verification result.

## Guardrails

- No script or remote execution — remediation is guidance for on-site staff, ATA admin-panel guidance for the tech, or a carrier/vendor case.
- Never read, transcribe, or attach fax content into ticket notes — for healthcare and legal clients the content is regulated; reference metadata (time, page count, destination) only.
- Do not promise fax reliability on a voice-optimized VoIP path — if the client needs healthcare-grade reliability, say plainly that T.38 end-to-end or a cloud eFax service is the defensible architecture, and treat that as a recommendation, not an action taken.
- Carrier and number-routing faults are vendor-only — open the case, attach evidence, track it; do not improvise around it.
- Do not invent vendor limits, status-page results, or ATA settings — web_search the exact model/vendor and cite. search_itglue / search_hudu may be absent for this tenant; fall back to search_knowledge_base and say what you could not check.
- Notes destined for a PSA sync: plain text, no markdown or emojis.
