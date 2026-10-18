# SinCityNotary - Security and backups

**Release:** 1.3 - manual WhatsApp, device images and warranty status, 18 October 2026

The tracker uses one shared team code without usernames. Every code holder has full access. An optional session label is self-reported, not verified identity. Admin/User in the Access checklist describes external application access; it does not restrict this tracker.

## Secrets and rotation

`ACCESS_CODE_HASH` holds the PBKDF2 hash representation of the code; generate it with the packaged helper. `SESSION_SECRET` is a long random secret used for protected session lookups. Use private environment variables/hosting secrets, not the repository. Never include the plain code, populated database, secret file or employee exports in the source ZIP.

Change the hash to change future sign-ins. Also rotate `SESSION_SECRET` to invalidate existing sessions. Restart/redeploy as the host requires and test access. Use HTTPS and sign out on shared devices.

## Operational limits

The API rejects edits to issued IDs and writes accepted record changes and their history events atomically. Saved progress revisions and separate manual email/WhatsApp confirmations have no edit/delete API and remain append-only through the authenticated application. Shared server validation and database uniqueness constraints enforce normal application writes; business rules are not installed as SQL trigger bodies. A fully privileged database owner can still edit or replace storage. Shared-code users cannot be individually authenticated from an optional display label. Named identities and separately controlled audit storage are future work if required.

## Device image boundary

Device images are private authenticated resources, not public source assets. Every current code holder can view and download them; there are no employee-specific permissions. Uploading uses the existing session, expected origin and CSRF protections. The server validates the saved device, its revision and employee assignment when committing the batch. A changed assignment must be reviewed rather than guessed from a name. Metadata preserves the permanent Asset ID and employee at upload, while gallery grouping follows the device's current employee.

Up to six images per batch and 10 MB per image are accepted. Validation inspects supported image signatures/content and rejects unsupported or arbitrary files, even if their extensions or browser MIME types claim to be images. Filenames must be treated as display metadata and safe download names, never as executable content or filesystem paths. These checks identify image containers; they do not constitute a malware scan or prove that every decoder can open a damaged image.

The authenticated API provides append-only uploads. There is no supported endpoint to replace, delete, edit or reassign an accepted image. Draft file selections remain changeable only before submission; uncertain retries retain the original batch identity. A privileged storage administrator can still alter underlying data, so this is application-level immutability, not a legal retention lock.

Use inline previews only for explicitly browser-safe raster types. SVG, TIFF, HEIC/HEIF, camera RAW and unsupported display types are downloads; SVG must not execute in the application origin. Return private image data with no-store and nosniff headers. Do not cache media in the service worker or persist selections/previews across sign-out. Original bytes may include EXIF location and other camera metadata: the app retains them rather than stripping or rewriting an uploaded image. Avoid uploading unnecessary sensitive information.

## Backups

Use the full **.ttbackup** archive for recovery with images: it streams the core snapshot, image metadata and original bytes with integrity hashes and a required completion trailer. Legacy JSON remains restorable only without image references. JSON containing image references is not a complete media backup and the importer rejects it. Spreadsheets remain reporting snapshots.

Keep dated private off-host copies and test restoration into a separate clean installation. Full export takes one transactional snapshot across core, progress, email/WhatsApp confirmations and image metadata tables, then streams its immutable referenced originals. Restore stages and checks all bytes before atomically publishing core data and image references; a failed restore must not expose partial records or images. Private unreferenced staged objects can remain after failure; maintainers may inspect these without deleting referenced uploads. The clean-target gate includes image metadata and all application data tables, while allowing existing session/rate-limit rows. Imports are not merges.

The streamed archive importer requires the current code in `x-backup-code` as well as the normal authenticated session, CSRF and Origin protections. It accepts up to 90 MiB (including base64 expansion), subject to provider limits. Never log this header. Legacy JSON imports retain a 10 MiB request limit. Larger collections need a coordinated host-level transfer of the complete database and private image storage with writes paused. Preserve compatible source/storage versions for rollback and verify hashes before switching users.

## Implemented controls

The session cookie is HttpOnly, SameSite=Strict and Secure over HTTPS. The browser locks after 30 minutes without pointer, keyboard or touch activity and stops background refresh while hidden; separately, the server expires a session after 30 minutes without API activity or eight hours total. Sign-out deletes that session. Login throttles repeated failures after 12 attempts per IP per 15-minute window. A successful sign-in resets the counter. Rate-limit keys are stored under a keyed IP hash. Office users sharing one public IP share this protection.

PBKDF2 uses SHA-256, 100,000 iterations, a random 16-byte salt and 32-byte digest. The stored representation is `pbkdf2-sha256$100000$<salt-hex>$<digest-hex>`. Generated session tokens are opaque random values; the database stores a keyed hash rather than their plain token. Mutating requests require the expected Origin and an X-CSRF-Token after sign-in.

In portable production, PUBLIC_ORIGIN must be HTTPS. HOST defaults to loopback. Only set TRUST_PROXY=true behind a trusted reverse proxy that overwrites X-Forwarded-For, and block untrusted direct connections to the Node listener. Enabling it behind an untrusted proxy would let clients influence rate-limit identification.

## Global search and browser preferences

Global search and exact-record/event fetches use the same authenticated session as every other record view. They include archived records and all activity, so every code holder retains full access to that historical information. Results use no-store responses and are not persisted as a client-side search cache. The browser stores only the chosen light/dark theme preference for this feature; no record, activity label, code or session secret is written alongside it. A theme change is not an access-control action.


