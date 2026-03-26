# Microsoft Defender XDR Token Replay Hunting Queries

Tiny, client-friendly repo with Microsoft Defender XDR Advanced Hunting queries for the Huntress-reported Railway / M365 token replay and device code phishing campaign, plus a small set of practical follow-on hunts for broader scoping.

Source reporting:
- https://www.huntress.com/blog/railway-paas-m365-token-replay-campaign

## What's here

### Campaign-focused hunts
Focused Microsoft Defender XDR Advanced Hunting queries tied to the reported lure and infrastructure patterns, organized by the telemetry that populates the underlying tables:

- `queries/campaign/clicks/`
  - `01_url_clicks_on_lure_infrastructure.kql`
  - `08_clicked_urls_for_impacted_users.kql`
- `queries/campaign/email/`
  - `02_messages_with_lure_themes_or_infrastructure.kql`
  - `03_click_delivery_correlation.kql`
- `queries/campaign/cloud-apps/`
  - `04_mailbox_cloud_app_activity_for_impacted_users.kql`
  - `05_inbox_rule_and_forwarding_hunt.kql`
  - `06_oauth_consent_and_app_activity_hunt.kql`
  - `07_teams_sharepoint_onedrive_follow_on_access.kql`

### General follow-on hunts
Broader scoping queries you can use after the initial token-replay checks. These are not specific to the Huntress campaign; they are meant to help you widen scope into adjacent endpoint, identity, cloud, and alert activity after the campaign-focused pivots.

- `queries/general/endpoint/`
  - `devices-talking-to-rare-external-domains.kql`
  - `encoded-powershell-downloads.kql`
  - `suspicious-rundll32-network-activity.kql`
- `queries/general/cloud-apps/`
  - `impossible-travel-cloud-app-activity.kql`
- `queries/general/identity/`
  - `rare-successful-identity-logons.kql`
- `queries/general/alerts/`
  - `recent-alerts-with-entities.kql`

Folder guides:

- `queries/campaign/README.md`
  - explains how the Huntress-specific campaign hunts are organized
- `queries/general/README.md`
  - explains what the broader follow-on hunts are for and when to use each telemetry family

## How to use

1. Open **Microsoft Defender XDR** → **Advanced hunting**.
2. Start with the numbered campaign-focused queries in `queries/campaign/`.
3. Replace placeholders like `<user1@domain.com>` with impacted users.
4. Use the `queries/general/` folders to scope related activity across endpoint, cloud, identity, and alert telemetry.
5. Tune time windows, allowlists, and thresholds for your environment before operationalizing anything.

If your editor still shows flat `queries/01...08` files, refresh the file tree. The repo now stores those hunts only under `queries/campaign/`.

## Choose by telemetry or license

Microsoft Defender XDR is the unified hunting portal and incident experience, but individual Advanced Hunting tables are populated by deployed Defender services. In practice, telemetry coverage is the reliable way to choose queries; common license bundles are just a shortcut.

| Environment / likely licensing | Start here | Notes |
| --- | --- | --- |
| Full Microsoft Defender XDR or Microsoft 365 E5-equivalent | All `queries/campaign/` and `queries/general/` folders | Broadest coverage across email, endpoint, cloud, identity, and alerts |
| Defender for Office 365 Plan 2 | `queries/campaign/email/`, `queries/campaign/clicks/` | Click hunts can use full Safe Links telemetry; email hunts require mail telemetry |
| Defender for Endpoint or Defender for Business | `queries/general/endpoint/`, `queries/campaign/clicks/` | Click hunts fall back to endpoint browser telemetry when Safe Links data is absent |
| Defender for Cloud Apps | `queries/campaign/cloud-apps/`, `queries/general/cloud-apps/` | Best for SaaS activity, mailbox actions surfaced via CloudAppEvents, and app activity pivots |
| Defender for Identity | `queries/general/identity/`, `queries/general/alerts/` | Useful for identity pivots and identity-backed alert triage |

## Suggested triage order

If you're responding to a suspected token replay / device code phishing incident, a good order is:

1. `queries/campaign/clicks/01_url_clicks_on_lure_infrastructure.kql`
2. `queries/campaign/email/02_messages_with_lure_themes_or_infrastructure.kql`
3. `queries/campaign/email/03_click_delivery_correlation.kql`
4. `queries/campaign/clicks/08_clicked_urls_for_impacted_users.kql`
5. `queries/campaign/cloud-apps/04_mailbox_cloud_app_activity_for_impacted_users.kql`
6. `queries/campaign/cloud-apps/05_inbox_rule_and_forwarding_hunt.kql`
7. `queries/campaign/cloud-apps/06_oauth_consent_and_app_activity_hunt.kql`
8. `queries/campaign/cloud-apps/07_teams_sharepoint_onedrive_follow_on_access.kql`
9. Then pivot into the `queries/general/` hunts for broader scoping.

## Notes

- These are hunting queries, not turnkey detections.
- Table availability varies by tenant licensing and telemetry coverage.
- All queries now guard against missing Advanced Hunting tables so they return empty results instead of semantic errors when a tenant lacks specific telemetry.
- The email hunts still require Defender for Office 365 mail telemetry to return mail results, and the cloud-app hunts still require `CloudAppEvents` coverage to return SaaS activity.
- The click hunts in `queries/campaign/clicks/` prefer `UrlClickEvents` and fall back to `DeviceNetworkEvents` when Safe Links click telemetry is unavailable.
- The original Huntress post also includes Sentinel sign-in hunts; this repo intentionally stays M365 Defender-focused.
- Each query now starts with a short header describing its primary telemetry source, behavior, and missing-telemetry behavior.

## License

MIT
