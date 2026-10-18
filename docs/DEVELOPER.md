# SinCityNotary - Developer handoff

**Release:** 1.3 - storage capacity/type and drive roles, manual WhatsApp, device images and warranty status, 18 October 2026

The existing production project has a hosted wrapper and an independent bundled React browser client/Node.js 24 server. Keep business rules in `server/api.ts` so deployment adapters share permanent IDs, validation and history.

## Core paths

- `shared/fields.ts`: form configuration, labels, hints, options and dependencies.
- `shared/types.ts`: shared record/event contracts.
- `shared/search.ts`: search parameter types and result contract.
- `server/search.ts`: authenticated global SQL search rules.
- `web/GlobalSearch.tsx` and `web/theme.ts`: search dialog and browser appearance preference.
- `shared/role-guide.ts`: editable role-based access planning suggestions, not permission grants.
- `web/`: reusable responsive React interface.
- `server/`: common API, security and database adapters.
- `server/input.ts`: shared route-ID decoding with client errors for malformed encodings.
- `server/whatsapp.ts`: country-validated request-linked drafts, fresh signed-context checks, manual click-to-chat links and separate append-only confirmations.
- `server/beta-updates.ts`: v1.3 progress journal, revision history, separate manual email/WhatsApp confirmations and compatible backup handling; the historical filename does not connect production to the separate beta.
- `shared/image-formats.ts` and `server/image-types.ts`: supported image extensions, signature checks, safe preview flags, filename checks and size limits.
- `server/images.ts`: append-only image upload/list/content API and immutable metadata validation.
- `server/node-images.ts`: private filesystem storage adapter; hosted deployments supply the BUCKET object-storage binding.
- `server/backup.ts` and `server/full-backup.ts`: shared atomic application-data restore and streamed complete-media backups.
- `drizzle/0004_manual_whatsapp.sql`: additive append-only manual WhatsApp confirmation table and indexes; original email logs stay separate.
- `drizzle/0003_device_images.sql`: additive image batch/metadata tables and three indexes; original record rows remain untouched.
- `drizzle/0002_beta_request_progress.sql` and `drizzle/meta/`: registered additive migration, journal and schema snapshot.
- `shared/catalog.json`, `shared/catalog.ts` and `shared/catalog-sources.md`: hardware/OS catalog, rules and provenance. International phone validation uses libphonenumber-js/max.
- `portable/`: Node server and browser build entry.
- `docs/`: manual, security notes and this handoff.

## Runtime

Use Node24 for the portable server. Set `ACCESS_CODE_HASH` and `SESSION_SECRET` privately. Use the helper with interactive input so the real code is not saved in shell history. Choose a persistent SQLite path outside public assets. Check `.env.example` and `package.json` for exact supported variable names and scripts. Do not expose development mode as a production server.

## Data rules

Records use stable IDs and revision numbers. The common API validates identities, links and revisions and writes each accepted record mutation with its history event in an atomic server/database transaction. Database uniqueness constraints provide an additional collision boundary; this release does not rely on SQL trigger bodies or runtime DDL for business-rule enforcement. Enforce immutable identities and request references on the server; disabled controls are not a security boundary. Preserve ID reservations when archiving, restoring or moving. Employee sequences are three digits; ticket suffixes are five. Exhaustion must return an error rather than reuse an ID.

Validate name, date, international phone rules and dependent catalog combinations on the server. Devices retain Asset IDs during transfers. Requests may omit assets; selected assets must belong to the selected employee when created.

## Add or change fields

1. Update `shared/fields.ts` and shared types where needed.
2. Update server validation/allowlists and relational checks.
3. Review cards, search, reports, backup/import and historical values.
4. Add a SQL migration if schema, indexes or constraints change.
5. Add focused valid/invalid tests and run release checks.

Generic payload fields may not need SQL columns. Removing a form field must not silently discard history or break older backups.

Warranty Status is the optional device payload field `warrantyStatus` in `shared/fields.ts`, placed under Lifecycle and security in both Add device and Edit device. Its ordered options are exactly `Expire/OOW`, `Current`, `Extended`; other nonblank values are rejected by shared/server validation. It has no default, no existing-record backfill and no calculated relationship to `warrantyEnd`. Each value stays on its own device record/Asset ID, including when an employee has multiple devices. The common field schema supplies API and backup allowlists and CSV columns; normal record saves retain revision/history protections. No SQL migration is needed for this payload field. Older backups without it remain valid; do not infer a status during restore.

### Device storage roles and legacy values

