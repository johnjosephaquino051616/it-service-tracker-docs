# SinCityNotary Team Tracker
## User manual and hosting guide

A practical guide for employees, helpdesk staff and the person maintaining the app.

**Existing production address:** [private production address omitted]

**Edition:** 1.3 - storage capacity/type and drive roles, Report/Request, WhatsApp, device images and warranty status, 18 October 2026. Check the app footer for the edition and the release report for verification results. Existing records and permanent IDs are unchanged.

### Start in five steps

1. Open the app and enter the team code supplied by your administrator.
2. Add an employee's complete name and start date. A valid automatic save issues their permanent Employee ID.
3. Add the employee's physical device under Equipment. The app assigns its permanent Asset ID.
4. Record applications under Access and issued peripherals under Accessories.
5. Create a Report/Request for an issue. In **Edit Report/Request > Updates & messages**, save progress reports and open manual employee email or WhatsApp drafts.

### How the records fit together

An Employee ID connects a profile to its devices, access, accessories and reports/requests. An Asset ID belongs to one physical device. Each issue receives a Report/Request ID; selecting a reason also generates a permanent ticket/reference. Original Activity history records accepted record changes. The new report/request-level Updates journal saves separate progress entries with editable text and preserved prior versions.

Navigation is **Overview, Employees, Equipment, Accessories, Access, Report/Request, Equipment gallery, Activity history**. Click the logo or brand to return to Overview; pending edits are checked first. Use the **search bar or magnifier in the upper bar** for global search. On smaller screens, open the navigation menu. The form saves valid changes after about a second without typing. Wait for **All changes saved**, or choose **Save now**, before leaving. Device images use a separate, explicit upload action.

**Existing access code stays unchanged.** Preserve the production host secrets and database. Never import beta records into production.





<!-- page -->



# 1. Employees and contact details

### Create an employee

Choose Employees, then **Add employee**. Enter the complete name as **Last name, First name** and supply the employment **Start date**. Both are required before automatic saving creates the permanent ID. The form shows a name-format hint. Confirm both values before moving on.

The ID is exactly 15 characters: **EMP-00120260122** is an example. It combines EMP-, a three-digit sequence, the four-digit start year and four random digits. The stored ID does not change if a name or start date is later corrected. The three-digit sequence supports up to 999 employee IDs per installation; extend the format before that limit.

### Fill the profile

Enter one email address, country and contact number in their separate fields. Do not enter multiple email recipients or a display-name/address list. Address is free text beside the contact details. Position, department, manager, work location and employment status use the agreed dropdowns. Keep other context in Notes / requests.

Select the phone number's country first. Enter a plus sign, country code and digits, for example **+639766008239**. Spaces, a domestic leading zero after the country code, extensions and punctuation may be rejected. Validation uses country numbering metadata instead of one worldwide length. It cannot prove ownership or reachability.

### Find and update safely

In the global upper-bar search, enter a complete name or Employee ID to find the profile and its linked equipment, reports/requests, access, accessories and activity. Related forms offer names with IDs to distinguish people with similar names. Check the selected ID before saving. Related profile details come from that employee link; do not create another employee just to correct a spelling error.

Archiving preserves the employee's identity and history. Keep historical profiles instead of replacing one person's details with another person's details. A replacement hire receives a new employee record and ID.



<!-- page -->



# 2. Systems and accessories

### One record for each physical device

In **Equipment**, choose **Add device**. Select device type, make / brand and serial number, then link its employee. These core details must be valid before automatic saving creates the device. The app creates an eight-character Asset ID containing letters and numbers. Keep this ID throughout the device's life, including repair, reassignment and retirement.

Select the employee's name or enter their Employee ID to link the same profile. Confirm the employee details before saving. One employee may have several devices. A transfer changes the device assignment; it must not create a replacement Asset ID for the same machine.

### Record technical details

Use the dropdowns for device type, make, model, operating system, edition, version, architecture, processor and graphics. Add RAM, computer name, purchase date, warranty end date, condition, encryption and endpoint protection where known. Record each drive's capacity, type and role as described on the next page. Leave unknown values blank instead of guessing.

In **Add device** or **Edit device > Lifecycle and security**, choose **Warranty Status**: **Expire/OOW**, **Current** or **Extended**. It belongs only to this device and may be left blank. Select it manually; the separate **Warranty end date** does not calculate or change it. Existing devices remain blank until a status is saved.

