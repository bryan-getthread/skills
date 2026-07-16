---
name: Browser SSO Loops
description: Break infinite sign-in loops — the page bounces between app and identity provider forever, or re-prompts on every visit — via the cookie-scope / trusted-zone / device-identity / per-browser isolation matrix.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# Browser SSO Loops

An SSO loop means the identity provider thinks it issued a session and the app disagrees — usually because the token or cookie that should carry the session is being blocked, scoped wrong, or the device identity the policy demands isn't visible from this browser. This playbook finds which leg of the round-trip drops the baton. It goes deeper on the auth loop than the general Browser Issues playbook; use that one for crashes, extensions, and single-site breakage.

## When to use

- "The page just keeps bouncing back to the sign-in screen" / redirect loop between app and login
- Sign-in works in one browser but loops in another on the same machine
- Loops that started for many users after a security or policy change
- "It asks me to sign in every single time" where it used to stay signed in

## Steps

1. **History first.** search_tickets for this client + the app + "sign in" symptoms. Loops that started tenant-wide on a date almost always trace to a Conditional Access or browser-policy change shipped that day.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the SSO landscape: identity provider, whether devices are Entra-joined/registered, browser standard, any documented Conditional Access device filters or trusted-site policies.
3. **Identify versions.** Browser and version, OS, the app being looped, and whether the machine is domain/Entra-joined. Loop behavior is browser-specific by design — device identity travels differently in each (Edge natively, Chrome via the tenant's documented mechanism for passing device identity, other browsers often not at all). Never assume; check what this tenant deployed.
4. **Evidence before theory.** Reproduce once and capture: the URL at each bounce (which side keeps redirecting), any error/correlation ID on the loop page, and — where tenant access exists — the Entra sign-in log rows for the attempts (pair with the M365 Sign-in Issues playbook; interrupted/failed entries name the policy). A loop with NO sign-in log entries means the request never reaches the IdP — look at local blocking, not policy.
5. **Isolate cheaply, in order.** (a) Private/incognito window: works there → cookie/extension state in the profile. (b) Clean browser profile: works there → corrupted profile state. (c) Different browser: works there → per-browser identity or settings. (d) Different user on the same machine, or same user on another machine → account-side vs device-side.
6. **Branch:**
   1. **Cookie scope / blocking** — private window fixes it, or the loop involves an embedded/iframed sign-in. Third-party-cookie blocking and strict tracking prevention break auth flows that hop domains; SameSite rules bite embedded logins. Fix: allow cookies for the identity provider's and app's auth domains specifically (get the exact domains from the vendor's docs via web_search — do not guess), ideally via managed browser policy so it sticks. Escalate when: the app only signs in inside an iframe and modern cookie rules break it — that is a vendor architecture issue; report it, work around with the top-level sign-in URL if one exists.
   2. **Trusted-zone / integrated auth** — on-prem apps using Windows integrated auth prompt or loop instead of silently signing in. The app's URL must be in the browser's intranet/trusted zone for credentials to flow automatically; a hostname change (new URL, HTTPS migration) silently drops it out. Fix via the centrally managed zone/policy list, not per-machine hand edits. Escalate when: the zone list is GPO/MDM-managed — route the addition to whoever owns that policy.
   3. **Device identity / CA filters** — loops only outside the standard browser, or sign-in log shows a device-compliance or device-filter failure. The policy demands a compliant/registered device claim this browser session cannot present: unregistered device, a browser that can't pass device identity, or a work profile signed into the wrong account. Fix: use the supported browser, sign the browser profile into the work account, or register the device — whatever the tenant's standard prescribes. Escalate when: the device shows non-compliant or unregistered and shouldn't be — that is an enrollment/compliance ticket (mobile-device-mdm or the tenant admin), not a browser setting.
   4. **Stale/corrupt session state** — one user, one profile, started randomly. Clear cookies and site data for the IdP and app domains only (not the nuclear clear-everything), sign out of all sessions via the IdP's portal, retry. Escalate when: it recurs on a clean profile — suspect an extension interfering with redirects (bisect per the Browser Issues playbook) or a broken SSO configuration on the app side.
   5. **Time skew** — everything loops or tokens are rejected as expired on one machine. Check the clock; minutes of skew invalidate tokens. Fix time sync; loop ends. Escalate when: skew keeps returning — endpoint time-source issue for the tech.
7. **Close the loop (the good kind).** Have the user sign in from a fresh browser session and confirm the session persists across a browser restart. Post a plain-text note: app, browser, branch, evidence (correlation IDs, which isolation step forked), fix or policy handoff, verification result.

## Guardrails

- Fix forward to the tenant's standard — never resolve a loop by weakening policy (disabling a CA device filter, blanket-allowing third-party cookies, turning off tracking protection globally). Policy exceptions belong to the tenant admin with the evidence attached.
- No script or remote execution — remediation is guidance for the user or tech; managed-policy changes route to the policy owner.
- Whitelist auth domains narrowly and from vendor documentation (web_search, cite) — do not invent domain lists.
- Do not have users clear all browser data as a first move — it destroys the evidence and everyone else's sessions; scope clearing to the involved domains.
- search_itglue / search_hudu may be absent for this tenant — fall back to search_knowledge_base and say what you could not check; without sign-in-log access, say the diagnosis is browser-side only and what the tenant admin should pull.
- Notes destined for a PSA sync: plain text, no markdown or emojis.
