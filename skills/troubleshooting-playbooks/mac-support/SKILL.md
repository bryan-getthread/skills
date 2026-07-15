---
name: Mac Support
description: The Windows tech's ladder for Mac tickets — keychain password prompts, MDM enrollment, app permissions (TCC), FileVault — mapping Mac-specific causes so the tech doesn't apply Windows reflexes to macOS.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# Mac Support

Most MSP desks are Windows-native, and Mac tickets stall because the tech doesn't know where macOS keeps the equivalent problem. This playbook maps the four issues that generate most Mac tickets — keychain, MDM enrollment, TCC permissions, FileVault — with the Mac-correct steps and the places Windows instincts mislead.

## When to use

- Mac user gets endless password prompts after a password change ("keychain" anything)
- A Mac won't enroll in (or fell out of) MDM; profiles not applying
- An app can't see the screen/camera/mic/files it needs (screen shares show black, mics "don't work")
- FileVault login/recovery issues, or a Mac ticket where the tech says "these steps are for Windows"

## Steps

1. **History first.** search_tickets for this user/device and for Mac tickets at this client — desks with few Macs often have one tech who solved it before; their note is gold.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the client's Mac standard: MDM product, whether Macs are supervised (ADE/Apple Business Manager) or user-enrolled, identity integration (Platform SSO / directory), FileVault key escrow location.
3. **Identify versions — never assume.** Exact macOS version and the app version involved; macOS behavior (especially permissions and MDM) changes materially between versions, so verify steps against the running version with web_search rather than reciting steps that were true two versions ago.
4. **Branch:**
   1. **Keychain** — the signature: password changed (via IdP, another machine, or a directory), now the Mac prompts repeatedly for the *old* password ("keychain login" prompts) or apps re-ask credentials endlessly. The login keychain is still encrypted with the old password. Fix path: if the user remembers the old password → update the login keychain password to match (Keychain Access / prompted flow). If not → a new login keychain (macOS offers this at the prompt) — which discards saved passwords/certs in the old one: say so before choosing it, and check what's in there (Wi-Fi, mail, app creds) so re-entry is planned, not discovered. Never "fix" by deleting keychain files from disk manually.
   2. **MDM enrollment** — won't enroll, or profiles stopped applying. Determine intended state from docs (supervised/ADE vs manual). Checks: is the device serial assigned in Apple Business Manager to the right MDM server; does the device show in the MDM console (stale/duplicate record blocks re-enrollment — remove the stale record per the MDM's procedure); network reachability to Apple's enrollment endpoints (filtered networks break this — verify Apple's published ports/hosts). Re-enrollment of an ADE device usually needs an erase — that is a data-destroying step: gate it behind backup confirmation. Escalate when: ABM/MDM tenancy issues (tokens expired, server mismatches) — the MDM admin owns those.
   3. **Permissions / TCC** — app "can't" use camera, mic, screen recording, or files, typically after an update or on a new machine; remote-support tools showing a black screen is the classic. Fix: System Settings → Privacy & Security → the specific permission → enable for the app, then *fully quit and relaunch the app*. On managed Macs, some permissions (notably screen recording) can be pre-approved only partially by MDM (PPPC profiles) — if the toggle is greyed out or resets, it's MDM-managed: route to the MDM config rather than fighting the endpoint.
   4. **FileVault** — user can't get past disk unlock, or recovery is needed. Never guess: check the documented escrow (MDM-escrowed personal recovery key, or the client's key record) before attempting anything. A password change made elsewhere may not reach the FileVault unlock screen until the account's local password syncs — known behavior; verify the current guidance for the macOS version. If no escrowed key exists and no user password works, be honest: data recovery is effectively closed; the path is erase-and-restore from backup. Escalate when: no key escrow is configured fleet-wide — flag that as a client-level risk finding beyond this ticket.
5. **Cross-platform translation.** When the resolution exists as Windows steps (KB, prior ticket), translate faithfully rather than approximating — pair with the cross-platform-translation playbook; never invent macOS menu paths.
6. **Verify and note.** Re-test the exact failing action (screen share visible, no prompt storm across a reboot, profile applied). Plain-text note: macOS version, branch, cause, action, anything discarded (old keychain) and what the user must re-enter, verification.

## Guardrails

- Confirm the macOS version before giving stepwise UI instructions, and verify version-specific steps with web_search — settings locations move between releases.
- New-keychain and device-erase choices destroy stored data — both are gated behind explicit user/tech acknowledgment of what is lost and what must be backed up or re-entered.
- FileVault recovery keys and any credentials never go in ticket notes or email — secure channel per client practice only.
- MDM-managed settings are fixed in the MDM, not fought on the endpoint — route configuration changes to the MDM owner.
- No remote execution — all steps are guidance for the tech or user.
- Docs tools vary per tenant — note what you could not check (especially key-escrow location).