Choose **Device type** and **Make / brand** before **Model**. For example, choose **Laptop > Acer**, search **A715-59G**, then select **Aspire A715-59G**. Searchable dropdowns search the full list and show up to 100 matches; type the exact model or processor code to narrow it. **Exact model / model number** appears only when **Model** is **Other / custom**; enter the full code from the device label. **Installed build / custom OS version** appears only when **Operating system**, **Edition** or **OS version** is **Other / custom**. These details are required for their matching custom selections.

For a CPU, choose **Processor manufacturer → Processor family → Processor** (exact SKU). Example: **Intel → Core i5 13th Gen → Core i5-13420H**. Manufacturers include **Intel** and **AMD**. The model and processor CSV references are downloadable under **Help & guide**; use **Processor reference by manufacturer & family** for CPU choices.

Dependent choices narrow after a parent is selected; changing a parent can clear incompatible child values. Merely hiding a custom-detail field does not erase its saved text. Operating system narrows edition and version. Architecture records the instruction platform, such as x64 or ARM64. Previously saved CPU labels, including older misclassified labels, remain available when editing that record; new selections use the corrected manufacturer/family mapping.

Model lists were reviewed against official Philippines and worldwide support sources, including exact platform/regional codes and historical products. They cannot guarantee every regional SKU, current stock, support or compatibility. For an unlisted item, choose **Other / custom**, enter its full code in the matching exact-detail field, and request a reviewed catalog entry. For a CPU, use **Exact processor (custom)**.


<!-- page -->


# 2A. Storage and peripherals

### Choose capacity and type together

In **Equipment > Add device** or **Edit device**, find **Hardware specifications > Storage**. **Primary storage** is always shown. Choose a complete value, such as **128GB SSD**, **512GB SSD** or **1TB HDD**. All three roles offer the same choices, from **1GB** to **24TB**, including **128GB** with every listed drive type. Capacity labels have no space before GB or TB. The full list scrolls from low to high by capacity, with GB before TB; type **128GB** to filter it. Leave a slot blank when no drive is recorded there; do not guess the physical drive's type.

### Add the second and third drives

Choose **Add secondary storage**, then **Add tertiary storage**, to reveal the next missing role. Select its own capacity and type. The device can record up to three roles, each belonging to that device's drives. Check the employee and permanent Asset ID before saving.

Clear the primary selection to remove its value. Secondary and tertiary rows also have a remove action. Removing or clearing a role affects only that slot: a tertiary drive remains tertiary when secondary is removed. Adding storage again offers the missing role without renumbering the others. These labels document the equipment and do not change its boot order, partitions or operating-system settings.

### Keep existing storage accurate

Older saved values are retained. A known saved capacity and type may be combined for display; a capacity without a known type is shown as **type not specified**. Viewing a record or saving an unrelated field does not convert its original storage values. Choose a new complete value only after checking the device. Compact labels appear in device details, history and search. Search accepts both **128GB** and **128 GB**. CSV keeps exact saved fields plus a compact summary; complete backups preserve the originals.

### Track peripherals

Accessories holds keyboards, mice, headsets, extra monitors, speakers, cameras, docks, chargers, adapters and other items. Link the employee and main device when relevant. Record quantity, serial number, condition, issue date and return date. Use separate records when serial numbers, condition or return dates differ.



<!-- page -->



# 2B. Device images and gallery

### Add images under Additional notes

1. Open **Equipment > Add device** or an existing device's **Edit device** form. Under **Additional notes**, find **Device images**.
2. Check the selected employee and permanent **Asset ID**. For a new device, fill the required device details first; the app must save its device record before attaching images.
3. Choose **Choose images** and select up to **six files in one batch**, each no larger than **10 MB**. You can replace or remove a selected file before uploading.
4. Review every file and the device identity, then choose **Upload images**. Keep the form open until the result confirms the saved images.
5. Saved images are permanent through the app: they cannot be replaced, edited, deleted or moved to another device. You can add further batches of up to six.

Selecting files is not an upload, and ordinary form autosave does not upload them. If an upload fails, keep the selected batch and retry. After an uncertain response, use the retry offered for that same batch instead of selecting replacements: the app checks whether the original upload was already accepted. Leaving or signing out discards unsent file selections.