Storage uses three fixed, optional device payload slots: `storage` is **Primary storage**, `secondaryStorage` is **Secondary storage**, and `tertiaryStorage` is **Tertiary storage**. All three use the same 360 combined capacity/type options, for example `128GB SSD`, `512GB SSD`, `1TB HDD` and `2TB NVMe SSD`. Compact capacity labels have no space before GB or TB. Capacity choices span the retained 16GB-24TB list plus 1GB, 2GB, 4GB and 8GB, with 128GB available for all 12 types and all three roles. Sort by numeric capacity across units, not lexical text: 1GB, 2GB, 4GB, through 120GB, 128GB, 160GB, and later 1TB, 2TB and 10TB. Within a capacity retain the defined type order. Types include SSD, HDD, NVMe SSD, SATA SSD, SAS SSD, SAS HDD, Apple SSD, eMMC, UFS, Fusion Drive, RAID / storage pool and External / network. A blank slot means no storage value is recorded for that role; do not use the old `None` sentinel for new choices.

Storage dropdowns expose all 360 options when the query is empty; do not apply the 100-item result cap used by other catalog selectors. Their scrollable list and text filter both reach 128GB and every later capacity. Storage search normalizes spaced/compact capacity text so either form finds records; leave unrelated catalog and text fields untouched.

The editor shows primary initially; **Add secondary storage** and **Add tertiary storage** reveal the next missing row under Hardware specifications > Storage. Primary is cleared through its selection; removing secondary/tertiary clears only that named payload slot. Never compact, promote or reindex drives when another role is blank or removed. Values remain attached to the same physical device's Asset ID and employee assignment. The shared field schema, validation, details/search and CSV/backup paths must all recognize tertiary storage.

Retain legacy `storage`, `storageType` and `secondaryStorage` values exactly through reads, unrelated edits, history and backup/restore. The shared display helper compacts known legacy GB/TB labels and may combine a saved primary capacity plus type or render an older type-first secondary label in the current order; this is presentation, not a migration. Previously saved spaced combined values remain valid and retain their exact bytes through unrelated saves, history and backups. Device/history display and readable CSV summaries use compact labels; raw CSV field columns remain exact. Unknown types display **type not specified** and are never inferred from capacity, manufacturer or model. An explicit selection writes the chosen canonical capacity/type to that role. Preserve unchanged legacy values through validation rather than making every old label a new dropdown choice. CSV retains raw field columns and adds a role-aware readable `storageSummary`; search includes the readable roles/types/capacities. No SQL schema migration, backfill, ID change or authentication change is required.

## Portability and mobile

Keep client and API on the same HTTPS origin. Cross-origin cookies need additional design/testing. The hosted adapter uses D1, while the portable adapter uses `node:sqlite`. Move source separately from a confidential full backup. Restore to a clean target, verify links/history/registry, then switch the team URL with source writes paused for the final snapshot.

Responsive UI, REST contracts, manifest and icons are ready for future wrapper work. Store signing, permissions, push, offline synchronization and platform session behavior require further work. This source does not contain a compiled native app.

Keep short-screen search dialogs scrollable, editor headers/footers reachable, long saved labels wrapped and tablet/phone touch controls at least 44 CSS pixels where revised. Mobile navigation returns keyboard focus to its trigger, supports Escape and must not leave hidden links in the tab order. Review at 320-1280 CSS-pixel widths, short landscape heights and with an on-screen keyboard in both themes. Source review and automated checks are not evidence of actual browser rendering.

Device image selection uses the browser's file picker and authenticated same-origin APIs. A future native wrapper must preserve file-byte integrity, 10 MiB/file and six-file batch limits, retry identity, unsaved-file guards, secure private downloads and session-expiry cleanup. Add photo-library/camera permissions only when the native implementation needs them; the image addition does not change the PWA manifest or grant camera access.

## Release checks

Run automated API/auth checks, TypeScript checking and hosted/portable production builds. Test manual-ID rejection, relations, stale revisions, failed sign-in and backup restoration against a temporary database. Check phone and desktop screens in real browsers, keyboard navigation and long text. Report only checks actually run and retain QA notes.

## Environment variables

| Variable | Purpose / default |
| --- | --- |
| ACCESS_CODE_HASH | Required PBKDF2 representation; server secret |
| SESSION_SECRET | Required random value of at least 32 characters; server secret |
| DATABASE_PATH | Portable SQLite file; default `./data/tracker.sqlite` |
| MEDIA_PATH | Portable private image directory; default `./data/media`; persistent and outside public assets |
| PUBLIC_ORIGIN | Portable browser origin; HTTPS required in production |
| HOST | Portable bind address; default `127.0.0.1` |
| PORT | Portable listening port; default `3000` |
| TRUST_PROXY | Default false; true only behind a trusted proxy overwriting X-Forwarded-For |
| NODE_ENV | Set `production` on production Node hosts |

