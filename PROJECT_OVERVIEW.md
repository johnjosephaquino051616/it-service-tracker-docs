# SinCityNotary Team Tracker v1.3

This release promotes the user-approved v1.2.2 beta functionality to the original production codebase.
The v1.3 production release was published and its authenticated API and record preservation were verified on 17 October 2026. This source also includes the 17 October make/brand model-catalog and processor-family corrections. The package retains the existing `.openai/hosting.json` project ID and D1 `DB` binding; the separate Render/Neon beta remains separate.

Do not change the live shared code. Reuse the existing production `ACCESS_CODE_HASH` and `SESSION_SECRET` in the host's secret store. Never include secret values in the source ZIP.

## Added features
The 18 October storage update offers the same combined capacity/type choices, such as **128GB SSD**, **512GB SSD**, **1TB HDD** and **2TB NVMe SSD**, for **Primary storage**, **Secondary storage** and **Tertiary storage** in Add/Edit device. All 360 choices are accessible by scrolling or filtering; **128GB** is available with every drive type for all three roles. Labels omit the space before GB/TB, and capacities sort numerically from **1GB** to **24TB**, with GB before TB. Search accepts both compact and spaced capacity text. **Add secondary storage** and **Add tertiary storage** reveal the next missing role, up to three per device. Clearing a role never renumbers another. Readable summaries carry the drive roles into details, search and CSV; backups and raw export fields retain stored values. Older capacity-only/type-first values remain intact through unrelated edits, and unknown types are not guessed. This payload-only update requires no SQL migration or existing-record rewrite; shared access, Asset IDs, employee links and uploaded originals stay unchanged.

The current workflow is labeled **Report/Request** throughout navigation, add/edit actions, ID/search labels, CSV headings and newly prepared employee messages. This 18 October v1.3 label update preserves internal `requests` API/storage identifiers, existing records, permanent IDs, employee/device links and historical message confirmations; it requires no database migration. The manual and developer handoff use the current labels.

The 18 October manual WhatsApp addition mirrors the request email workflow in **Updates & messages**. **WhatsApp employee** prepares an editable update for the request's linked employee using their saved, country-validated Contact number. **Open WhatsApp** opens a click-to-chat draft; the operator reviews it and presses Send in WhatsApp. A separate **Mark WhatsApp as sent** action records only a self-reported confirmation, never delivery. There is no automatic sender, carrier SMS, WhatsApp API or provider secret. Missing or invalid numbers must be corrected on the linked Employees profile. Existing email confirmations remain separate. Current backups include WhatsApp confirmations atomically; older backups without them remain valid. Apply registered additive migration `drizzle/0004_manual_whatsapp.sql`; see the current developer handoff for its API and backup contract.

The 18 October warranty addition places **Warranty Status** under **Lifecycle and security** in Add/Edit device, with **Expire/OOW**, **Current** and **Extended** choices. The optional per-device value is independent of Warranty end date. Existing records are not backfilled, and no SQL migration is required. The guide and developer handoff describe the field.

The 18 October device-image update adds immutable originals under Equipment > Additional notes, an employee/device Gallery, and complete `.ttbackup` archives containing original images. See `docs/IMAGE_UPLOADS_2026-10-18.md` for this update's checks and limits. Hosted images use the private `BUCKET` binding; portable images use persistent private `MEDIA_PATH` storage.

1. Request-level progress updates saved separately from Notes; independent saved revisions/history.
2. Manually composed email drafts to an employee's saved email; optional internal "Marked as sent" audit entry (no automatic send or delivery verification).
3. Needs attention counts only unarchived High/Urgent Report/Request records whose status is neither Resolved nor Closed. Equipment status and condition never contribute. The card always opens Report/Request, including when its count is zero.
4. Full backups include progress, revisions and separate manual email/WhatsApp confirmations in one consistent snapshot; restores validate and commit the complete dataset atomically. Older v1.1 full backups remain compatible.
5. Version labels, cache key and user manual updated to 1.3.

## Required upgrade procedure
See `docs/PRODUCTION_UPGRADE_V1.3.md` and `docs/Team_Tracker_Guide.md` (also included as PDF).
**Take and verify a current, private full backup of production first.** Apply pending registered migrations in order, including `drizzle/0002_beta_request_progress.sql`, `drizzle/0003_device_images.sql` and `drizzle/0004_manual_whatsapp.sql`, to the existing D1 database before publishing this hosted frontend/server. Never overwrite the D1 database or restore test data into production. The registered Drizzle migration adds three tables and four indexes. Each DDL statement occupies one line for the hosted importer; it installs no triggers. The authenticated API protects journal revisions and manual email confirmations from modification.

## Build
Source targets Node.js 24 and pnpm 11.25.0 (`pnpm install --frozen-lockfile`). Hosted wrapper: `pnpm run build`. Portable build: `pnpm run portable:build`. `pnpm run typecheck` and `pnpm run test:api` plus frontend, security, search, catalog and portable tests are release checks. Node.js 24 and the source dependencies are available for the current release work. See `docs/QA_SUMMARY.md` for completed checks and remaining limits.

The historical filename `server/beta-updates.ts`, table prefix `beta_` and `/api/beta/` endpoints are maintained for backwards-compatible functionality; **they use the production database** on the live installation, not the separate beta service.

## Rebuild and check the processor catalog

Run from the source root:

```sh
python3 scripts/refresh-processor-catalog.py
node scripts/test-processor-catalog.mjs
```

The refresh script reconstructs the catalog from the retained model and CPU sources. The independent CPU check verifies manufacturer, family and exact-SKU mappings. CPU selection follows manufacturer → family → exact SKU, including Intel → Core i5 13th Gen → Core i5-13420H; the manufacturer name is AMD. Search covers the complete dropdown list, and unlisted processors use Other / custom plus Exact processor (custom). Previously saved labels remain available on edit without making an old misclassification a new selection.

In Add/Edit device, Exact model / model number appears only for Model → Other / custom. Installed build / custom OS version appears only when Operating system, Edition or OS version is Other / custom. Displaying an existing record does not erase hidden custom-detail text; changing a parent choice retains the existing dependent-field resets. See the user guide for the entry workflow.

## Maintenance - 18 October 2026

See `docs/MAINTENANCE_2026-10-18.md` and `qa/dependency-security-2026-10-18.json`.
The maintenance release updates security dependencies and hosted response headers and reads bootstrap records/history together in one consistent database batch. It does not change stored records, catalog labels, schema or team access settings. Next.js is 16.4.2; the React/RSC packages stay on the compatible 19.2 branch at 19.2.9. Reviewed transitive security pins live in `pnpm-workspace.yaml`. Exact recent security releases have explicit exceptions to the default seven-day release-age policy; the default remains in effect for all other dependencies.

One upstream `braces` build-tool advisory has no published patched version. Builds use trusted project glob patterns; tracker forms/search do not execute glob patterns. Keep development servers private. Browser/physical-device verification remains outstanding. This historical maintenance note predates the device-image update; the latest guide now documents its upload, gallery and media-backup workflows.

## Machine-model coverage - 18 October 2026

Official Philippine and worldwide support/product inventories added 18,759 verified brand/device/model choices across all 13 named brands, including 3,078 Acer choices. Select Laptop / Acer and search A715-59G for Aspire A715-59G. Existing choices and operational records are retained. See `docs/MODEL_CATALOG_AUDIT.md` and `shared/model-source-audit-2026-10-18.json` for per-brand counts, evidence, exclusions and coverage limits; the Help & guide CSV contains the complete named list.
