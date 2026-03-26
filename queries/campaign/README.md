# Campaign Queries

These queries were created specifically for the Huntress-reported Railway / Microsoft 365 token replay and device code phishing campaign:

- https://www.huntress.com/blog/railway-paas-m365-token-replay-campaign

They are the campaign-specific hunts in this repo. Use these first when you are investigating activity that may match the lure themes, infrastructure, click patterns, mailbox abuse, or follow-on SaaS access described in that reporting.

## How This Folder Is Organized

- `clicks/`
  - Hunts user clicks and browser-driven visits to the reported lure and redirector infrastructure.
  - Best fit when you have Defender for Office 365 Safe Links telemetry, though the click queries can fall back to Defender for Endpoint browser network telemetry.
- `email/`
  - Hunts delivered mail and correlates suspicious campaign URLs with message delivery.
  - These queries depend on Defender for Office 365 mail telemetry to return mail results.
- `cloud-apps/`
  - Hunts mailbox actions, inbox rule abuse, consent activity, and follow-on SaaS access after initial compromise.
  - These queries depend on `CloudAppEvents` coverage, typically via Defender for Cloud Apps / Defender XDR integrations.

## Suggested Order

1. Start with `clicks/` to identify known lure-domain interaction.
2. Move to `email/` to identify delivered messages and URL-delivery correlation.
3. Use `cloud-apps/` to scope post-click and post-token-replay activity.

## Notes

- These queries are campaign-focused, not broad detections for every phishing scenario.
- Each query includes a short header describing the primary telemetry source, what the query is intended to find, and how it behaves if that telemetry is missing.
- Missing tables return empty results instead of semantic errors, but you still need the relevant telemetry to get useful coverage.
