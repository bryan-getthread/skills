---
name: Cross-Platform Translation
description: Translate a set of troubleshooting or setup steps between platforms — "these steps are for Windows, make them work for Mac" (or the reverse, or mobile) — faithfully, with explicit caveats where no equivalent exists.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# Cross-Platform Translation

A KB article, vendor doc, or prior-ticket resolution exists — for the other platform. This skill converts it step by step, preserving the *intent* of each step, verifying the target platform's real menu paths and commands, and flagging honestly where a step has no equivalent instead of papering over it.

## When to use

- "This KB is for Windows — the user is on a Mac" (or Mac → Windows)
- Translating desktop steps to the mobile app (iOS/Android) or web equivalent
- A vendor's fix is written for one OS and the fleet runs another
- Prepping one procedure into per-platform variants for the KB

## Steps

1. **Pin both ends — never assume.** Source platform/version the steps were written for, and the exact target: OS and version (macOS/Windows/iOS/Android release), and the app's version *on that platform* — cross-platform apps ship different features per platform, and a translation targeting the wrong version fails at step 2 of 9.
2. **History and docs first.** search_tickets for whether this translation already happened (a prior Mac resolution of the same issue beats a fresh translation), and search_knowledge_base / search_itglue / search_hudu for an existing platform-specific procedure at this client.
3. **Extract intent per step.** Before translating words, restate what each source step *accomplishes* (e.g. "flush the resolver cache", "remove the cached credential", "restart the app's background service") — the translation targets the intent, not the phrasing. Steps that are pure Windows mechanics (registry edits, services.msc) translate to their macOS mechanism (defaults/config profiles, launchctl) *only if the vendor supports that path on the target* — check.
4. **Verify every target-platform step against current documentation.** web_search the vendor's own docs for the target platform and the OS vendor's documentation for OS-level steps — menu names, settings locations, and commands are version-specific and drift constantly. Never invent a menu path or command from plausibility; every step in the output must come from a verified source or be marked unverified.
5. **Handle the non-equivalents honestly.** Three cases, each labeled in the output:
   - **Direct equivalent** — translated step, verified.
   - **Different mechanism, same intent** — the target reaches the goal another way (e.g. Windows "reinstall the driver" → macOS "remove and re-add the printer; drivers are system-managed"); explain the substitution in one line.
   - **No equivalent** — the step cannot be done on the target (feature absent, platform manages it automatically, or admin/MDM-only). Say so explicitly, state whether it matters for the fix, and name the alternative owner (e.g. "on macOS this is MDM-managed — route to the MDM admin") rather than skipping silently.
6. **Add the platform caveats that change outcomes.** Permissions prompts the target will raise (macOS TCC approvals, mobile OS permission dialogs), admin-rights differences, MDM/managed-device restrictions per the client's docs, and case-sensitivity/path differences. A translation that omits "macOS will ask for screen-recording permission here" produces the next ticket.
7. **Deliver in the same shape as the source.** Numbered steps mirroring the original's structure (so a tech can hold both side by side), each with its verification status, caveats inline at the step that triggers them, and a short header naming source platform, target platform/version, and what could not be translated. If the destination is the KB, pair with the KB-authoring skill for formatting.
8. **Close the loop.** Have the tech/user run the translated procedure and report which step, if any, diverged from reality — feed corrections back into the note (and the KB) so the translation converges instead of fossilizing an error.

## Guardrails

- Never fabricate menu paths, commands, settings names, or keyboard shortcuts on the target platform — verified source or explicitly marked unverified; an invented step is worse than a gap.
- Preserve intent, not cosmetics: a step whose purpose you cannot determine gets flagged and asked about, not guessed at and translated blind.
- No-equivalent steps are declared, never silently dropped — and if a dropped step is load-bearing for the fix, say the translation cannot fully substitute and what the alternative path is.
- Confirm target OS/app versions before translating; refuse "just make it for Mac" without knowing which macOS when steps are version-sensitive.
- Destructive steps in the source (deletes, resets, reinstalls) carry their warnings *and* the target platform's own data-loss implications, re-checked for the target — not copied assumptions.
- This skill translates procedures; it does not execute them — output is guidance for the tech, user, or KB.