### Formats and viewing

The uploader accepts supported image formats after checking the actual file contents, not just a renamed extension. Common photos, modern image files and many camera RAW formats are supported; it is not a promise to accept every image format ever created. PDFs, documents, videos and arbitrary files are not device images. A rejected file needs a supported image copy.

JPEG, PNG, GIF, WebP, AVIF and other supported browser-safe rasters may show a preview. Use **View image** to enlarge a preview. TIFF, HEIC/HEIF, SVG, RAW and formats the browser cannot display use **Download original**. SVG is kept as a download and is not executed inside the page. Originals are retained without app editing or recompression.

### Keep each employee's devices separate

Open **Equipment gallery** directly above **Activity history** in the navigation. Filter by **Employee** and **Device**; groups show the employee and each device's permanent Asset ID. Check IDs when names are similar. Images belong to that physical device, including after archival or reassignment. The gallery shows its current employee and, when different, **Employee at upload**, preserving the original association. Reassigning a device never copies its images to another Asset ID.

Every authorized code holder can view these images. Avoid unnecessary personal or confidential information. Use the complete backup archive in section 6 to preserve original image files as well as records.


<!-- page -->


# 3. Report/Request and tickets

### Report an issue

Open Report/Request and choose **Add Report/Request**. Select an employee by name or ID and verify the linked profile. Select their device before entering a reason or issue details. If exactly one device is assigned, it fills automatically; check it. Leave Device / Asset ID blank for a general issue.

Explain what happened, when it began, its effect on work and troubleshooting already tried. Choose Reason, Reported by and Owner. The last two fields select employee names. Owner handles the issue; Reported by raised it.

### Permanent IDs

Employee plus Reason or issue details triggers the first valid automatic save and issues an eight-character Report/Request ID containing letters and numbers. The employee and device links then lock. Check them before that first save; a wrong link requires archiving the report and creating another. Its selected physical device supplies the Asset ID; a report/request never creates another identity for that device. The server generates IDs once and prevents manual editing.

Choosing Reason creates the eight-character ticket/reference: a prefix and five random decimal digits. A draft with details but no reason receives a Report/Request ID first; adding Reason issues the ticket later. Leading zeros are allowed.

| Reason | Ticket example |
| --- | --- |
| Repair | REP25414 |
| Update | UPD02461 |
| Replacement | RPL83720 |
| Swap/Exchange | SWE41056 |
| Recycle | RCY10734 |
| Parts Out | PRO50681 |

These examples illustrate the format, not assigned tickets. The five-digit suffix is reserved across all reasons, so there are at most 100,000 ticket numbers in total. Generation stops rather than recycling a number; near capacity, random allocation may need a retry.

### Continue or open a new issue

Update status, priority, dates and notes as work progresses. In v1.3 use the separate **Updates & messages** section in Edit Report/Request for timestamped progress. Preserve the original report and notes. Reason locks once its ticket exists so the prefix cannot silently become misleading. Start a new report/request for a separate issue. Later profile changes do not regenerate an existing report's ID.



<!-- page -->



# 4. Access, history and notes

### A role-based access checklist

Create an Access record for each employee/application combination. Access role offers exactly **Admin** and **User**. Explain required access and restrictions. Record approver, approval date and provisioned date where relevant.

New Access forms show a suggested checklist based on the selected employee's position. Choose **Use this** to fill a suggested application and restrictions, then review them. Suggestions do not replace approval. This checklist documents permissions in other applications; it does not grant them. Follow the actual application's approval process, then update the tracker. Do not store passwords, recovery codes or private keys in notes.

### Automatic history

Accepted changes appear in **Activity history** as events with unique server-generated Event IDs, timestamps, record links and change details. Before and after values explain what changed. History is not an editable notes sheet.

Set the optional label under **Workspace settings > Your activity label**. It appears as the event actor. It is self-reported, not proof of a person's identity. Shared credentials cannot provide verified individual accountability. Timestamps originate on the server; the interface may show local browser time.

Each accepted automatic save or **Save now** is the transaction boundary. Unsaved keystrokes are not permanent database revisions. Rejected input is not recorded as an accepted change. Save a Notes / requests entry for general context. Report/Request-specific progress uses the separate v1.3 Updates journal, which preserves corrections and their earlier versions.