The hosted adapter receives the D1 `DB` binding, private object-storage `BUCKET` binding, ACCESS_CODE_HASH and SESSION_SECRET. Keep the expected origin tied to the deployed origin. Do not treat caller-provided proxy headers as trustworthy on a directly reachable Node server.

## REST contract

All paths use the app's same origin. JSON requests send Content-Type: application/json. Mutations send Origin; after login they also send X-CSRF-Token obtained from the session response. Browser requests include the session cookie automatically on the same origin.

| Method and path | Input / result |
| --- | --- |
| POST /api/login | `{code}`; session cookie, CSRF token and default actor |
| GET /api/session | Authenticated flag; CSRF token and actor when signed in |
| PATCH /api/session | `{actor}`; self-reported label of 1-80 characters |
| POST /api/logout | Revoke the current session |
| GET /api/bootstrap | Up to 10,000 records and latest 300 events in one consistent database batch |
| GET /api/search | Authenticated global record/activity search with filters, sort and pagination |
| GET /api/records/:id | One saved record, including archived records |
| GET /api/history/:eventId | One exact event, including its full before/after data |
| GET /api/history | Optional recordId and cursor; up to 500 events and nextCursor |
| POST /api/records | `{kind,data,clientKey}`; generated record ID |
| PATCH /api/records/:id | `{data,revision}`; full editable field data and current revision |
| POST /api/records/:id/archive | `{revision,archived}`; true archives, false restores |
| POST /api/records/:assetId/images | Multipart fields `files` (1-6), `clientKey`, `revision`, `employeeId`; append-only accepted batch |
| GET /api/records/:assetId/images | Saved images for exactly one device, including archived devices |
| GET /api/device-images | Gallery; optional `employeeId` (current assignment) and `assetId`; an empty employeeId filters unassigned devices |
| GET /api/device-images/:imageId/content | Authenticated original bytes; `download=1` forces download; unsupported preview types always download |
| GET /api/export | Consistent JSON snapshot including image metadata; with image references it is a manifest, not a complete restorable media backup |
| GET /api/export-full | Streamed .ttbackup NDJSON archive including core snapshot, image metadata, original image bytes, hashes and completion trailer |
| GET /api/export.csv?kind=employees | Excel-compatible CSV for a record category |
| POST /api/import | `{code,backup}`; legacy JSON restore into an empty database; rejects image references |
| POST /api/import-full | Raw .ttbackup File body; current code in x-backup-code plus normal session/Origin/CSRF; up to 90 MiB; clean target only |

Record kinds are employees, systems, requests, access and accessories. POST clientKey is a unique 16-100 character alphanumeric/underscore/hyphen save key used for retry-safe creation. A UUID is suitable. Do not generate permanent IDs in a client. PATCH uses all editable field data, not a partial merge; obtain the latest revision first. API-generated request ticket and employeeSnapshot values must be excluded from editable payloads.

Errors return an error message and sometimes a field key. Typical statuses: 400 malformed input/route encoding, 401 session/code, 403 origin/CSRF, 409 conflict or capacity, 413 body too large, 422 invalid input and 429 login rate limit. A 409 stale revision requires refresh/reconciliation, not automatic blind retry.

Full export reads core, v1.3 progress/revision/email/WhatsApp and image metadata tables in one transactional database batch, so concurrent saves cannot split the metadata snapshot. The JSON format remains `version: 1` with optional progress/image fields for compatibility. Original image bytes are not included in JSON; use `/api/export-full` for complete recovery with images.

Import validates all core and extension rows before visible data writes, then commits one atomic batch with its empty-target gate. The gate includes image metadata and all application data tables; session and rate-limit rows may already exist. An extension-write failure rolls back the core restore too, allowing a clean retry. Older backups without `betaWhatsAppMarks` restore with zero WhatsApp confirmations. Legacy v1.1 backups restore with zero progress/image entries while preserving their original IDs. ID registry, counters and history are retained; sessions and secrets are excluded. Legacy JSON import accepts up to 10 MiB and rejects image references. The streamed complete archive accepts up to 90 MiB; provider execution/batch limits also apply. Use supported import flows; do not bypass the clean-target check or force a merge with direct SQL.

### Manual WhatsApp contract

