# IT Service Tracker - public documentation

SinCityNotary Team Tracker v1.3 manages employees, equipment, accessories, access checklists, Report/Request tickets and activity history.

This repository is a documentation showcase. Application source, a runnable website, production credentials, employee records, images and backups are not included. Employees can open the live site without a ChatGPT account and sign in using the existing shared team code. Records, images and backups require a valid tracker session.

## Read the documentation

| Document | Contents |
| --- | --- |
| [Project overview](PROJECT_OVERVIEW.md) | Features, workflow and release scope |
| [User guide](docs/Team_Tracker_Guide.md) | Complete readable user manual |
| [PDF manual](docs/USER_MANUAL.pdf) | Downloadable 18-page manual |
| [Requirements](docs/REQUIREMENTS.md) | Record fields and business rules |
| [Developer handoff](docs/DEVELOPER.md) | Architecture, data contracts and maintenance |
| [Security and recovery](docs/SECURITY.md) | Access controls, backups and limitations |
| [Security audit](docs/SECURITY_AUDIT_2026-10-18.md) | Historical findings, fixes and verification limits |
| [Security audit PDF](docs/SECURITY_AUDIT.pdf) | Downloadable audit report |
| [QA summary](docs/QA_SUMMARY.md) | Dated checks and limits |
| [Upgrade runbook](docs/PRODUCTION_UPGRADE_V1.3.md) | Historical upgrade procedure |
| [Maintenance](docs/MAINTENANCE_2026-10-18.md) | Dated maintenance record |
| [Device images](docs/IMAGE_UPLOADS_2026-10-18.md) | Private uploads, gallery and media recovery |
| [Manual WhatsApp](docs/WHATSAPP_RELEASE_2026-10-18.md) | Drafting and self-reported confirmations |
| [Report/Request labels](docs/REPORT_REQUEST_LABEL_2026-10-18.md) | Current workflow labels |
| [Machine model catalog](docs/MODEL_CATALOG_AUDIT.md) | Coverage, sources and limits |
| [Processor catalog](docs/PROCESSOR_CATALOG_AUDIT.md) | Processor mappings and source review |
| [Backup validation](docs/BACKUP_VALIDATION_2026-10-16.md) | Historical recovery rehearsal |
| [Release report](docs/SinCityNotary_Tracker_v1.3_Release_Report.md) | Historical deployment report |
| [Backup acceptance report](docs/SinCityNotary_Tracker_v1.3_Backup_Validation_Report.md) | Earlier backup acceptance record |
| [Release notes](RELEASE_NOTES.txt) | Feature and maintenance notes |

## Access and publication notes

The live site uses the application's existing shared-code authentication. The ChatGPT sign-in gate was removed on 18 October 2026 so employees can use the tracker without the owner's ChatGPT credentials. Anyone with the shared code can access the workspace; keep the code confidential and share it only with authorized employees. Earlier test counts and security observations are dated evidence, not a fresh security certification.

Private production URLs, hosting project IDs, snapshot details and backup fingerprints have been removed from this public edition. References to implementation files, scripts, data catalogs and QA artifacts identify files in the private application package. Those files are intentionally absent here. Safe documentation is readable; application access requires the private shared team code supplied by the owner.

## Rights

Copyright 2026. All rights reserved. Public availability of this documentation does not grant a license to copy, redistribute, deploy or use the private application. See [NOTICE.md](NOTICE.md).
