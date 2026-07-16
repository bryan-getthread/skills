---
name: Asset Warranty Report
description: Someone wants a warranty and end-of-life rollup of a client's fleet — what is out of warranty, expiring soon, or past hardware EOL.
category: Reporting & Analytics
tools: [search_ninjaone_devices, get_ninjaone_device, list_ninjaone_organizations, liongard_launchpoint, liongard_metric, search_clients]
---

# Asset Warranty Report

Roll up a client's device fleet by warranty and end-of-life status — in-warranty, expiring soon, expired, and past-EOL hardware — so refresh planning and renewal conversations run on data instead of guesswork. A budgeting and risk artifact, not a remediation action.

## When to use

- "What's the warranty status of <client>'s fleet?" / "what's aging out?"
- QBR or budget-season prep — building a hardware refresh forecast.
- A recurring-hardware-failure pattern prompts "how old is this fleet really?"

## Steps

1. Confirm the client and the fleet scope (search_clients), the reporting thresholds ("expiring soon" = next N days), and which system holds warranty/purchase data for this tenant.
2. Pull the fleet inventory. With NinjaOne enabled: list_ninjaone_organizations to resolve the org, then search_ninjaone_devices and get_ninjaone_device for warranty/purchase and hardware model fields. Where warranty lives in a config platform, use Liongard: liongard_launchpoint to confirm the relevant inspector exists and last ran, then liongard_metric to read warranty/purchase-date dataprints. State the source and the dataprint age.
3. Bucket devices: in-warranty, expiring within the threshold, expired, and — separately — past hardware/OS end-of-life (a supported-warranty device can still be EOL hardware). Verify EOL against the vendor's current published lifecycle, not memory.
4. Summarize the fleet: counts and share per bucket, broken out by device type/model and by site where useful, plus a short list of the highest-risk devices (expired + EOL + business-critical role).
5. Translate to planning: rough refresh cohorts by quarter and any concentration risk (a whole site aging out together). Frame as forecast inputs, not commitments.
6. Output: the bucket summary, the by-model/by-site breakdown, the high-risk device list, the refresh-cohort view, and a methodology note (data source, dataprint/inventory freshness, thresholds, EOL sources, coverage gaps).

## Guardrails

- Coverage honesty: report how many devices lack warranty/purchase data and exclude them from percentages rather than guessing — an incomplete inventory silently understates risk.
- State data-source freshness (NinjaOne last-seen, Liongard dataprint age); stale inventory produces a confidently wrong report.
- Verify hardware/OS EOL dates against the vendor's current lifecycle docs; do not assert EOL from memory.
- Degrade gracefully: if neither NinjaOne nor a Liongard warranty inspector is present, say the fleet data is unavailable and fall back to any documented asset list — do not fabricate warranty dates.
- This is a read/planning report only — no device changes, no reboots, no maintenance toggles. Any remediation is a separate, explicitly-authorized action.
- Do not invent serial numbers, warranty dates, or model EOL status.