The **Report/Request** form's **Updates & messages** tab shares saved progress updates with the existing email workflow. **WhatsApp employee** opens **Manual WhatsApp draft**; its body is editable, but the recipient and identity/context fields are not. The server always resolves the employee from the request's stored Employee ID, never from a supplied name or free-form number. A missing/invalid profile Country or Contact number returns a correction-required error. The number must be full international format and pass `libphonenumber-js/max` validity and country-match checks; the displayed/captured recipient uses canonical `+` E.164 notation.

| Method and path | Input / result |
| --- | --- |
| GET /api/beta/requests/:requestId/updates/:updateId/whatsapp-draft | Authoritative recipient name/ID/phone, request/ticket/status/asset, saved update text/revision and signed context |
| POST /api/beta/requests/:requestId/updates/:updateId/whatsapp-open | `{context,message}`; revalidates the saved identity/context immediately before opening a manual external link; no sent log |
| POST /api/beta/requests/:requestId/updates/:updateId/mark-whatsapp-sent | `{context,message,clientKey}`; explicit staff-confirmed immutable log with exact edited message |
| GET /api/beta/requests/:requestId/whatsapp-log | Separate manual WhatsApp confirmations for that request, including archived requests |

These paths retain the historical `/api/beta/` namespace for compatibility on production, like email. They share session, Origin, CSRF and request/update-link validation. Archived requests cannot compose or confirm new sends. Saved confirmations remain readable. Each body is nonempty and at most 12,000 characters. The context authenticates raw request, employee and update state, including revisions and archive flags, with an HMAC. A changed source returns 409; reload the authoritative draft and require review. The mark insert predicates the source snapshot in the same atomic database operation to protect a race after validation. It does not accept a browser-selected recipient. Use a fresh unique `clientKey` for one confirmation and retain it for retries; a retry must not create duplicate marks.

**Open WhatsApp** validates first, then constructs a link to `https://wa.me/` using the full international number's digits without its `+` and URL-encoded message in the `text` parameter, as documented in WhatsApp's official click-to-chat guidance: https://faq.whatsapp.com/5913398998672934. The browser/user hands off the message and manually presses Send in WhatsApp or WhatsApp Web. Long URL fallback is **Copy message**, followed by **Open chat without message** and pasting. Keep the external destination origin fixed; do not permit a client-supplied host or executable URL. Do not send through server fetch, install provider secrets or infer number ownership/account availability from validation. A manual log does not prove send, delivery or reading.

**Mark WhatsApp as sent** is a separate, explicit confirmation. It preserves canonical phone, exact message, linked employee/request/update identities and update revision, actor and server timestamp. Later edits do not rewrite it. Existing email marks remain in their own table/list. There is no edit/delete endpoint. Privileged database owners retain storage control; shared session actor names remain self-reported.

The additive registered migration is `drizzle/0004_manual_whatsapp.sql`. Keep one complete DDL statement per line for hosted compatibility. Source/package updates must include this file and matching Drizzle journal/snapshot metadata. Complete export reads `betaWhatsAppMarks` in the same database batch as core, progress, email and image metadata; complete and legacy image-free JSON import validate and restore them in the same atomic batch and clean-target guard. The field is optional for older backups and defaults to an empty list. Never add WhatsApp logs in a separate post-restore transaction. No real employee messages need be sent during QA; check draft/link data and confirmation APIs with fictional records. External app, desktop browser and physical-phone handoff remain separate verification work.

### Device image contract and storage

Add/Edit device shows Device images under Additional notes. Selection stays local until an explicit upload. Saving ordinary form fields never silently uploads files. The upload action first saves valid device edits, then sends the returned permanent Asset ID, current revision and employeeId with its multipart batch. The server checks those values again in the atomic metadata transaction, so a concurrent reassignment/archival cannot mix image links. Uploaded originals remain on that Asset ID for its lifetime. Stored employeeId is the upload-time employee snapshot; current gallery assignment is read from the device's current employeeId. Do not rewrite historical metadata after a transfer.

Use a fresh 16-100 character clientKey for each new batch. Retrying the same files, order, names and employee/device identity with the same key returns the accepted batch rather than duplicates. Changing a reused key's batch returns 409. After an uncertain response, retain files and the key until a safe retry confirms the result; do not offer replacement as if nothing could have saved. Device revision may change without changing the retry fingerprint, but a first acceptance requires the current confirmed revision and employee link. There are no PATCH, DELETE or relink image routes.

Each file is 1-10,485,760 bytes; a batch contains at most six and is bounded to 60 MiB plus multipart overhead. Server signature checks determine allowed MIME/preview flags; filename extension is an additional allowlist requirement, never proof of type. Keep the shared extension list and server checks in step. Originals are not recompressed or stripped of EXIF metadata. A successful signature check is container identification, not a full decoder validation or malware scan.