## v1.3 production change

Request progress update routes and backup extension are in `server/beta-updates.ts` (historical compatibility name). Apply the registered `drizzle/0002_beta_request_progress.sql` additively and preserve the existing production D1 database and access secrets. It adds three tables and four indexes without SQL triggers; protection of journal history and manual email confirmations is enforced by the authenticated API. The live tracker must be backed up and validated before deployment. Manual email only composes drafts and logs self-reported sending; no SMTP integration. Journal requests share the original authenticated session, CSRF and session-epoch checks. Late responses from an older session cannot populate a newer session. Draft text is not persisted and is cleared when inactivity expires the session.

## Dependency and hosted-header maintenance - 18 October 2026

The full-lockfile advisory count fell from 62 (including one critical) to one high advisory after reviewed dependency updates. See `qa/dependency-security-2026-10-18.json` for the exact counts and remaining paths. Next.js 16.4.2 includes the [14 October security release](https://nextjs.org/blog/nextjs-security-update-october-14-2026); React server DOM 19.2.9 is beyond the [19.2.8 security patch](https://github.com/advisories/GHSA-wx67-qw84-cm4g). Vite and affected transitive tools are pinned to compatible patched versions.

[GHSA-vfj7-8cjw-p6xm](https://github.com/advisories/GHSA-vfj7-8cjw-p6xm) affects `braces` 3.0.3 and has no upstream patched version as of this review. It remains in trusted build/lint glob tooling through Vinext and ESLint. The tracker API uses bound SQL searches, not glob execution. This reachability assessment is a source review, not a claim that the audit is clean. Do not expose development servers or process untrusted glob patterns in build tooling; revisit when upstream provides a patch.

The hosted adapter now adds `X-Content-Type-Options: nosniff`, `Referrer-Policy: same-origin` and a camera/microphone/geolocation-denying Permissions Policy to its generated HTML/API responses. It preserves the platform's embedding and access policy. The portable runtime keeps its existing CSP and security headers. Private data responses remain `Cache-Control: no-store`. These changes do not rotate access secrets or alter any Site audience setting.


## Manual WhatsApp boundary - 18 October 2026

The tracker prepares click-to-chat links only; there is no WhatsApp Business API, SMS gateway, automatic outbound request, background sender or integration credential. Opening the external `https://wa.me/` link passes the linked employee's phone and prepared message to WhatsApp. Staff must review those details and press Send in WhatsApp themselves. Avoid confidential internal notes in outgoing drafts; clipboard copies and WhatsApp conversations are outside the tracker's storage/session boundary.

The server derives the recipient from the request's immutable Employee ID link and that employee's saved country-validated international Contact number. It must not trust a browser-supplied free-form recipient or another request's progress entry. A stale recipient, progress revision or request context requires refresh and review rather than logging to an unintended person. Missing or invalid saved contact data must be corrected on the same Employees record. Country validation cannot establish phone ownership, reachability or an active WhatsApp account.

WhatsApp confirmations are separate append-only, self-reported activity. Their recorded body, recipient, employee/request/update links, revision and time are confidential workspace data visible to every shared-code holder. Confirmation never proves sending, delivery or reading. Keep them in complete and legacy JSON snapshots/restores alongside the other tables within one atomic boundary; older backups without WhatsApp confirmations remain valid. Drafts clear on sign-out/session expiry, use existing pending-change guards and must not be persisted in service-worker caches or local storage. QA must not send to actual employees.

## Security and responsive maintenance - 18 October 2026

Progress/email JSON bodies retain their 24,000-character limit and are now bounded to 72,000 UTF-8 bytes while streaming, including requests without a trusted Content-Length. Oversized input is stopped with 413 before the whole body is buffered. Malformed percent-encoded route IDs return 400 rather than an internal error. New core-record values, progress text and email confirmation subjects reject incomplete Unicode characters; new email subjects must also stay on one line. These controls do not rewrite existing records or history, and backup text retains its prior compatibility rules.

New employee email values and manual-email actions require one address, rejecting recipient lists, control characters and address/header delimiters. Before opening an email client or copying an employee message, the client revalidates the saved recipient, permanent request association and progress revision with the server. A stale draft remains available for review, but opening/copying is blocked until it is refreshed. Mark as sent remains a separate confirmation. Existing historical recipients remain unchanged. External email/WhatsApp handoff after the asynchronous recheck remains browser/device-dependent and needs real-device verification; staff review the recipient and press Send themselves.

Backup validation rejects email confirmations linked to a different employee from their report/request and rejects impossible progress-history timestamps. Image restore builds ID and batch indexes instead of repeatedly scanning the full manifest. Malformed SVG inspection no longer retries a long trailing expression at every false opening tag; the existing image-size boundary and download-only SVG policy remain. Container checks still do not constitute a full SVG parser or malware scan. Portable response streaming handles read failures by aborting the response, so a truncated backup must still fail its completion/integrity checks.

Responsive fixes contain long saved text, keep search content reachable on short screens, enlarge touch controls and improve mobile navigation focus handling. Source and automated checks do not substitute for real browser, tablet or phone testing. See `docs/SECURITY_AUDIT_2026-10-18.md` for completed checks, scope and residual risk. No measured site-breach probability follows from a successful test run.

The fresh dependency audit still reports one high advisory in the full build/lint dependency tree and none in production dependencies. The remaining `braces` advisory and build-only reachability boundary above remain applicable; there is no upstream patched release at this review. Its CVE-wide exploitation forecast is not a probability that this tracker will be compromised.
