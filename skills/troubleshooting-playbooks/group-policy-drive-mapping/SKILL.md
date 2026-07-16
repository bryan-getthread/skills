---
name: Group Policy Drive Mapping
description: Diagnose GP-mapped-drive failures specifically — drives not appearing for some users/machines — through the Group Policy Preferences drive-map item's item-level targeting, security filtering, credential/cached-context timing, and processing order, from gpresult evidence not guesswork. Broader GP problems belong to group-policy-troubleshooting.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# Group Policy Drive Mapping

Mapped-drive-via-GPP failures are almost always one of four things: item-level targeting that excludes the user, security filtering / GPO scope that doesn't apply the policy, a credential/timing problem (the drive maps before the network or credentials are ready), or the drive-map action/order itself. This playbook reads gpresult and the item's targeting before touching the share — broader Group Policy problems (not drive-specific) belong to group-policy-troubleshooting.

## When to use

- A mapped drive is missing for some users or some machines but present for others
- New users, new laptops, or a specific OU don't get the drive; VPN/remote users don't get it
- A drive maps to the wrong path, maps then disappears, or asks for credentials
- Drive mapping worked until a GPO/OU/security-group change, then stopped for a group

## Steps

1. **Design first.** search_itglue / search_hudu / search_knowledge_base for how drives are delivered: which GPO(s) map drives, whether via Group Policy **Preferences** drive-map items (the modern way — with item-level targeting) vs a logon script, the OU the GPO is linked to, its security filtering and any WMI filter, and the target share's server/path and permissions. Establish GPP-item vs script — the whole diagnosis differs. If a Liongard AD inspector runs, corroborate GPO/OU/group state via liongard_launchpoint / liongard_metric and note dataprint age.
2. **History first.** search_tickets for this client + drive/GPO: a recent OU move, a security-group membership change, a GPO edit, a file-server rename/IP change, or a shift to laptops/VPN. A group losing a drive after a date usually traces to filtering/targeting or the OU.
3. **Get gpresult before theorizing.** From an affected machine/user, `gpresult /h` (or /r) shows whether the drive-map GPO **applied** and, if filtered out, **why** (denied by security filtering, WMI filter, or not in scope). For GPP items also check the item's own **item-level targeting** (it can silently skip a user who doesn't match). Read what actually applied — don't assume the GPO reached the user.
4. **Branch:**
   1. **Item-level targeting excludes the user** — the GPO applies but the specific drive-map item's targeting (group membership, OU, site, computer name pattern) doesn't match this user/machine, so that one drive silently doesn't map. Read the item's targeting against the user's actual group/OU. This is the most common "some people get it, some don't" cause — fix the targeting or the user's group membership, whichever is wrong.
   2. **Security filtering / scope** — the whole GPO didn't apply: gpresult shows it denied or out of scope. Since the Authenticated-Users-read change, a GPO security-filtered to a group still needs the computer/user to have *read* on the GPO; a filtering or OU-link gap drops it entirely. Confirm the user/computer is in scope and passes filtering (and any WMI filter). Fix the scope, not the share.
   3. **Credential / timing / network-not-ready** — the drive maps then shows disconnected/red-X, or prompts for credentials: often the mapping ran before the network or credentials were available (fast logon, VPN-before-logon absent, laptop off-domain-network), or the user lacks share/NTFS permission so it maps but won't open. "Always wait for network at startup" helps timing; for VPN/remote, GP may not process at all without a domain connection. Distinguish "didn't map" (targeting/scope) from "mapped but won't connect" (permissions/timing).
   4. **Action / order / wrong path** — the drive-map item's action (Create vs Replace vs Update), drive letter collision, or path is wrong: a Replace re-creates every logon (slower, resets), a letter already used by another mapping wins unpredictably, or the UNC path points at a stale server name. Read the item's action and path; a server rename breaks the path for everyone.
5. **Verify and note.** Success is an affected user logging on and getting the correct drive, connected and openable — confirmed by gpresult showing the GPO/item applied. Plain-text note: GPP-item vs script, the gpresult finding (applied / why-not), branch, action or handoff, verification.

## Guardrails

- No remote execution — gpresult, GPMC, and share-permission checks are guidance for a tech with the right access; use get_ninjaone_device_link to reach a workstation when the RMM integration is enabled.
- Diagnose from gpresult, not assumption — "the GPO looks right" is not evidence it applied; the RSOP/gpresult output is.
- Don't loosen share/NTFS permissions to Everyone or broaden a GPO's scope to "make the drive appear" — fix the specific targeting/membership/permission gap.
- Credentials never go in the drive-map item or the ticket — a drive prompting for credentials is a permissions/timing problem to fix, not a password to store.
- Changing a GPP action (e.g. to Replace) or drive letter affects everyone the GPO targets — treat as a change and know the blast radius.
- Do not invent gpresult flags, GPP targeting behaviours, or the Authenticated-Users read requirement specifics — web_search Microsoft's current docs and cite.
- Docs/Liongard coverage varies per tenant — note what you couldn't check and dataprint age. Notes destined for a PSA sync: plain text, no markdown or emojis.