Saved metadata contains image ID, permanent asset_id, upload employee_id, batch key/position, private object key, filename, MIME, size, SHA-256, preview flag, timestamp and self-reported actor. Image IDs use `IMG-` plus 32 uppercase hexadecimal digits; these are image identities, not replacement Asset IDs. Content reads verify byte count and hash before returning an original, use no-store/nosniff and a restrictive sandbox CSP. SVG/RAW/HEIC/TIFF and other non-previewable formats are attachments. Gallery metadata is capped at 10,000 returned images per query; use server employee/device filters for larger workspaces.

Hosted originals use the private `BUCKET` object-store adapter. The Node adapter uses `MEDIA_PATH` and create-only filesystem writes with restrictive file permissions. Keep MEDIA_PATH and DATABASE_PATH on persistent storage and outside public/. For the supplied container, use `DATABASE_PATH=/data/tracker.sqlite` and `MEDIA_PATH=/data/media` on the mounted /data volume; configure both explicitly. Retain both storage components during restart, upgrade, rollback and host transfer. Missing image storage returns a configuration error rather than putting uploads in public assets.

### Complete-media archive format

`/api/export-full` responds as `application/x-ndjson` with a .ttbackup download name. The first line is `{format:"team-equipment-full-backup",version:1,backup:...}`; core backup includes `deviceImageBatches` and `deviceImages`. Each subsequent original line contains `{id,base64}`; the final line is `{complete:true,imageCount}`. Metadata is snapshot-consistent and originals are immutable. Each original hash is verified during export; a missing/corrupt object aborts the stream without a valid trailer. Treat such a download as incomplete.

`/api/import-full` accepts the raw file with Content-Type `application/x-ndjson`, current code in `x-backup-code`, Origin and X-CSRF-Token plus the normal session cookie. It streams bounded lines, validates the exact manifest, filename/type, every size/hash, batch fingerprints and completion trailer, and stages originals under fresh private keys before one atomic core/metadata restore. Staging keys may differ on the target; logical image/device identities and bytes remain the same. A caught failure removes staged objects where possible; interrupted processes may leave private unreferenced objects. Never delete a referenced object as failed staging cleanup.

The 90 MiB archive cap includes base64 overhead. Export preflights the estimated complete archive size and rejects larger collections before starting the download, rather than offering an archive that the browser importer cannot restore. For larger direct moves, pause all writes, preserve the complete database and original object storage from the same boundary, map object keys when adapters differ and verify every referenced size/hash before cutover. A database-only copy cannot recover external originals. Do not split an archive and force multiple clean-target imports. Keep old storage private/read-only until verification is complete.

History cursors pair timestamp and event ID so equal timestamps can be paged consistently. Retain the returned nextCursor; do not build a timestamp-only paging loop.

## Exact portable setup

The project requires Node.js 24.x. Run from the source root:

```sh
npm install -g pnpm@11.25.0
pnpm install --frozen-lockfile
pnpm run portable:build
cd portable
```

A distributed prebuilt portable folder already contains bundled code and has no install step. In either case, run the runtime from inside that folder. It contains server.mjs, hash-code.mjs, public/, drizzle/, package.json and .env.example.

1. Copy `.env.example` to `.env` and keep it readable only by the service owner.
2. Run `npm run --silent code:hash`. Supply the chosen code on standard input and end input with Ctrl-D on macOS/Linux or Ctrl-Z then Enter on Windows. Use a private terminal; standard input is not a masked password prompt. Copy only the resulting PBKDF2 representation into ACCESS_CODE_HASH in `.env`.
3. Generate a long random SESSION_SECRET. For example, `node --input-type=module -e "import {randomBytes} from 'node:crypto';console.log(randomBytes(48).toString('hex'))"` prints a suitable new value. Copy it into `.env` and do not share it.
4. Set PUBLIC_ORIGIN to the exact HTTPS browser origin, DATABASE_PATH and MEDIA_PATH to persistent private writable storage, and the intended HOST/PORT.
5. Run `npm start` from the portable folder. The Node24 server loads `.env` and applies pending SQL migrations.
6. Configure the reverse proxy, TLS, process restart and private off-host backups. Test against the public origin before inviting the team.

Keep root paths when mounting the app: the default client/API/manifest URLs assume it is served at the origin root. Hosting below a path such as /tracker/ requires a deliberate URL/base-path change and fresh checks.

### Production reverse proxy

