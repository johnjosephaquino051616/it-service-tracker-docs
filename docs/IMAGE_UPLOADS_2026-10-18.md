# Device image update - 18 October 2026

This additive v1.3 update adds image evidence under **Equipment > Add device / Edit device > Additional notes**. The new **Gallery** follows Activity history in navigation, preserving the existing Equipment, Accessories, Access and Requests order.

## Resulting behavior

- Select up to six supported images per batch, at most 10 MiB each. More batches are allowed.
- Replace or remove selected files before upload. Explicit upload saves valid device changes first and uses its returned permanent Asset ID and employee assignment.
- Accepted originals have no application edit, delete, replace or move route. New batches append new immutable images.
- Galleries group by permanent employee/device IDs, never by names, make or model. Reassignment keeps images with their physical device and retains the employee-at-upload provenance.
- Recognized image formats use a shared extension list plus server content signatures. Browser-safe raster formats preview inline; unsupported previews and SVG use original downloads. This does not claim support for every image format ever created.
- Full `.ttbackup` archives contain records, IDs, history, request progress, image metadata and original bytes. Legacy metadata JSON cannot silently restore an image-bearing workspace without originals. An archive over 90 MiB requires a database and private-image-storage migration; preflight rejects it before offering an un-restorable download.

## Preservation and security

Migration `0003_device_images.sql` creates two image tables and four indexes. It contains no existing-record updates, deletes or table alterations. The rehearsal applies it twice to synthetic data and compares all pre-existing table rows unchanged. Original migration files, permanent IDs, shared-code access settings and existing records are retained by the implementation.

Upload requests use the existing session, origin and CSRF checks. Reads and original downloads require an authenticated session. Device revision and employee links are checked again inside the metadata transaction. Create-only private object storage, retry fingerprints and batch keys prevent accidental replacement and duplicate retries. SVG is download-only with a sandbox response policy. Database/storage owners retain administrative access; immutability applies to ordinary app/API access.

Hosted storage uses `BUCKET`; portable storage uses `MEDIA_PATH`, defaulting to `./data/media`. Docker/Compose set `/data/media` within the persistent data volume. Neither originals nor private exports enter source/download packages.

## Verification

| Executed check | Passed |
| --- | ---: |
| Image upload/storage/format checks | 11 |
| Independent image, concurrency and failed-restore checks | 15 |
| Copied portable image HTTP, restart and exact-byte recovery | 10 |
| Image migration preservation rehearsal | 3 |
| D1 migration formatting and retry compatibility | 4 |
| Core records API | 25 |
| Existing security and transaction boundaries | 32 |
| Request progress, backups and dashboard | 13 |
| Frontend logic and rendered component controls | 106 |
| Search and additional search/session checks | 32 |
| Copied portable HTTP compatibility | 17 |

All 268 recorded cases/checks passed. TypeScript checking passed. These tests use synthetic data, disposable databases and fake credentials. The manual was rebuilt and all 16 pages visually reviewed. Hosted/portable production builds are part of final packaging.

Interactive desktop/mobile browser and physical-phone behavior remain unverified because the supported browser-control capability is unavailable. Rendered component checks do not establish interactive browser behavior.

Automatic approval review blocked a full production export because it includes private employee/device data and live credentials. No alternate credential route or private live export was attempted. Production schema inspection is available; a full before/after live-data comparison was not performed. Do not interpret local preservation tests as a verified production data comparison.

Publication results belong to the release handoff; this source note records implementation and local verification, not a completed deployment.
