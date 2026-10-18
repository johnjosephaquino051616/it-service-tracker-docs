# Report/Request terminology - 18 October 2026

The user-facing Request module is now named **Report/Request**. Navigation, page headings, add/edit forms, dashboard labels, search categories, identity labels, help, validation and newly prepared email/WhatsApp messages use the new wording. The corresponding form labels are **Add Report/Request**, **Edit Report/Request**, **Report/Request details** and **Report/Request ID**.

The messaging panel uses the editor's stable `data-record-kind` hook and supports the previous editor title as a compatibility fallback. Renaming the displayed form does not disconnect its progress, email or WhatsApp tools.

This changes presentation only. Internal `requests` keys, API routes, database tables, IDs, relationships and stored employee text/history remain unchanged. It requires no SQL migration. Generic **Notes / requests** fields and historical release evidence retain their existing wording.

The user manual remains 17 pages; changed pages were rendered and visually checked. Source and portable packages include the revised manual and app copy.

Executed checks: core API 25, WhatsApp API 14, independent WhatsApp/API 16, frontend logic and simulated handlers 125, search 16, progress/email/backup/dashboard 13, portable WhatsApp HTTP/recovery 7. All 216 passed. TypeScript passed. The existing dashboard assertion was updated to the renamed visible wording; its count expectations remain the same.

Checks use fictional records and disposable databases. Actual interactive browser/phone handoff remains unverified. No production data comparison or external employee messaging was performed. Publication status is reported in the final handoff.