Leave HOST=127.0.0.1 when a reverse proxy runs on the same machine. Set HOST=0.0.0.0 only when a container/network policy requires it and direct untrusted access is blocked. Keep TRUST_PROXY=false unless your controlled proxy overwrites X-Forwarded-For with the validated client address. PUBLIC_ORIGIN must match the browser Origin header exactly, including any nondefault port.

For a container, build the supplied Dockerfile and mount a persistent volume at /data. Pass ACCESS_CODE_HASH, SESSION_SECRET and PUBLIC_ORIGIN as runtime secrets/environment values. The image runs as an unprivileged node user; its data volume must be writable by that user. Put it behind HTTPS and keep port 3000 inaccessible except through the intended proxy.

### Source checks and builds

```sh
pnpm run typecheck
pnpm run test:api
pnpm run test:catalog
pnpm run test:security
pnpm run test:frontend
pnpm run test:portable
node scripts/test-v13.mjs
pnpm run portable:build
pnpm run build
```

Run portable API smoke checks against a temporary SQLite file before migrating live data. The runtime uses migrations in drizzle/ and tracks applied files; do not reapply initialization SQL manually against an existing database. Add a new migration and matching Drizzle journal/snapshot metadata for schema evolution, then test upgrade/restore compatibility. The hosted importer requires one complete DDL statement per line. v1.3 adds three tables and four indexes and installs no triggers; authenticated API checks enforce immutable journal history and manual email confirmations.

### Catalog maintenance

Edit shared/catalog.json and retain provenance metadata described in shared/catalog-sources.md. Parent labels joined by a vertical bar must match exactly. shared/catalog.ts supplies options and validates the same combinations on client and server. Missing products use explicit Other / custom values plus exact-detail fields, not fabricated catalog matches. Test changes with `pnpm run test:catalog`. Preserve older labels or migrate affected records before removing them.

## Automatic creation and linked forms

The web editor debounces valid changes for approximately one second and offers Save now. Employee creation becomes eligible after complete name and start date. A device needs device type, brand and serial number. A request becomes eligible with employeeId plus either reason or details. Reason can issue a ticket before issue details are filled; a draft with details alone gets its ticket when reason is later chosen.

The first request save freezes employeeId and assetId, including an empty asset for a general issue. The paired Employee ID and Employee name selectors write one employeeId field. With one assigned active device, the form fills it automatically; with several, the user chooses. Do not add a second name-based foreign key or guess which of several same-name employees was intended.

The list view shows ten rows per client page; this does not remove the 10,000-record bootstrap cap. Refresh polling is not offline synchronization. Service-worker caching is restricted to public shell assets, logo and icons; employee/API data is not a public offline cache.

## Inactivity and restoration UI

The browser locks after 30 minutes without pointer, keyboard or touch activity and pauses background refresh while hidden. The server's separate inactivity clock measures API activity (30 minutes) with an absolute eight-hour session cap. Preserve both controls when changing polling or session handling.

Workspace settings offers Restore a full backup only for an empty record set. Users select a complete .ttbackup archive up to 90 MiB or an image-free legacy JSON file up to 10 MiB and confirm the current team code; the API independently enforces a truly clean database and validates the complete backup. A client-side empty view is not sufficient authorization to bypass the server-side clean-target check. Do not route an archive through the legacy JSON parser or log the restore-code header.

## Global search and appearance in 1.1

The top-bar search bar or magnifier opens server-side search; the compact mobile control opens the same dialog. GET /api/search accepts the following query parameters:

| Parameter | Values / behavior |
| --- | --- |
| q | Up to 200 characters; an empty query browses records/events |
| category | all, employees, systems, accessories, access, history or requests |
| field | all, name, employeeId, assetId, requestId, accessId, accessoryId, activityId or ticket |
| sort | relevance, newest, oldest, name or id; empty-query relevance uses newest |
| offset | Nonnegative integer offset |
| limit | Default 25, maximum 50; the UI requests 25 |

The UI waits for nonempty search text; clearing it returns to search guidance. The API independently permits empty-query browsing. The result is `{results,total,offset,limit}`. Results include archived records and all stored history, not merely the bootstrap's latest 300 events. Current and historical complete employee names from profile history resolve associated records and events. Names can be ambiguous, so display stable IDs. SQL values are bound and percent/underscore/backslash query characters are literal, not caller-defined LIKE patterns. Ordering has deterministic ID tie-breaks, and count/page reads use a consistent snapshot.

The UI opens an active record in its editor, an archived record in its history, and a history hit as an exact event with full before/after context. Use GET /api/records/:id or GET /api/history/:eventId rather than assuming a result is in the currently bootstrapped arrays. Every endpoint requires the existing code-authenticated session, returns no-store data and must not be cached by a service worker or browser persistence layer.

