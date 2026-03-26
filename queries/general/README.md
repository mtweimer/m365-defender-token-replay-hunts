# General Follow-On Hunts

These queries are broader scoping hunts intended for use after the campaign-specific queries in `queries/campaign/`. They are not tied only to the Huntress Railway campaign; they help you look for adjacent suspicious activity that may indicate a wider compromise.

## How This Folder Is Organized

- `endpoint/`
  - Endpoint network and process hunts for unusual outbound domains, suspicious PowerShell execution, and `rundll32.exe` network activity.
  - Best fit when Defender for Endpoint telemetry is available.
- `cloud-apps/`
  - Cloud activity hunts such as fast country changes for the same account in SaaS activity.
  - Best fit when `CloudAppEvents` coverage is available.
- `identity/`
  - Identity hunts such as successful logons from IP addresses not previously seen for the same account.
  - Best fit when identity hunting telemetry is available.
- `alerts/`
  - Rapid triage helpers that join alerts to evidence/entities so you can pivot faster.
  - Useful when Microsoft Defender XDR alert and evidence tables are populated.

## When To Use These

- After you find campaign-specific activity and want to scope blast radius.
- When you want to look for related suspicious behavior even if you do not yet have a confirmed campaign hit.
- When your licensing or telemetry covers only part of Defender XDR and you need hunts that align to the tables your tenant actually has.

## Notes

- These are still hunts, not turnkey detections.
- Like the campaign queries, they now return empty results instead of semantic errors when a required table is missing.