### Useful notes

Example: "15 Oct - Restarts on battery. Tested another charger. Awaiting battery replacement; owner Jessica."

Include symptom, action, result and next step. Only record personal information needed for IT support. Notes are open fields, but they are visible to every code holder.

### Archive rather than overwrite

Archive old records to retain their IDs and history. Restore them if work resumes. Permanent IDs and history must remain with the database during a move. A fully privileged database administrator can bypass application controls, so protect storage access.



<!-- page -->



# 5. Search, mobile and appearance

### Find anything in one place

1. Use the **search bar or magnifier in the upper bar**. Type an employee name, other saved text or a permanent ID. The mobile control opens the same search dialog.
2. Narrow by category: Employees, Equipment, Accessories, Access, Activity history or Report/Request. Keep All categories to find connected information across the workspace.
3. Under **Search field**, choose All searchable fields, Employee name, Employee ID, Equipment / Asset ID, Report/Request ID, Access ID, Accessory ID, Activity / Event ID or Ticket / reference.
4. Under **Sort by**, choose Most relevant, Newest first, Oldest first, Name / title (A-Z) or ID (A-Z). Move through pages of **25 results**. Enter search text to show results; clearing it returns to the search help.

An employee name can find the employee and everything linked to that identity, including records associated with a previous name. Search covers saved active and archived records and the complete activity trail, including older events outside the initial history view. It runs on the authenticated server, requires internet access and does not search unsaved typing or a browser-only cache.

Select an active result to open its record. An archived result opens its history. An activity result shows that exact event with its full before/after details. Similar names may match several employees; check the permanent IDs.

### Light, dark and mobile

Use the **Sun/Moon button in the upper-right corner** to switch appearance. Initially the app follows the device's system preference. A chosen theme is remembered only in that browser; it does not change records or another person's view.

On phones and tablets, use the menu button to open navigation; Escape or the close button returns to the page. Long search results and forms scroll on short screens, and long saved values wrap. Use Install app or Add to Home Screen where supported. This is a web app without offline record synchronization or a compiled native mobile app. Wait for **All changes saved** before closing or leaving.

### Validation and conflicting edits

Invalid input produces an eight-second floating warning; field guidance remains after it disappears. Correct the problem and retry. Rejected input is not saved. If another user saves first, refresh the record and reconcile the latest values instead of overwriting their work. Server validation protects IDs and locked references even when browser controls are bypassed.



<!-- page -->



# 6. Exports, backups and moving data

### Choose the right export

Choose **Export CSV** in a register for an Excel-compatible report. CSV exports are snapshots for review and reporting. Treat them as confidential. Editing an export does not update the live app.

Choose **Workspace settings > Download complete backup** for a **.ttbackup archive** with original images, metadata, records, history, permanent ID reservations, progress and separate email/WhatsApp confirmations. Wait for completion and keep the entire file. Integrity checks and a completion marker reject truncated or modified files. The next page covers size limits and larger transfers.

Image-free legacy JSON backups up to 10 MB remain accepted. JSON with image references cannot recover originals and is rejected. Older backups without WhatsApp logs restore with no WhatsApp confirmations; v1.1 backups also lack progress and images. Source code and employee lists are not full backups.

### A practical routine

1. Export a full backup after setup and before upgrades or bulk changes.
2. Store it privately with access limited to the responsible team.
3. Keep dated copies and an off-host copy. Source code is not a data backup.
4. Restore a copy into a test installation regularly. Check linked records, history and ID reservations.
5. Limit production database access to trusted maintainers.

### Move to another host

Configure fresh target secrets and stop source writes for the final backup. On the empty target, choose **Workspace settings > Restore a full backup**, select the .ttbackup file, re-enter the team code and choose **Restore to empty workspace**. Import requires a **clean target database**, including progress, email/WhatsApp logs and image metadata; it is not a merge. The server validates the archive and commits all data atomically. Failed restores expose no partial records or images. Verify totals, links and originals before switching URLs; keep the source read-only until confirmed.

The clean-target rule prevents two unrelated installations and ID registries being mixed. Do not restore an old snapshot over ongoing work without a planned recovery. Bookmarks and installed web-app shortcuts may need the new URL.

