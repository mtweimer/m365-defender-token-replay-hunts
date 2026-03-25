# M365 Defender Token Replay Hunting Queries

Tiny, client-friendly repo with Microsoft 365 Defender Advanced Hunting queries for the Huntress-reported Railway / M365 token replay and device code phishing campaign, plus a small set of practical follow-on hunts for broader scoping.

Source reporting:
- https://www.huntress.com/blog/railway-paas-m365-token-replay-campaign

## What's here

### Campaign-focused hunts
Focused M365 Defender Advanced Hunting queries tied to the reported lure and infrastructure patterns:

1. `queries/01_url_clicks_on_lure_infrastructure.kql`
2. `queries/02_messages_with_lure_themes_or_infrastructure.kql`
3. `queries/03_click_delivery_correlation.kql`
4. `queries/04_mailbox_cloud_app_activity_for_impacted_users.kql`
5. `queries/05_inbox_rule_and_forwarding_hunt.kql`
6. `queries/06_oauth_consent_and_app_activity_hunt.kql`
7. `queries/07_teams_sharepoint_onedrive_follow_on_access.kql`
8. `queries/08_clicked_urls_for_impacted_users.kql`

### General follow-on hunts
Broader scoping queries you can use after the initial token-replay checks:

- `queries/general/rare-successful-identity-logons.kql`
- `queries/general/encoded-powershell-downloads.kql`
- `queries/general/suspicious-rundll32-network-activity.kql`
- `queries/general/impossible-travel-cloud-app-activity.kql`
- `queries/general/recent-alerts-with-entities.kql`
- `queries/general/devices-talking-to-rare-external-domains.kql`

## How to use

1. Open **Microsoft Defender XDR** → **Advanced hunting**.
2. Start with the numbered campaign-focused queries in `queries/`.
3. Replace placeholders like `<user1@domain.com>` with impacted users.
4. Use the `queries/general/` set to scope related activity across identity, endpoint, cloud, and incident telemetry.
5. Tune time windows, allowlists, and thresholds for your environment before operationalizing anything.

## Suggested triage order

If you're responding to a suspected token replay / device code phishing incident, a good order is:

1. `01_url_clicks_on_lure_infrastructure.kql`
2. `02_messages_with_lure_themes_or_infrastructure.kql`
3. `03_click_delivery_correlation.kql`
4. `08_clicked_urls_for_impacted_users.kql`
5. `04_mailbox_cloud_app_activity_for_impacted_users.kql`
6. `05_inbox_rule_and_forwarding_hunt.kql`
7. `06_oauth_consent_and_app_activity_hunt.kql`
8. `07_teams_sharepoint_onedrive_follow_on_access.kql`
9. Then pivot into the `queries/general/` hunts for broader scoping.

## Notes

- These are hunting queries, not turnkey detections.
- Table availability varies by tenant licensing and telemetry coverage.
- The original Huntress post also includes Sentinel sign-in hunts; this repo intentionally stays M365 Defender-focused.
- Query comments call out useful tuning ideas where helpful.

## License

MIT
