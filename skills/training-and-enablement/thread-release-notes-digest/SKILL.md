---
name: Thread Release Notes Digest
description: On demand ("what's new in Thread?"), pull the latest Thread changelog and product docs and post a plain-English summarized digest of new features and changes. Answers the commonly requested "keep the team current on Thread" workflow; runs manually on demand (Flows can't schedule it).
category: Training & Enablement
tools: [search_thread_docs, web_search]
---

# Thread Release Notes Digest

The team wants to know what changed in Thread without reading a raw changelog. On request, gather the latest release notes and product-doc updates and turn them into a short, skimmable digest aimed at the desk.

## When to use

- "What's new in Thread?" / "any Thread updates this month?"
- "Summarize the latest Thread release notes for the team"
- "Did Thread ship anything about <feature> recently?"
- Prepping a team huddle or enablement note on new Thread capabilities

## Steps

1. Gather source material. Use search_thread_docs for the changelog / release-notes / what's-new pages, and web_search for the public Thread changelog when docs don't cover the most recent items. Prefer official Thread sources over third-party write-ups.
2. Scope to the window the member asked for ("this month", "since <date>", "latest release"). If they didn't specify, default to the most recent release or the last ~30 days and say which window you used.
3. Group changes by theme the desk cares about — e.g. Magic AI / Agent, Voice, Flows & automation, Inbox/Messenger, admin & reporting — and within each, lead with the practical impact ("Auto-categorize now supports <X> — fewer manual re-tags") not the marketing line.
4. For each item give: one-line what-changed, who it affects (techs / admins / dispatchers), and a doc link if one exists. Keep it plain-English; translate feature names into "what this lets you do".
5. Close with a short "worth trying this week" callout — the one or two changes most relevant to this desk's known workflows — and offer to draft a team announcement or a huddle blurb from it.

## Guardrails

- No fabrication. Only report changes you actually found in Thread's changelog or docs; if the requested window has no notable releases, say so plainly rather than padding. Do not invent version numbers, dates, or features.
- Cite links only when they resolve to real Thread doc/changelog pages — never construct a plausible-looking URL.
- Distinguish shipped-and-available from announced/beta; don't present a roadmap teaser as a live feature.
- Keep it desk-relevant: summarize for the audience, don't paste the raw changelog.
- On-demand only. Thread Flows fire on ticket events and have no schedule/cron, so this cannot be a recurring flow — run it manually or from an external scheduler.
