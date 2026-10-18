# Production upgrade runbook: v1.1 -> v1.3

**Status:** RELEASE CANDIDATE. This file is instructions, NOT evidence of live deployment.

## Scope and ownership
- Keep the original live website, D1 database, permanent ID registry, history, sessions configuration, and existing shared access code.
- The source `.openai/hosting.json` retains production project ID `[private-project-id]` and the `DB` D1 binding.
- The Render/Neon beta site remains a separate staging installation; do not connect this release to its database.
- Production project secrets `ACCESS_CODE_HASH` and `SESSION_SECRET` MUST NOT change.

## Phase 1: freeze and backup (mandatory)
1. Arrange a maintenance window so users stop submitting new records.
2. From the current production site, download **Workspace settings > Download full backup**. This must be an export made *after* the last production write, not a prior beta backup.
3. Confirm backup is valid JSON with `format=team-equipment-backup`, `records`, `history`, `issuedIds` and `counters` arrays; inspect ID uniqueness, record totals and event totals. Save it in approved private storage. Do not email it or include it in GitHub.
4. Create a database-native D1 backup/snapshot if your hosting console supports it; retain an immutable pre-release version of the v1.1 source and deployment config.
5. Keep both backups outside the production host. All existing permanent IDs must remain reserved.

## Phase 2: rehearse away from production
6. Make a secured test database from an approved production backup/snapshot. Do not restore over the existing beta, and do not expose private employee data to testers.
7. Apply the registered Drizzle migration `drizzle/0002_beta_request_progress.sql`. Its journal entry and schema snapshot are included. It adds three tables and four indexes, with one complete DDL statement per line for the hosted importer and no triggers. Confirm it does not drop or change the `records`, `issued_ids`, `history`, `counters`, or `sessions` tables. The authenticated API preserves immutable journal revisions and manual email confirmations; a privileged database administrator can still modify storage.
8. Start v1.3 source against only this **test database** with test-specific session secrets and code. Compare pre/post counts and IDs. Confirm sign-in, original request notes, update history, manual draft and Needs attention.
9. Verify both v1.1 JSON backups (missing the new progress arrays) and v1.3 JSON backups are accepted by the v1.3 import validator into an **EMPTY** test database. Do not import any backup into a populated production database.

## Phase 3: cutover to current production site
10. Re-check the destination project ID and the D1 binding. Stop writes and take a **fresh final** backup if any records changed after rehearsal.
11. Apply the SQL migration to the **existing live D1 DB**, then publish the v1.3 code to the **existing live project**. Do not deploy an empty new database or a copied workspace under the old URL.
12. Keep `ACCESS_CODE_HASH` and `SESSION_SECRET` unchanged. An application deployment should not reset either; verify host secret values are preserved without displaying them.
13. Perform signed-in smoke tests: original employees/equipment/requests/history/IDs visible, existing shared access code accepted, version footer `v1.3`, dashboard Needs attention counts, new update history, email draft preview, search, CSV, JSON backup and responsive layout.
14. Confirm no console errors, failed save attempts, or unexpected privilege changes. Invite users back to normal work only after confirmation.

## Phase 4: rollback and monitoring
15. If the new UI fails, roll back application code only while preserving the new schema and writes. Do **not** restore a stale backup over fresh production entries.
16. Keep the pre-release backup and historical files until an actual recovery drill is completed. The added tables are non-destructive and can remain after a code-only rollback.
17. Keep the separate beta site as the testing environment for future updates and provide dated release notes when promoting another change.

## Current release prerequisites and evidence
- A fresh authenticated full JSON backup from the live production API has been saved: the private production dataset. The corrected migration was applied twice to a local copy without changing original rows; foreign-key checks passed. Take a new final backup if production writes occur after that snapshot.
- The connected tools do not expose a native D1 snapshot interface. Preserve the available durable full JSON backup and v1.1 git commit; the additive migration does not replace or remove original data. A native snapshot can be added if a maintainer has access to one, but is not represented as an available tool or completed check.
- The original Sites production project is connected for publication. Render and Neon continue to manage the separate beta only.
- Node.js 24 and source dependencies are available for the supported build and test commands. Record actual results in `QA_SUMMARY.md`.
- Authenticated local HTTP API checks are supported. Browser rendering and physical mobile-device testing remain unverified because the browser-control skill is unavailable in this session. Do not describe API checks as browser or phone verification.
- This runbook is not proof of a completed production upgrade. Record the deployment result, backup preservation and hosted authenticated API checks before marking the release complete.


## Additive manual WhatsApp update - 18 October 2026

This addendum applies to an existing v1.3 installation and preserves its original shared code, sessions configuration, database, image storage, employee/device/request links and email confirmations. It does not repeat the historical v1.1-to-v1.3 data cutover.

1. Preserve the current private database/media recovery state and compatible source. Use a complete `.ttbackup` archive for images; a JSON manifest alone cannot recover originals. Do not overwrite production from a stale backup.
2. In an isolated synthetic workspace, apply registered `drizzle/0004_manual_whatsapp.sql` after the existing migrations and exercise request-linked drafting, stale contacts/context, explicit confirmations, retry behavior and backup/restore. Keep one complete DDL statement per line for host compatibility.
3. Apply the additive migration through the existing hosted migration process and deploy to the existing project with its D1 and BUCKET bindings. Preserve `ACCESS_CODE_HASH` and `SESSION_SECRET`. No WhatsApp API/provider secrets or automatic sender are required.
4. Verify the current manual and source download, protected API access, old email behavior and the new request **Updates & messages** controls. Preview a synthetic draft without sending an external message. Compare original data only through authorized read-only checks; document any unavailable checks rather than claim completion.
5. New backups include `betaWhatsAppMarks` in the same snapshot and atomic restore boundary as existing data. Older backups without that array are still valid. Confirm a failed restore leaves no partial marks or core rows and the target gate includes the new table.
6. Record actual test/build/publication results and remaining browser/phone handoff limits in the dated release report. Never test by messaging an actual employee without a separate instruction to do so.

If rollback is necessary, preserve the new table and its confirmations; roll back compatible code deliberately without removing accepted logs or restoring stale records. Old export code may omit newer logs, so retain a complete current-version recovery snapshot.