The navigation order is Overview, Employees, Equipment, Accessories, Access, Report/Request, Equipment gallery, Activity history. Brand/logo activation returns to Overview using the same pending-edits guard as other navigation. The upper-right Sun/Moon control replaces the former initials badge. It toggles light/dark appearance, defaults to the system preference and stores only the chosen theme in browser storage. It is independent of the optional activity label in Workspace settings and does not modify authentication or shared records.

When changing search or theme, review mobile dialog scrolling, keyboard focus/Escape behavior, result pagination, archived events, renamed employees, both themes and record navigation with pending edits. Report only completed QA, not assumed browser compatibility.


## v1.3 production change

Request progress update routes and backup handling are in `server/beta-updates.ts` (historical compatibility name). Apply the registered `drizzle/0002_beta_request_progress.sql` additively and preserve the existing production D1 database and access secrets. The migration adds three tables and four indexes; it installs no triggers. The authenticated API rejects modifications to preserved revisions and manual email confirmations. A privileged database administrator can still alter storage. The live tracker must be backed up and validated before deployment. Manual email only composes drafts and logs self-reported sending; there is no SMTP integration. Mark-sent requests must include the draft recipient and current progress revision. The server rejects a stale revision or a recipient that no longer matches the saved employee email, including changes that race the log write.

Before **Open email client** or **Copy message**, POST `/api/beta/requests/:requestId/updates/:updateId/email-open` with `{revision,recipient}` through the existing session/Origin/CSRF boundary. It checks the request, linked employee's saved email and selected update in one database snapshot and returns the authoritative recipient, employee ID and update revision without creating a confirmation. A 409 preserves draft text and requires refresh/review; never silently open the stale address. Email subjects for new confirmations must be a single line. This endpoint does not send mail or log a send.

New employee email writes and manual-email actions must accept one address only, rejecting control characters, comma/semicolon recipient lists and angle-bracket address syntax on both client and server. Preserve existing records and historical recipients. Recheck user activation, clipboard denial and configured-email-client handoff in actual browsers after the asynchronous server validation; VM event tests do not verify external application behavior.

Progress/email JSON requests are capped while reading at 72,000 UTF-8 bytes, then at the original 24,000 UTF-16 characters after decoding. Do not rely solely on Content-Length or call an unbounded text reader before checking limits. Malformed percent-encoded IDs return 400 consistently across common record, journal, image and WhatsApp routes.

The journal uses the original application session, CSRF token and session-epoch guards. Never issue a separate journal session or accept stale asynchronous results after logout, request changes or a newer sign-in. Pending edits combine original record fields with unsaved progress text, corrections and user-modified email drafts. Closing/navigation/search/sign-out use one discard check; an in-flight journal write blocks closing. Detail refresh/load-more changes guard corrections and modified drafts, and preserve text typed while a refresh was pending. Browser beforeunload guards journal drafts/writes, subject to browser prompt policy. Drafts are not persisted and are cleared on inactivity expiry.

The original Sites project is connected for this release, and Node.js 24 plus dependencies are available for checks. Publication is not complete until the deployment result and authenticated hosted API checks are recorded. Browser rendering and physical mobile-device testing remain unverified because the browser-control skill is unavailable; local HTTP API tests are supported.

## Device image deployment - 18 October 2026

This is an additive v1.3 update, not a database replacement. Retain the existing Sites project, D1 binding, access hash and session secret. Apply the registered `drizzle/0003_device_images.sql` migration using the supported host process; it adds two tables and three indexes without rewriting existing record, history or ID rows. Bind private object storage as BUCKET on the hosted adapter. The portable runtime applies pending migrations and uses MEDIA_PATH for originals. Source and portable download archives exclude real media, databases and secrets.

Rehearse with synthetic employees/devices in an isolated installation: six-file upload, seventh-file rejection, MIME/extension mismatch, private content access, immutable originals, unchanged Asset ID after transfer, employee provenance, stale assignment conflicts, retry after uncertain acceptance, .ttbackup round trip and incomplete-archive rejection. Run the existing API, progress, security, frontend, search and catalog suites, TypeScript checks and both builds. Check both themes and phone/desktop layouts where browser tooling is available. Record only completed checks in dated release evidence; this runbook is not evidence of live publication or production-data comparison.


## Machine-model catalog audit - 17 October 2026

