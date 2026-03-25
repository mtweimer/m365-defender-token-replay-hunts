# M365 Defender Token Replay Hunting Queries

Tiny repo with Microsoft 365 Defender Advanced Hunting queries for the Huntress-reported Railway / M365 token replay and device code phishing campaign.

Source reporting:
- https://www.huntress.com/blog/railway-paas-m365-token-replay-campaign

## What's here
- Focused M365 Defender Advanced Hunting queries
- Specific infrastructure and lure patterns pulled from the reporting
- Simple placeholders so defenders can swap in impacted user email addresses

## How to use
1. Open Microsoft 365 Defender Advanced Hunting.
2. Copy a query from `queries/`.
3. Replace placeholder values like `<user1@domain.com>`.
4. Run the broad hunts first, then pivot into impacted users and cloud app activity.

## High-value starting points
1. `01_url_clicks_on_lure_infrastructure.kql`
2. `02_messages_with_lure_themes_or_infrastructure.kql`
3. `03_click_delivery_correlation.kql`
4. `04_mailbox_cloud_app_activity_for_impacted_users.kql`
5. `05_inbox_rule_and_forwarding_hunt.kql`
6. `06_oauth_consent_and_app_activity_hunt.kql`
7. `07_teams_sharepoint_onedrive_follow_on_access.kql`

## Notes
- These are hunting queries, not detections.
- Table availability varies by tenant licensing and telemetry coverage.
- The original Huntress post also includes Sentinel sign-in hunts; this repo intentionally stays M365 Defender-focused.
