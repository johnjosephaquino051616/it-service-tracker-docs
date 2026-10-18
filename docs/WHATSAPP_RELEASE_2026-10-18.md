# Manual WhatsApp update - 18 October 2026

This v1.3 addition places **WhatsApp employee** beside email on each saved request progress update under **Requests > Edit request > Updates & messages**.

## Behavior

The server reads the request, its linked employee and the selected saved update together. The draft shows the employee name, permanent Employee ID, validated saved international number, Request ID, ticket, device Asset ID when present, and saved request status. The recipient cannot be edited in the draft. The message can be reviewed and edited before opening WhatsApp.

Opening WhatsApp requires a fresh context check. A changed phone, country, employee profile, request or update rejects the old context and requires a refreshed draft. The browser opens a fixed `https://wa.me/` URL with encoded message text, in a separate tab with no opener/referrer access. Oversized links offer a copy-and-open-chat fallback instead of silent truncation. The user presses Send in WhatsApp.

**Mark WhatsApp as sent** is a separate, explicit staff confirmation. It records the exact draft, recipient, employee/request/update identities, update revision, activity label and timestamp. Retry keys prevent duplicate confirmations. Confirmation rows cannot be edited or deleted through the app. Opening/copying a draft does not create a confirmation. Email and WhatsApp counts and history remain separate.

The tracker does not contact WhatsApp from its server, verify account registration, or verify actual sending, delivery or reading. The number must belong to the linked employee and be registered with WhatsApp. Validate real-world recipient ownership before sending.

## Storage, upgrades and portability

Migration `0004_manual_whatsapp.sql` creates one new confirmation table and two indexes, with generated Drizzle metadata and one complete idempotent DDL statement per line. Existing employee, equipment, request, email, image and permanent-ID tables are not rewritten. The production project identity, access settings and existing storage bindings remain the same.

Legacy metadata JSON and complete `.ttbackup` archives include optional `betaWhatsAppMarks`. Restore validates links to the request's permanent employee and an existing saved update revision, then commits all records and extensions together. Earlier backups without WhatsApp marks remain accepted. The empty-target gate includes the new table. The portable runtime uses the same API implementation and migrations.

## Executed checks

| Check | Passed |
| --- | ---: |
| Manual WhatsApp API scenarios | 14 |
| Independent adversarial API scenarios | 16 |
| Portable WhatsApp HTTP, restart and recovery | 7 |
| Frontend logic and actual-handler simulation | 125 |
| Core records API | 25 |
| Existing security and transaction boundaries | 32 |
| Progress, email, backup and dashboard regression | 13 |
| Independent image/restore regression | 15 |
| Portable image HTTP and exact-byte recovery | 10 |
| Copied portable server HTTP compatibility | 17 |
| Hosted migration formatting/retry checks | 4 |

All 278 executed checks passed. TypeScript passed. The frontend checks include 19 WhatsApp handler scenarios using simulated DOM, API, clipboard and popup objects. Adversarial cases inject changes to raw employee phone/country, request association/status/archive and progress text/revision before confirmation insertion. Stale changes are rejected without adding a confirmation. The standalone HTTP tests run copied portable servers with fictional records and independent restore targets.

The 17-page manual was rebuilt, rendered and visually checked. Hosted and portable builds are completed during final packaging. Release code and downloads exclude production credentials, private records and backups.

## Verification limits

No real employee was contacted and no WhatsApp URL was opened during tests. Interactive browser behavior, actual WhatsApp app/Web handoff and physical phones remain unverified. Automated tests do not establish delivery or real-world phone ownership. Production schema can be inspected without reading private records; no full private production before/after export comparison was performed for this change.

This source report records implementation and local checks. The final release handoff records the actual publication result.

## Manufacturer guidance

WhatsApp Help Center: [How to use click to chat](https://faq.whatsapp.com/5913398998672934). The implementation uses a full international phone number without the plus sign in the path and URL-encoded prefilled message text.
