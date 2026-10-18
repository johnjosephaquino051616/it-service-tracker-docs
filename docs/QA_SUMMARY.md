# SinCityNotary Team Tracker v1.3 — Release QA

Prepared: 17 October 2026
Status: historical v1.3 release QA. The original production release was verified on 17 October 2026; see MAINTENANCE_2026-10-18.md for current maintenance evidence and remaining limits.

## Current manual WhatsApp update - 18 October 2026

The current source adds request-linked manual WhatsApp drafts and separate self-reported sending confirmations. Historical totals below describe earlier releases. See `WHATSAPP_RELEASE_2026-10-18.md` for the checks actually run against this update; they do not imply external WhatsApp sending or a verified physical-phone handoff. No external employee messages are required for QA.

## Fresh production backup and migration rehearsal
- A fresh authenticated backup from the original live API was saved before publication: the private production dataset. Private backup fingerprint omitted. The private backup is excluded from source/download packages.
- The corrected additive migration was applied twice to a local copy of that backup. All original rows were unchanged, all 10 application tables were present, and foreign-key checks found no errors.
- A native D1 snapshot interface is unavailable in the connected tools. The available recovery safeguards are the fresh durable full JSON backup, preserved v1.1 git commit and non-destructive additive migration. Take another final JSON backup if writes occur after the captured snapshot.

## Earlier backup evidence
- The user-provided production JSON backup was accepted by the actual v1.3 restore validator in an isolated SQLite rehearsal.
- The rehearsal preserved original records, history, issued IDs and counters byte-for-byte. See `BACKUP_VALIDATION_2026-10-16.md` for that snapshot's dated results.
- Historical dashboard checks used a combined device/request rule; the current Report/Request-only rule supersedes it. Private snapshot counts are omitted.
- A current final off-host backup is required at cutover if production writes occurred after that snapshot. A host-native D1 snapshot should also be retained if available.

## Current release environment and migration
- The original Sites production project is connected; its project ID, D1 binding and existing secret names are retained. The beta Render/Neon installation remains separate.
- Node.js 24 and source dependencies are available for release checks. Earlier notes about Node 22, missing dependencies or missing publication access are obsolete.
- `drizzle/0002_beta_request_progress.sql` is registered in the Drizzle journal and schema snapshot. It adds three tables and four indexes, uses one complete DDL statement per line and installs no triggers.
- Journal revision and manual-email confirmation immutability is enforced through the authenticated API. Privileged database administrators can modify underlying storage.

## Current focused v1.3 checks
- `node scripts/test-v13.mjs`: 13 cases passed, covering journal corrections/history, manual email validation, archived/concurrent edits, complete backup round-trip, legacy v1.1 restore, clean-target rejection, rollback after an injected extension-write failure and a simulated export-boundary race, concurrent progress corrections (one accepted and one conflict) and a recipient change between validation and logging (rejected without an incorrect email confirmation).
- Full export now reads all seven data tables in one transactional batch; import validates every core/extension row before committing one atomic batch. The restore gate includes all data tables and allows session/rate-limit rows.

- Core API suite: 24 cases passed; security: 32; frontend state: 76; search: 16; additional search: 16; catalog: 13; migration: 4. TypeScript typechecking passed. Hosted and portable builds passed. The copied portable runtime passed 15 HTTP integration checks. The updated 14-page PDF was rendered and visually checked. The accompanying release report records hosted publication results.

## Checks to record before release completion
Run typechecking, API/auth, catalog, security, frontend state, search and portable-server checks, then build the hosted and portable targets. Exercise the request progress journal, correction history, email draft/confirmation validation, archived request protections, concurrent edits, complete backup/restore and clean-target enforcement. Record final command results with the published release; earlier candidate check totals do not certify later source changes.

Verify on the hosted service that the existing shared code still works, unauthorized data access is denied, existing records and permanent IDs remain present, v1.3 assets are served, and full backups include the new progress data. Test email composition without sending to an actual employee. The website has no SMTP or delivery verification.

## Verification limits
- Local HTTP API testing is supported without a browser.
- Browser rendering, interactive desktop/mobile layout, keyboard/focus behavior and testing on physical phones are unverified. The browser-control skill is unavailable in this session.
- Static source checks and HTTP API checks do not prove rendered browser behavior.
- Keep `ACCESS_CODE_HASH` and `SESSION_SECRET` unchanged on the existing installation. Never restore a stale snapshot over accepted production writes.

## Processor catalog correction - 17 October 2026

- Independent CPU suite: 14 checks passed against all 4,454 canonical named mappings, 173 manufacturer/family paths, 17 manufacturer options and 661 exact historical tuples. Actual SQLite API creates and edits one synthetic device per family, rejects wrong parents, exports/restores all records and audit events, and preserves historical labels through unrelated edits.
- General catalog suite: 14 checks passed. Existing model suite: 11 checks and 741,211 assertions passed; all 4,588 model labels and the full model CSV are unchanged. Core API: 24 cases; frontend state: 76 cases; TypeScript typechecking passed.
- Both refresh commands reproduce the same catalog, processor CSV, legacy tuples, source notes and audit manifest byte-for-byte. Non-CPU catalog rows exactly match the pre-CPU snapshot.
- The updated 14-page guide was rebuilt and its changed hardware page rendered and visually checked. Browser rendering and physical-phone testing remain unverified.
- Copied portable runtime: 17 real HTTP checks passed, including exact processor CSV bytes and saving Core i5-13420H through the bundled Node adapter. Hosted publication/data-preservation evidence is recorded separately after deployment. No live test records are created.
