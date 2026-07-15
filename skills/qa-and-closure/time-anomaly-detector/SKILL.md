---
name: Time Anomaly Detector
description: Benchmark logged time against per-issue-type expectations and flag outliers — suspicious overtime on simple issues, underlogging on complex ones, and zero-time closures — without treating unverifiable time as a failure.
category: QA & Closure
tools: [search_tickets, search_members]
---

# Time Anomaly Detector

Finds tickets where the time entries do not fit the work: three hours on a password reset, six minutes on a server migration, or closures with no time at all. Produces a review list for a lead — it detects anomalies, it does not accuse.

## When to use

- "Check this week's time entries for anomalies" / "who is overlogging / underlogging?"
- Pre-billing review: catching time that will not survive a client's scrutiny.
- A lead investigating why utilization numbers look off.
- Grading one specific ticket's time against what the notes describe.

## Steps

1. Establish the benchmarks. Use desk-configured thresholds per issue type where the user provides them; otherwise derive rough expected ranges from historical closed tickets of the same type/subtype via `search_tickets` (e.g. median and typical spread for "password reset" on that board). State which basis was used.
2. Pull the review window's closed tickets with time entries via `search_tickets`, split per board; disclose result caps.
3. For each ticket, compare total logged time to the benchmark for its issue type and to what the notes actually describe. Flag three anomaly classes:
   - **Overtime**: logged time far above the expected range with notes that do not document why (no scope growth, no vendor call, no on-site).
   - **Underlogging**: logged time far below what the documented work plausibly took, or a multi-touch thread with a single tiny entry.
   - **Zero-time closure**: closed with no time entry at all.
4. For each flag, check the thread for a legitimate explanation first — a documented vendor escalation, parts wait, or scope note absorbs an outlier. A ticket with a documented reason is not an anomaly; drop it.
5. Classify each remaining flag's confidence: CLEAR (notes contradict the time) vs UNVERIFIABLE (nothing in the thread proves the time either way). **Unverifiable is not a FAIL** — time can be legitimately spent off-thread (travel, phone, thinking). Mark unverifiable items "for conversation, not correction."
6. Output the review list grouped by anomaly class, then by technician (`search_members` for names): ticket, issue type, expected range, logged time, confidence, and the one-line reason it was flagged. End with aggregate per-tech tendencies (e.g. "consistently logs 0.25h regardless of work") stated neutrally.

## Guardrails

- Never present an anomaly as misconduct — the output is a review list for a human lead, and every line distinguishes CLEAR from UNVERIFIABLE.
- Unverifiable ≠ FAIL: absence of thread evidence for time spent is a documentation gap at most, never proof of padding.
- Do not modify or delete time entries; this skill is read-only on time. Corrections are the tech's and lead's job.
- Benchmarks derived from history are approximations — label them as such and never enforce them as policy the desk has not set.
- Per-tech aggregates are counts and medians, not character judgments; keep wording neutral and localizable.
- Disclose result caps; a time audit on a truncated sample must say so.