`shared/model-catalog-audit.json` records manufacturer sources, all appended mappings, original retained model rows and before/after counts. `public/model-catalog.csv` is the human-readable download. To rebuild from reviewed lists, edit `shared/model-catalog-reviewed.json` and run `python3 scripts/refresh-model-catalog.py` (or pass additional reviewed JSON files); this starts from the included `shared/catalog-baseline.json` and retains its rows/metadata exactly. Do not generate model-number or processor/year permutations. Preserve valid brand and device-type parent labels. All 13 named brand options and both manual brand choices are checked.

Run `node scripts/test-model-catalog.mjs` plus the baseline catalog/frontend/API suites. The model list caches sorted choices only for valid immutable parent combinations and returns defensive array copies. Invalid API inputs do not create cache entries. Exact model is required for Other / custom. The current Add/Edit device form shows `exactModel` only when `model` is Other / custom, and `installedBuild` only when at least one of `os`, `edition` or `version` is Other / custom. Existing custom-detail text must not override these visibility rules or be cleared merely by rendering a hidden field. Retain the existing parent-change resets, API acceptance and stored values; this display change needs no schema migration. Exact processor and GPU visibility keep their existing behavior.

## Current Report/Request display labels - 18 October 2026

The production workflow is displayed as **Report/Request**, including navigation, add/edit actions, **Report/Request ID**, search labels, CSV headings and newly prepared employee message text. The exact module label is singular and has no spaces around the slash. Keep internal `requests` kinds, `requestId` fields, HTTP routes, SQL tables and original permanent IDs unchanged. Labels do not change employee/device links, backups, accepted history or earlier email/WhatsApp confirmations. This v1.3 update requires no migration. Update current interface documentation when labels change; retain dated release evidence as historical evidence.

## Security stress maintenance - 18 October 2026

The maintenance patch is a code-only update with no schema migration or record/media rewrite. New core-record values, progress text and email confirmation subjects reject ill-formed Unicode; existing reads, history and backup-text compatibility are retained. Future restores verify email-confirmation employee links and canonical progress-history timestamps before committing. Image manifests use maps keyed by image ID and batch key so size-limited restore validation does not repeatedly scan all images. Preserve every byte/hash/link check and the atomic clean-target boundary when optimizing. SVG container inspection uses bounded scans instead of a backtracking trailing-root expression; SVG remains an attachment. Portable HTTP streaming catches source-read errors and terminates the incomplete response rather than leaving a rejected stream promise unhandled.

Run `node scripts/test-stress-audit.mjs` with the existing API, security, frontend, search, progress, messaging, image, backup and portable checks. Use isolated temporary storage and fictional linked employees/devices for concurrent/rejected-write tests. Do not load-test production writes, send employee messages, rotate production secrets or alter uploaded originals to produce audit evidence. Review `docs/SECURITY_AUDIT_2026-10-18.md` for this audit's measured coverage and remaining limits. Dependency audit counts and CVE-wide forecasts do not establish a site-specific compromise probability.

## Regional and historical model review - 18 October 2026

The model review adds exact platform and regional codes from official Philippine and worldwide support, product and archival lists. Inputs remain in `shared/model-catalog-reviewed.json`; this review’s source snapshots, hashes, classification evidence and exclusions are summarized in `shared/model-source-audit-2026-10-18.json`. Public support inventories include peripherals and chassis infrastructure: require explicit PC-form evidence and retain the peripheral-exclusion regressions. Do not generate numeric ranges or infer every suffix combination. Full refresh preserves the CPU hierarchy; model input `reviewedOn` is a canonical ISO date, and metadata keys include source, note and date. Run model/processor/catalog/frontend/API checks and typechecking before packaging. Existing model labels and all non-model label/provenance tuples must remain unchanged. Source inventories cannot certify every regional ordering code ever released.

## Report/Request date label - 18 October 2026

The shared `requests` date field displays **Date reported** in Add/Edit forms and Activity history. Its stored key remains `dueDate` for compatibility with existing records, date values, raw CSV columns and backups. This is a display-label change; it does not backfill dates, change date validation or add scheduling behavior.

## Needs attention scope - 18 October 2026

The Overview counter uses only unarchived `requests` records with Priority `High` or `Urgent` and Status other than `Resolved` or `Closed`. Retain the existing Report/Request criteria: `Open`, `In progress`, `Waiting` and blank status can qualify; a nonblank ticket string is not an additional requirement. Each qualifying record contributes one, including separate reports for the same employee or device. All Equipment records, statuses and conditions are excluded. The card detail displays the priority report/request count and its navigation target is always `requests`, including at zero. This is a presentation/business-rule correction with no schema migration, stored-value rewrite or access change. Historical combined Equipment/request QA totals are dated evidence, not the current rule.
