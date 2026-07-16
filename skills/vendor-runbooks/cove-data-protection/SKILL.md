---
name: Cove Data Protection Alerts
description: An N-able Cove Data Protection backup ticket arrived — classify the failure family, verify recoverability rather than assuming it, and keep archive/retention sessions straight from standard sessions. Verify against N-able's current Cove documentation.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# Cove Data Protection Alerts

Vendor specialization of backup-failure-triage for N-able Cove Data Protection (formerly N-able Backup). backup-failure-triage owns the classify → recurrence → fix-here-vs-escalate canon; this skill adds Cove's cloud-first architecture, its failure families, and the discipline of verifying recoverability instead of asserting it. Cove's feature names and console (the Backup Manager / Cove dashboard) evolve — verify against N-able's current documentation.

## When to use

- A Cove backup failure, "backup did not run", or session-error alert lands as a ticket
- Someone asks whether a device or data set is actually recoverable from Cove
- A retention/archive question or storage/LocalSpeedVault issue needs a decision

## Steps

1. Classify the failure family from the alert text plus device state, per backup-failure-triage, using Cove's specifics:
   - Device offline / Backup Manager not checking in at schedule time → availability problem, not a backup fault.
   - Session interrupted / connection to cloud storage lost → Cove is cloud-first; network path and the device's ability to reach the storage node matter. Distinguish a one-off interrupted session (Cove resumes/retries) from persistent failures.
   - LocalSpeedVault (LSV) out of sync / unavailable → LSV is the optional local copy that also seeds the cloud; an LSV fault degrades local restore speed but the cloud copy may still be current — state which copy is affected.
   - Credential / data-source auth failures → application-aware sources (Exchange, SQL, M365, Hyper-V/VMware) with rotated service accounts or expired tokens.
   - Data-source-specific errors (VSS/writer, file-lock, mailbox) → OS/application-level; check for pending reboot or recent patch in the device history.
2. Route to the correct client per backup-failure-triage; Cove alerts often arrive on a shared backup-monitoring mailbox. Low routing confidence → no reassignment, flag for a human.
3. Recurrence check via search_tickets (same device, same failure class, 30–90 days). One failure with a successful session after it is noise; repeated same-class failures are a problem ticket, not a close.
4. Verify recoverability — do not assert it. Cove's Recovery Testing (automated restore-to-standby/verification, where the client has it configured) is the evidence that a restore actually works; without a passed recovery test, the strongest honest claim is the last successful session date per data source. Never tell a client "your data is safe" — report the last known good session and whether recovery testing is passing.
5. Archive vs standard sessions — keep them straight: Cove archiving sessions retain point-in-time copies on a separate schedule/retention from continuous sessions. A restore "as of <date>" depends on whether an archive covers that date; confirm the archive schedule and retention before promising a historical restore point. Retention/archive changes are billing- and compliance-relevant — route commercial changes to account management.
6. Decide the path: handle-here for offline-at-schedule, obvious transient session interruptions, pending-reboot writer errors; escalate to N-able support for repeated same-class failures after local remediation, integrity/corruption errors, or failures spanning many clients at once (possible storage-node/product-side incident — check N-able's status page first). Document the failure classification, evidence, recurrence verdict, last known good session per source, and the recommendation.

## Guardrails

- Never state data is safe or that a restore will succeed — last successful session on record (and a passing recovery test where present) is the strongest claim; restore verification is a human task.
- Cloud copy vs LocalSpeedVault copy are different exposures — always say which one the failure affects.
- A restore "as of <date>" is bounded by archive schedule and retention — confirm a covering restore point exists before promising anything.
- Do not clear or reset backup alerts during triage; the alert is the evidence trail.
- Repeated same-class failures become a problem ticket, never a one-off close.
- Console actions (restores, recovery-test runs, retention changes) are technician steps the agent directs and records; the agent has no Cove console access.
- Degradation: without documentation access (search_itglue), the client's Cove data-source scope and retention design may be unknown — say so rather than assuming full coverage.