### Source and data are separate

The handoff includes code, assets, schema, configuration examples and documentation. It excludes secrets, employee data and uploaded images. A developer needs the source and a separate approved complete backup to reproduce current records.


<!-- page -->


# 6A. Image backup and host transfer

### Preserve a complete archive

The **.ttbackup** format streams the workspace snapshot and every original image into one file. It preserves each image's permanent Asset ID, employee at upload, filename, media type, size and integrity hash. An image's original bytes remain unchanged. Keep the archive private: it includes all employee data and images accessible to the shared-code users.

The browser restore supports archives up to **90 MiB** (94,371,840 bytes), subject to the destination host's execution limits. Base64 encoding increases image data size inside the archive, so a collection of originals below 90 MiB can still exceed that limit. A single image has a separate **10 MiB** limit and each upload batch holds at most six files. The number of later batches is not limited to six images total.

### Rehearse recovery

1. Download a fresh complete archive, store a dated private off-host copy and preserve the matching source release.
2. Prepare a secured empty test installation using current migrations and a private image-storage adapter. Do not put uploaded images in public assets.
3. Restore the .ttbackup file through Workspace settings with the target installation's current team code.
4. Check employee/device counts, permanent IDs, history, report/request progress, email/WhatsApp confirmations and image counts. Download sample originals and compare their hashes with the backup.
5. Check Gallery groups, employee filters, archived devices and a device's upload-time employee provenance. Reassignment must retain images on the original Asset ID.

Restore checks file contents, sizes, hashes and relationships before publishing records and image references. Failed staging can leave private unreferenced storage objects for a maintainer to inspect; those objects are not visible in the app. Do not reuse a partial archive or bypass the clean-target rule.

### Larger collections and direct transfers

When a collection would exceed the 90 MiB archive limit, the download is rejected before it begins. Arrange a host-level transfer instead of splitting or editing an archive. Pause writes and copy the complete database plus its private image storage from the same point in time. A database alone can contain image references without the original bytes. Preserve storage keys and verify every recorded image size and SHA-256 hash before switching users.

Hosted and portable deployments use different storage adapters. Use the supported full archive when it fits, or have a maintainer map storage keys and move both components explicitly. See **docs/DEVELOPER.md** for adapter details. Keep the old deployment until records and images are verified on the destination.



<!-- page -->



# 7. Shared-code security

### Access boundary

The team code is checked on the server. It is not embedded in public JavaScript or this guide. No username is required. Successful sign-in creates a protected session cookie. Use HTTPS and sign out on shared computers. The browser locks after 30 minutes without pointer, keyboard or touch activity and pauses background refresh while hidden; separately, the server ends sessions after 30 minutes without API activity or eight hours total. Sign in again when prompted.

Every code holder has full tracker access, including employee details and exports. Anyone receiving the code can pass it on. Share it only with authorized users through a private channel. There are no separate viewer and administrator accounts in this release.

### Server secrets

**ACCESS_CODE_HASH** is a PBKDF2 hash representation of the code. **SESSION_SECRET** protects server-side session lookups and must be a long random value. Put both in hosting secrets or private environment variables. Never include their values in source, support tickets or the handoff ZIP.

Use the supplied helper to generate the hash without putting the plain code in source files. Give each independent installation its own session secret.

### Rotate and revoke

If the code is disclosed, generate a replacement hash and update the host secret. Deploy or restart as required. This controls future sign-ins. Also rotate SESSION_SECRET to invalidate existing sessions. Test access before telling the team the new code.

Rate limiting blocks repeated failures after 12 attempts per IP address in a 15-minute window. A successful sign-in resets the attempt counter. Office users sharing a public IP share this protection. Same-origin and session protections reduce common browser attacks; they do not turn shared credentials into named accounts. Protect hosting access, database files, backups and the maintainer's computer.

### Audit limits

The API reserves IDs and rejects ordinary edits to them. History is append-only through the API. A fully privileged storage administrator can still modify or replace the database. Stronger tamper resistance or verified personal auditing requires named identities and separately controlled audit storage.



<!-- page -->



# 8. Hosting and deployment

### Compatible hosting

The portable edition runs on **Node.js 24** with a persistent writable disk, using SQLite through node:sqlite. A VPS, container host with a volume or suitable managed Node host can run it. The Dockerfile provides another starting point.

