# SinCityNotary Team Tracker v1.3 - Release report

Published to the original production site on 17 October 2026, 02:27 Philippines time.

URL: [private production address omitted]

## Release
- The native hosting service reports successful deployment of v1.3.
- Original site, DB binding and runtime environment revision retained. Shared access code accepted by the hosted API.
- Added request progress journal, correction history, manual employee email drafts/confirmations and corrected Needs attention calculation.
- Fixed migration packaging and registration, atomic full backup/restore, unsaved journal safeguards, stale-session handling and changed-recipient confirmation protection.
- The in-app manual and source ZIP match the release downloads. Source excludes real credentials, production data and databases.

## Data preservation
- Fresh pre-upgrade and post-upgrade authenticated full exports were captured.
- All 7 original records, the original history, 8 permanent ID reservations and the employee counter are unchanged.
- Three additive progress tables are present. No old record was replaced and no backup was imported into production.
- Applying the migration twice to an isolated copy preserved every original row with no foreign-key errors.
- A native D1 snapshot interface was unavailable; recovery safeguards are the durable JSON backup and preserved v1.1 source/deployment. Roll back code without overwriting newer data.

## Verification
- 209 automated checks passed: 24 core API, 32 security, 76 frontend state, 16 search, 16 extra search, 13 catalog, 4 migration, 15 portable HTTP integration, 13 focused v1.3 progress/backup/dashboard.
- TypeScript checks, hosted build, portable build and ZIP integrity checks passed.
- The source ZIP was compared against the published Git commit. The 14-page manual was rendered and visually checked.
- Hosted checks passed: existing team-code login, blocked unauthenticated records, bootstrap, v1.3 health, original request progress feed and email log, Request ID search, CSV export, full JSON export and logout.
- Hosted PDF and source ZIP bytes match the release downloads. The current saved data produces Needs attention = 2 (one flagged device plus one unresolved urgent request).
- No production test records, progress entries or email confirmations were added; no email was sent.

## Limits
- Interactive browser layout, keyboard/focus behavior and physical phone/tablet testing remain unverified; browser-control capability was unavailable.
- Manual email opens or copies a draft. The tracker sends no email and does not verify delivery. Mark as sent is a team member's statement.
- Shared-code sessions use self-reported activity labels. Database administrators retain direct storage access.

## Source provenance
- Published source commit: b7cdc86fea6fd1382c7baac5e3dd93a04b4e777e
- Native saved version: 6 (the application edition is v1.3).
- Prior production source commit preserved: 5ce1034b877ebf28c5ed929ed60a35390855a273

## Download fingerprints
- SinCityNotary_Tracker_v1.3_Manual.pdf: `[private verification fingerprint omitted]`
- SinCityNotary_Tracker_v1.3_Source.zip: `[private verification fingerprint omitted]`
- SinCityNotary_Tracker_v1.3_Portable.zip: `[private verification fingerprint omitted]`
- production-post-v13-backup-2026-10-17.json: `[private verification fingerprint omitted]`
