---
name: Endpoint Encryption Audit
description: Audit disk-encryption coverage across a client's endpoints — BitLocker on Windows, FileVault on Macs — flag unencrypted devices, and verify recovery keys are actually escrowed somewhere retrievable. Use for "are all their laptops encrypted", compliance evidence requests, or after a lost laptop raises the question.
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, liongard_launchpoint, liongard_metric, liongard_query, search_itglue, search_hudu, search_tickets, add_ticket_note, create_ticket]
---

# Endpoint Encryption Audit

Two questions, both mandatory: is every endpoint's disk encrypted, and if a device died tomorrow, could the desk retrieve its recovery key? Coverage without escrow is a lockout waiting to happen; escrow without coverage is a breach waiting to happen.

## When to use

- "Are all of <client>'s laptops encrypted?" — compliance, insurance, or audit evidence.
- A device was lost or stolen and the encrypted-or-not answer determines breach severity.
- Before enforcing an encryption policy: what is the current baseline?
- Spot-check: "do we actually have the BitLocker keys for these machines?"

## Steps

1. Build the endpoint population from the RMM (`search_ninjaone_devices`, verify workstation/laptop class in device details) — this is the denominator. If listings may have hit a cap, the denominator is "at least N" and the audit says so.
2. Gather encryption state per device, best source first: RMM device details (`get_ninjaone_device`) where the platform exposes encryption/volume status; Liongard's Windows and Microsoft 365/Entra inspectors (`liongard_launchpoint` to confirm the inspector exists and last ran successfully, then `liongard_metric` / `liongard_query` for BitLocker status and escrowed-key presence — state the dataprint age); documentation (`search_itglue` / `search_hudu`) as a last resort with its staleness named. Devices with no encryption evidence from any source go in an "unknown" bucket — unknown is not unencrypted, and it is definitely not encrypted.
3. Bucket the fleet: encrypted with key escrowed, encrypted with NO escrow evidence (the sleeper risk — one dead motherboard or firmware update from permanent data loss), not encrypted, and unknown. Macs: FileVault status typically needs MDM data (via the Liongard/Intune path or an MDM export) — if no Mac source exists, the Mac fleet is "unknown", stated plainly.
4. Escrow verification — evidence, not assumption: identify where keys are supposed to live (Entra ID/AD, the MDM, the documentation platform's flagged fields, per client policy from docs) and verify presence for the audited devices via the available reads. Presence of a key object is the pass bar this skill can reach; confirming a key actually unlocks its volume is a hands-on test — recommend it for a sample, never claim it. Never copy actual recovery keys into notes, tickets, or the report — existence and location only.
5. Flag and route: unencrypted devices get a remediation ticket (`create_ticket`) — enabling encryption is a policy/hands-on action this skill cannot perform, and on older hardware without TPM it needs per-device assessment; encrypted-without-escrow devices get a "capture key before anything else" ticket flagged urgent-quiet (no reboots, no firmware updates on that device until the key is escrowed); unknowns get a verification pass. Check `search_tickets` for an existing encryption project to attach to before opening duplicates.
6. Output: coverage summary (encrypted+escrowed / encrypted-no-escrow / unencrypted / unknown, with percentages against the stated denominator), per-bucket device lists, evidence source and freshness per claim, and tickets opened. Post the plain-text summary via `add_ticket_note`. For compliance requests, state clearly this is a point-in-time technical audit, not a compliance certification.

## Guardrails

- Recovery keys are never reproduced in any output, note, or ticket — location and existence only. A key pasted into a ticket is itself a security incident.
- "Unknown" is its own honest bucket — never fold unknowns into either encrypted or unencrypted to make the percentage cleaner.
- Encrypted-without-escrow devices must not be rebooted, firmware-updated, or have TPM changes made until the key is captured — say this on every such finding.
- Every encryption claim carries its evidence source and age; inspector data is used only when the launchpoint shows a successful recent run.
- This skill enables nothing and rotates nothing — remediation is policy work (MDM/GPO) handed off with a per-device list.
- If neither RMM nor Liongard is available, the audit degrades to documentation + ticket history and is labeled "unverified — evidence-gathering pass required".
- Notes are plain text — no markdown, no emojis.

## See also

- `intune-rmm-reconciliation` — the enrollment gaps that usually explain missing encryption policy.
- `win11-readiness-assessment` — TPM presence data useful for the unencrypted-old-hardware bucket.
- `bitlocker-key-retrieval` (m365-administration) — the identity-gated process for retrieving an escrowed key when a device actually needs it.