The hosted edition uses the common API with Cloudflare D1. The Node edition uses a SQLite adapter and independent bundled browser build. Neither depends on the previous tracker. Databases on different deployments do not automatically synchronize.

Static-only hosting cannot run the backend. PHP-only hosting needs a compatible Node backend elsewhere or an explicit server rewrite. Confirm runtime, disk persistence, HTTPS and process management before moving.

### Production setup

1. Install dependencies from the supplied lockfile using the documented package manager.
2. Generate ACCESS_CODE_HASH and a random SESSION_SECRET locally.
3. Set persistent database and private image-storage paths plus the listener address. Keep data outside public assets.
4. Build the portable client and start the Node process using package scripts.
5. Put an HTTPS reverse proxy in front and restrict database access.
6. Import only into a clean target. Test sign-in, records, links, export and sign-out.
7. Configure process restart, off-host backups and error monitoring.

### Current hosted service

Keep the hosted wrapper and D1 binding for the current platform. Platform sharing settings are separate from the app's code gate. Changing the team code does not change those settings. Preserve intended platform access during deployment.

See **docs/DEVELOPER.md**, package.json and .env.example for exact commands and supported environment names. They are maintained with the source release.



<!-- page -->



# 9. Modifying and maintaining

### Focused changes

Use **shared/fields.ts** for labels, option lists, hints, order and form fields. Update shared/types.ts when contracts change. Server validation is separate; changing a form alone does not change a business rule.

For a new field, update form configuration and server validation. Review cards, search, exports, backup/import and tests. Generic record payloads can support additive fields without a physical SQL column. Changes to SQL structure, indexes or constraints need a migration. Decide how historical records and older backups will be handled before removing a field.

Use **shared/role-guide.ts** to change role-based access suggestions. For hardware or OS choices, edit **shared/catalog.json** and retain the provenance in shared/catalog-sources.md. Preserve parent-child links. Test valid and incompatible combinations. Do not rename permanent IDs because a display label changes.

### Release sequence

1. Back up current data and preserve the source release.
2. Change a test installation with non-production records.
3. Run automated checks and both production builds from the developer handoff.
4. Check phone, tablet and desktop views in real browsers, plus keyboard use and long text.
5. Confirm secrets and database path, then deploy.
6. Test a linked employee, device and report/request flow, history and exports.
7. Roll back compatible code and data deliberately if needed. Do not overwrite newer work with an old database blindly.

### Record what was verified

A successful build does not prove every device, browser or host works. Keep release QA notes with completed checks and remaining limits. Use fictional records in a separate installation for stress tests; preserve live access, records and image originals. The current interface supports up to 10,000 records across sections; larger workspaces need paginated views. A text field accepts up to 12,000 characters.

Consider named accounts when teams need different permissions. Plan a wider ID format before sequence limits. Add offline use only with a defined conflict and synchronization design. App-store publishing requires a separate mobile build and review process.



<!-- page -->



# 10. Report/Request progress updates

### Add an update in Edit Report/Request

1. Open **Report/Request**, find the report/request by employee name or permanent Report/Request ID, and open **Edit Report/Request**.
2. Select the **Updates & messages** tab. Alternatively, scroll to **Progress updates & employee messages** in the report/request form.
3. Type the new troubleshooting action or outcome under **Add progress update**. Include a short status, action taken, results and next step, for example: **GPU upgrade approved; higher-tier card ordered; waiting for shipment**.
4. Choose **Save update**. The entry is stored independently from the general **Additional notes** and shown in **Saved update history** with its saved time and activity label.
5. Add a new entry for the next action. Do not overwrite earlier updates merely to record new progress.

The journal stays attached to the permanent Report/Request ID (fictional example: **R7X2A9B4**). Use fictional records when testing.

### Review, correct and audit

Select **History** on an update to read each saved version, most recent first. If you need to fix inaccurate text, select **Edit**, make the change and choose **Save correction**. The corrected version becomes current; the earlier version stays in its numbered **History**. You cannot erase old versions using the application. A team member can add additional updates as work progresses.

### Unsaved updates and drafts

