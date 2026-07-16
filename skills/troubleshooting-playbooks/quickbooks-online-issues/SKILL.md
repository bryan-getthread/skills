---
name: QuickBooks Online Issues
description: Diagnose QuickBooks Online (the web/browser product) problems — distinguishing QBO from Desktop first, then bank-feed failures, multi-user access/role errors, and browser-layer issues (cache, extensions, third-party cookies) — from the actual error and the browser, not by assuming Desktop. For QB Desktop multi-user use quickbooks-desktop-multiuser.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# QuickBooks Online Issues

QuickBooks Online is a web app, so its failures are browser-and-account problems, not file-and-network problems — the opposite of Desktop. The first job of this playbook is to confirm you're actually on QBO (not Desktop), because the whole troubleshooting model differs. Then it works bank feeds, access/roles, and the browser layer.

## When to use

- A user reports QuickBooks web problems: pages won't load, buttons dead, "something went wrong"
- Bank feeds stopped importing, ask to reconnect, or duplicate/missing transactions from a feed
- A user can't access the company, has the wrong role/permissions, or "you've reached your user limit"
- Login/access works elsewhere but not in one browser or on one machine

## Steps

1. **Confirm QBO vs Desktop first.** search_itglue / search_hudu / search_knowledge_base and confirm with the user which product this is — QuickBooks **Online** (accessed at a qbo/intuit web URL in a browser) vs **Desktop** (an installed app, possibly hosted). If it's Desktop multi-user, stop and use quickbooks-desktop-multiuser. This single check prevents chasing file/network causes that don't exist on QBO. Also note the QBO subscription tier (user limits and features differ by plan) and how users sign in (Intuit account, SSO).
2. **History first.** search_tickets for this client + QuickBooks Online: a recent bank change (banks periodically force feed re-auth), a browser/OS update, a user added/removed, or a plan change. A feed that dropped across many clients at once may be an Intuit/bank-side outage, not this client.
3. **Get the exact error and isolate the browser before theorizing.** The verbatim error/screen, and immediately isolate the browser layer: does it work in a **private/incognito window** or a different browser? (That one test separates a browser/cache/extension problem from an account/data problem.) For feeds, which account, the feed error text, and when it last imported. Read the actual behaviour — don't assume.
4. **Branch:**
   1. **Browser-layer problems** (page won't render, buttons dead, endless spinner) — QBO is sensitive to cache, ad-block/privacy extensions, and **third-party cookie** blocking. If incognito works, the fix is clearing cache/cookies for the Intuit domains or disabling the offending extension in the normal profile. Confirm the browser is a currently-supported one (Intuit drops old browsers). This is the most common QBO ticket and needs no account changes.
   2. **Bank-feed failures** — a feed stopped or asks to reconnect: banks change their connection/security regularly, forcing a re-authentication of the feed; the user reconnects the bank within QBO. For duplicates/missing transactions, it's usually an overlap/gap from a re-link or a manual import — reconcile the date range rather than mass-deleting. Never bulk-delete or bulk-accept feed transactions to "clean up" — that's live financial data; correct the specific overlap. Escalate when: the bank itself blocks aggregation — that's between the client and their bank/Intuit.
   3. **Access / roles / user limit** — a user can't get in or lacks rights: check the user's role and status in QBO's Manage Users, and whether the plan's user limit is reached ("upgrade to add users" is a plan/cost decision for the client). Adding, removing, or changing a user's role is an access change — confirm with the client's QBO admin, and role changes affecting who can see payroll/banking are sensitive. Pair with access-request-handling / employee-offboarding.
   4. **Company data / "something went wrong" persisting across browsers** — if it fails in incognito and another browser too, it's account/data-side, not local: an Intuit service issue (check Intuit status), a corrupted session, or a genuine data problem that is Intuit's to resolve. Don't attempt local fixes for a server-side problem — set expectations and, if it's Intuit's, package the error for their support.
5. **Verify and note.** Success is the user completing the real action (page loads, feed imports the expected transactions, the user signs in with the right role). Plain-text note: confirmed QBO (not Desktop), the error, the incognito/other-browser result, branch, action or handoff, verification.

## Guardrails

- **QBO is live financial data in Intuit's cloud** — never bulk-delete/accept bank-feed transactions or change company data to "clean up"; correct the specific discrepancy and let the client's bookkeeper verify.
- No remote execution — browser and account steps are guidance for the user/tech; use get_ninjaone_device_link to reach a workstation when the RMM integration is enabled.
- User/role/plan changes are the client's access and spend decisions — confirm with the QBO admin before adding/removing users or recommending an upgrade; keep user identities out of PSA notes.
- Don't chase file/network/hosting causes on QBO — those belong to Desktop; confirm the product first (that's the whole point of step 1).
- Server-side/Intuit problems are Intuit's to fix — check Intuit's status, set honest expectations, and package errors for their support rather than improvising.
- Do not invent supported-browser lists, feed behaviours, or menu paths — web_search Intuit's current docs and cite (QBO changes frequently).
- Docs coverage varies per tenant — note what you couldn't check. Notes destined for a PSA sync: plain text, no markdown or emojis.
