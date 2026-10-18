# SinCityNotary - Requirements and practical limits

**Release:** 1.3

| Area | Behavior | Boundary |
| --- | --- | --- |
| New app | Separate responsive React app and database | Earlier app is not reused or synchronized |
| Access | Shared team code without username | Full access for code holders; actor label is self-reported |
| Global search | Authenticated search across saved records and all activity, including archived items | Online only; current/historical employee names resolve linked records; ambiguous names require ID checks |
| Search controls | Category and field filters; relevance/newest/oldest/name/ID sorting; 25 results per UI page | Query up to 200 characters; API limit at most 50 per page |
| Navigation | Overview, Employees, Equipment, Accessories, Access, Report/Request, Equipment gallery, Activity history | Logo/brand returns to Overview after pending edits are checked |
| Appearance | Sun/Moon light and dark toggle; default system preference | Only browser-specific theme preference is persisted |
| Employee ID | 15 characters after name and start date | EMP- + sequence 001-999 + start year + four random digits |
| Asset ID | Eight characters per physical device | Transfer retains device identity |
| Device storage | Primary, Secondary and Tertiary storage use the same combined capacity/type choices, such as 128GB SSD or 1TB HDD; compact GB/TB labels, numeric capacity order and all 360 options, including 128GB for each type/role; Add secondary/tertiary storage reveals the next missing role | Maximum three fixed roles per device; removing a role never promotes another; preserve legacy values and never guess an unknown type |
| Warranty Status | Optional per-device dropdown under Lifecycle and security in Add/Edit device: Expire/OOW, Current, Extended | No default or backfill; independent of Warranty end date; each device keeps its own saved value |
| Device images | Explicit upload under Additional notes in Add/Edit device; six images per batch, 10 MB per image | Draft replacement/removal only; accepted uploads have no edit/delete/relink action; further batches allowed |
| Image gallery | Employee and permanent Asset ID filters; current employee grouping and upload-time provenance | Archived devices remain visible; device reassignment retains images on the same Asset ID |
| Image formats | Validated image content and supported-format allowlist; original bytes retained | Browser-safe previews; download originals for SVG, RAW, TIFF, HEIC and unsupported browser formats; not arbitrary file uploads |
| Manual WhatsApp | Saved-update draft for the request-linked employee; editable body, fixed country-validated phone, explicit Open WhatsApp and separate Mark WhatsApp as sent | User presses Send in WhatsApp; no automatic send, SMS, delivery/read verification or recipient override; email logs remain separate |
| Report/Request ID | Eight characters per saved report | General reports/requests may omit a device |
| Ticket | Prefix plus five random digits, permanently reserved | 100,000 suffixes total across all reasons; no recycling; reason locks |
| Phones | Country-aware international validation | Does not prove ownership or reachability |
| Catalog | Dependent system lists | Broad curated selection, not every product ever made |
| Access checklist | Admin/User plus restrictions | Does not grant external access or tracker roles |
| History | IDs and change details for accepted saves | Not unsaved keystrokes; storage admins retain control |
| Validation | About eight-second notice plus field guidance | Invalid data is not saved |
| Collaboration | Revisions reject stale updates | Refresh and reconcile conflicts |
| Portability | Common API; Node24/SQLite alternative | Static/PHP-only hosting needs a compatible backend |
| Mobile | Responsive UI and PWA assets; scrollable short-screen search, wrapped long text, mobile navigation focus and larger touch controls | No compiled native app, push or offline sync; actual browser/device verification is separate |
| Recovery | Complete .ttbackup with records, registry, history, progress, separate email/WhatsApp confirmations and original images | Clean-target import, not merge; 90 MiB archive cap; image-free legacy JSON up to 10 MiB |

Release QA notes state what was actually verified. API/build checks alone do not prove browser behavior or native mobile packaging.

## Device image addition - 18 October 2026

This is an additive v1.3 feature. Uploading requires the saved device's permanent Asset ID, employee link and revision; the server must reject stale assignments rather than attach images to an unintended employee/device. Uploads require the existing authenticated session, origin and CSRF protections. Metadata and saved image bytes must be included in recovery planning; source code and CSV reports are not media backups. Refer to the dated release evidence for completed tests and deployment status.

## Capacity and output formats