Progress text, corrections and modified email or WhatsApp drafts are not saved automatically. Save updates and corrections explicitly. Leaving a report/request checks its unsaved fields and journal drafts together; pending journal writes must finish first. Refreshing the journal asks before discarding a correction or modified email/WhatsApp draft.

Browser reload warnings depend on the browser. Drafts stay only in the current page and clear when the session locks. Save before leaving. An unchanged message preview does not count as an edited draft.

A saved entry includes a time from the server. The displayed activity label is self-reported because the tracker uses a shared team access code, not personally verified accounts. If two testers edit the same version, the server rejects the second stale edit. Refresh to review the newest version rather than overwriting another change.

### Keeping the existing fields unchanged

Progress entries preserve the original report/request fields, permanent IDs and **Additional notes**. Archived reports/requests remain readable; restore them before adding or correcting updates.



<!-- page -->



# 11. Manual employee email (NEW)

### Compose from a saved update

1. Check that the linked employee profile has a correct **Email** address under Employees.
2. In **Report/Request > Edit Report/Request > Updates & messages**, choose **Email employee** on the progress entry to share.
3. Review the automatically selected recipient, suggested subject and editable email message. The message includes the Report/Request ID, available ticket and report/request status, and the saved progress text.
4. Choose **Open email client** to open a draft in your configured email application, or **Copy message** to paste into Gmail, Outlook or another program. The tracker rechecks the saved recipient and progress revision first.
5. Review the recipient and message and manually press **Send** inside your email application. The tracker cannot and does not send messages itself.
6. After sending, return to the tracker and choose **Mark as sent**, then confirm. This creates an internal manual log with recipient, subject, update revision, time and activity label.

**Important:** Clicking Open email client or Copy message does not send email or log delivery. Mark as sent only records a team member's statement; it is **not a delivery receipt**. The tracker cannot know whether the email was delivered or opened. Do not share confidential internal IT details in employee-facing emails.

### Reviewing confirmations

The **Manual email confirmations** section shows messages marked sent. Later corrections do not change the logged update revision; sent-message evidence stays in the email system. Correct a missing address in the saved employee profile. If the saved address or progress revision changes, opening, copying or marking the draft as sent is blocked. Your draft text stays available; refresh and review the recipient and message before retrying. Keep the subject on one line.

### Limitations of manual mode

This release requires a configured email application to open `mailto:` drafts; browser-only email setups may need **Copy message**. Nothing runs in the background; there is no SMTP, auto-notification, delivery verification, or scheduled email. Consider these only in a separately tested future release after email authorization and delivery rules are agreed.



<!-- page -->


# 11A. Manual employee WhatsApp

### Prepare an update for the linked employee

1. Under **Employees**, confirm the employee's **Country** and **Contact number**. Use a valid international number, including its plus sign and country code. The employee must use that number on WhatsApp.
2. Open **Report/Request > Edit Report/Request > Updates & messages** and select **WhatsApp employee** on a saved progress update.
3. Check the recipient's name, permanent **Employee ID**, phone number, **Report/Request ID**, ticket and device **Asset ID** when present. The recipient is taken from this report/request's linked employee; you cannot enter or switch recipients in the draft.
4. Review the prepared, editable message. It includes the report/request reference, current status and selected saved update. Share only the details appropriate for that employee.
5. Choose **Open WhatsApp**. WhatsApp or WhatsApp Web opens a prepared conversation; review its recipient and message, then press **Send** there yourself. **Copy message** is available as a fallback. For a long message, copy it, choose **Open chat without message** and paste it in WhatsApp.
6. After you send it, return to the tracker, choose **Mark WhatsApp as sent**, and confirm. This records a separate internal WhatsApp confirmation; it never sends a message.

Opening or copying a draft does not create a sent confirmation. A confirmation is your statement only: the tracker cannot verify sending, delivery, reading or whether the number has a WhatsApp account. Regular carrier SMS is a separate service and is not included.

### Keep the correct employee and report/request together

If the contact is missing or invalid, correct **Country / Contact number** on the linked employee's profile, then reopen the draft. Do not substitute another employee's number. If the saved phone, selected update or report/request context changes while a draft is open, choose **Refresh draft** and review the new draft before opening or confirming it. Archived reports/requests can be read; restore them before composing or confirming messages.

**Manual WhatsApp confirmations** are shown separately from **Manual email confirmations**. Each retains the recipient, exact message, linked report/request/update revision, time and self-reported activity label. Later profile or update corrections do not rewrite the old confirmation. Shared-code users can view these records, including saved phone numbers and message text.

### Manual handoff and testing limits

WhatsApp requires internet access and an appropriate WhatsApp app or WhatsApp Web session. Opening the prepared link shares its recipient and message with WhatsApp; keep confidential internal notes out of employee-facing drafts. The tracker has no WhatsApp API, SMS provider, background sender, integration credentials or delivery tracking.

Modified drafts stay only in the current page and are checked before leaving. They clear on session expiry or sign-out. For QA, use fictional records and check drafts without sending to real employees. Automated data checks do not verify the external WhatsApp app or physical-phone handoff; see the dated release report for completed checks.


<!-- page -->


# 12. Needs attention dashboard (v1.3)

### What is counted

**Needs attention** counts saved priority records from **Report/Request** only. Equipment status and condition do not contribute.

- Each unarchived Report/Request marked **High** or **Urgent** priority with a status other than **Resolved** or **Closed** counts as one.
- The counter includes High or Urgent reports/requests in **Open**, **In progress**, and **Waiting** status. Records with no status are treated as open.
- Archived, closed or resolved reports/requests never count. Low and Normal priority records do not count.

If a device is **Needs repair** and its unresolved report/request is **Urgent**, the dashboard shows **1**. A device in the same condition without a qualifying report/request adds **0**. Two qualifying reports/requests count separately, even if they concern the same device or employee.

### Card details and troubleshooting

The card's subline shows the priority report/request count. Selecting it always opens **Report/Request**, including when the count is zero.

After a newly published version, refresh the browser (**Ctrl+Shift+R** on most Windows browsers) to discard older pages. The footer should display **v1.3**, not the previous Beta or v1.1 label.

If the count is still wrong, check the report/request's saved Status, Priority and archive state. Unsaved edits do not affect the counter. Changing an Equipment condition does not change this count; update the Report/Request record as work progresses.


<!-- page -->


# 13. Upgrading the production workspace

### Preserve data before deployment

1. Arrange a short maintenance window and pause edits for the final recovery snapshot.
2. Choose **Workspace settings > Download complete backup** and keep a private dated **.ttbackup** archive with its matching source release. It includes original images, records, permanent IDs, history, report/request progress and message confirmations. An image-reference JSON manifest alone cannot recover uploaded originals.
3. Take a database-native snapshot and preserve private object storage too, if your host supports them. Never restore an old snapshot into populated production as part of a normal code upgrade.
4. Rehearse with fictional records or an authorized secured test copy. Test employee, device and report/request links, image originals, warranty values and separate email/WhatsApp confirmations. Compare the preserved data within your authorized access.

### Apply additive changes to the same installation

Use the original production project, existing D1 database and private image-storage binding. Keep the production URL, shared code, **ACCESS_CODE_HASH** and **SESSION_SECRET**. Do not substitute beta records, secrets or databases.

Run pending registered migrations in order using the actual host's migration process: report/request progress **0002**, device images **0003**, then manual WhatsApp **0004**. The latest adds a separate WhatsApp confirmation table; it does not rewrite employee, device, report/request or email data. The detailed maintainer procedure and exact filenames are in **docs/PRODUCTION_UPGRADE_V1.3.md**. Warranty Status is a payload field and requires no SQL migration.

### Verify the release

Check sign-in, protected data access, original permanent IDs, employee/device links, history, gallery, warranty selections, search, reports and complete backup restoration in an isolated clean target. Confirm the **v1.3** footer and current manual/downloads. Verify the **Updates & messages** tab and separate email/WhatsApp drafts and confirmation lists. For QA, inspect fictional drafts without sending messages to real employees.

Record the actual API, build and browser checks in the dated release report. Automated data tests do not prove that an external email application or WhatsApp handoff works on a physical phone. A manual sent confirmation never proves delivery or reading. Resume team access only after reviewing the checks and remaining limits.

### Roll back without losing accepted work

Preserve current records, image storage and new message logs during a compatible code rollback. Do not overwrite newer work with a stale database export or remove the new tables. Older export code may omit WhatsApp confirmations: retain a complete current-version recovery archive. Keep the source private and read-only until recovery or host transfer has been verified.