The interactive bootstrap supports 10,000 records total. Global search reads saved database records and the full activity trail independently of the initial history window; search does not include unsaved edits. History is retained in the database, with the latest 300 loaded initially and further pages of up to 500 available through the history API. Fields accept up to 12,000 characters. Image-free legacy JSON import accepts up to 10 MiB; full .ttbackup import accepts up to 90 MiB including base64 overhead. Hosting transaction/execution limits also apply. Larger image collections require a coordinated database-plus-media transfer. CSV reports are Excel-compatible but are not native .xlsx workbooks. A JSON manifest with image references cannot recover originals and is rejected by the legacy importer.


## v1.3 production change

Request progress update routes and backup extension are in `server/beta-updates.ts` (historical compatibility name). Apply `drizzle/0002_beta_request_progress.sql` additively and preserve the existing production D1 database and access secrets. The live tracker must be backed up and validated before deployment. Manual email only composes drafts and logs self-reported sending; no SMTP integration.


## Manual WhatsApp addition - 18 October 2026

WhatsApp composes only from a saved request update for that request's linked employee. A missing/invalid saved country/phone must be corrected in Employees, not replaced in the draft. Each confirmation preserves recipient, content, request/update revision and server timestamp; a changed recipient or context requires a refreshed draft. Opening/copying is not sending and does not confirm a send. Manual confirmations are append-only through the authenticated application and separate from email confirmations. The user sends through WhatsApp or WhatsApp Web; the tracker does not verify account presence, reachability, delivery or reading. It adds no provider secret and no background sender. Full snapshots/restores must include WhatsApp rows in their atomic boundary, with absent rows in older backups treated as an empty list.

## Report/Request label update - 18 October 2026

The user-facing module is **Report/Request**. Its add/edit actions, ID label, search category, status labels, message drafts, CSV headings and current guide use that wording. This is a label-only v1.3 change: internal `requests` record kinds, `requestId` keys, API paths, table names, issued IDs, persisted records and historical message confirmations keep their existing identities. No SQL migration or data rewrite is required.

## Security and responsive maintenance - 18 October 2026

Progress/email JSON requests retain the 24,000-character limit and are additionally bounded to 72,000 UTF-8 bytes during streaming; oversize requests return 413. Malformed route encodings return 400. New core-record text, progress text and email confirmation subjects require complete Unicode characters; existing reads/history and backup-text compatibility are retained. New manual-email subjects must be single-line. Opening or copying a manual-email draft rechecks its saved employee recipient and progress revision; stale drafts require refresh and review without losing their text. Opening/copying still neither sends a message nor records a send.

Employee email input and manual-email actions accept one saved address, not a recipient list or header/display-name syntax. Unsafe existing profile addresses must be corrected manually before a new email handoff; historical confirmations are preserved. External email-client handoff and clipboard behavior require separate browser/device verification.

Future restores reject email-confirmation links to the wrong employee and invalid progress-history dates, while preserving the clean-target, atomic import rule. Indexed image-manifest validation, bounded malformed-SVG scanning and portable stream-error handling address resource and incomplete-download failures. The patch adds no migration, changes no shared-access setting and rewrites no existing records or image originals. Completed test coverage and residual risks belong in `docs/SECURITY_AUDIT_2026-10-18.md`; no test suite proves an exact probability of compromise or complete browser compatibility.

## Device storage update - 18 October 2026

In Add/Edit device, **Hardware specifications > Storage** offers **Primary storage**, **Secondary storage** and **Tertiary storage**, with the same capacity-and-type selections for each. Primary is always visible. **Add secondary storage**, then **Add tertiary storage**, reveals the next missing additional role, up to three roles total. Clearing/removing a role changes only that slot; existing tertiary data never becomes secondary. The labels record drive roles and do not configure the physical device.

New selections include capacity and type together, such as **128GB SSD**. No capacity has a space before GB or TB. All three roles expose the full 360-choice list, including **128GB** for every listed drive type; order capacities numerically from **1GB** to **24TB**, with GB capacities before TB capacities. Scrolling and filtering must reach every choice, not stop at the first 100. Search accepts either spaced or compact capacity text. Older capacity/type fields remain intact through unrelated edits and backup/restore; known saved values may be combined only for display, and an unknown type is explicitly shown as **type not specified**. No automatic conversion or database migration is performed. Device details, global search, CSV storage summaries and the current guide use the role-aware presentation. Raw export fields and permanent device/employee associations are preserved. Automated and build results must be recorded for the current change before production publication; they do not substitute for actual browser/device testing.
