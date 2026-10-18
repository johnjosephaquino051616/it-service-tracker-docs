# SinCityNotary Team Tracker maintenance - 18 October 2026

Application edition remains v1.3. Starting authoritative source: `5fed5919b5457a1bf0348ebc20855c5f9dad02e6`. Same original live Site, database binding and audience.

## Findings and changes

- Updated Next.js 16.3.4 to 16.4.2 and matching ESLint configuration. Updated React, React DOM and React server DOM together from 19.2.6 to 19.2.9. Applied reviewed compatible transitive security pins and regenerated the frozen lockfile.
- The official package advisory service reported 62 findings before the update: 8 low, 23 moderate, 30 high, 1 critical. After the update it reports one high finding and zero low, moderate or critical findings. Counts describe dependency advisories, not demonstrated exploitable endpoints. Exact evidence and remaining paths: `qa/dependency-security-2026-10-18.json`.
- Added nosniff, same-origin referrer and camera/microphone/geolocation-denying response headers to the hosted HTML/API adapter. Existing platform embedding and access settings remain in effect.
- Measured bootstrap worker wall time of 632-754 ms with only 4-5 ms CPU; its record/history reads were separate database requests. Combined those reads into one transactional batch, removing one database round trip and giving them a consistent snapshot. Added an authenticated bootstrap response/history regression. Live after-publication timing is reported separately; no load test or synthetic writes were run against production.
- At this maintenance release, the reported Needs attention issue did not reproduce under the then-current rule. The regression counted four flagged active devices and two unresolved priority requests as six, excluding resolved/closed/archived records. The observed live dataset at that time yielded zero: both devices were Good and its urgent request was Resolved. This historical rule was later superseded on 18 October 2026: the current counter excludes Equipment entirely and counts only qualifying Report/Request records. The same regression inputs now contribute two, not six; the earlier live observation is not a new production check.
- Both catalog audits passed: 4,588 distinct model labels, all 135 brand/device paths and 4,454 canonical named processor mappings across 173 manufacturer/family paths. All 661 historical CPU tuples remain supported. No catalog labels or operational records were altered. Regeneration reproduced catalog, audit, legacy and CSV artifacts byte-for-byte.
- Fresh official Lenovo PSREF and Intel specifications confirm IdeaPad Slim 3 15IRH10 and Intel Core i5-13420H / Core i5 13th Gen. These entries are in the live downloads and saved equipment. This is a targeted fresh upstream check plus complete structural/provenance regression, not a re-retrieval of every regional SKU ever manufactured.

## Backup and integrity

A fresh authenticated consistent full JSON backup was verified and saved privately before publication. It contains 9 records, 131 history events, 11 issued IDs, one counter, one request progress update, one progress revision and zero email marks. Size: 113,009 bytes. SHA-256: `[private verification fingerprint omitted]`.

Read-only checks verified unique permanent IDs, ledger membership, linked records, revision/history counts and matching latest history. Backups, session tokens, access values and employee exports are excluded from source/public downloads. No migration or production-data repair is needed. Publication preservation and final live checks are recorded in the accompanying maintenance report after deployment.

## Checks on this source

| Check | Passed |
| --- | ---: |
| Core API, including bootstrap snapshot response | 25 |
| Shared catalog | 14 |
| Additional security | 32 |
| Frontend state, inactivity, theme and journal guards | 76 |
| Search and additional search | 32 |
| Progress, backup and dashboard | 13 |
| Machine models | 11 (741,211 assertions) |
| Processor hierarchy and historical compatibility | 14 |
| Migration validation | 4 |
| Copied portable HTTP integration | 17 |

Total: 238 cases/checks. TypeScript, hosted and portable production builds also pass. API/restore/concurrency tests use isolated synthetic data and throwaway databases. No real email is sent. Model/processor regeneration is reproducible.

The portable source/runtime downloads are rebuilt with this maintenance source. README, developer/security notes and release QA are updated. User workflows and the 14-page guide are unchanged; live `guide.pdf` matched the current authoritative PDF byte-for-byte, so no PDF rewrite is needed.

## Remaining issues and verification limits

- `braces` 3.0.3 has the high-severity upstream advisory GHSA-vfj7-8cjw-p6xm with no published patched version. It is pulled by trusted build/lint glob tooling through Vinext and ESLint; tracker form/search inputs do not reach those glob calls. Keep development servers private and do not give build tooling untrusted glob patterns. This remains unresolved pending an upstream patch; the audit is not clean.
- Browser-control support is unavailable. Interactive rendered desktop/mobile behavior, focus/keyboard interactions and physical phones remain unverified. Component/static markup and HTTP checks do not establish those behaviors.
- The existing documented 10,000-record interactive cap, shared-code access model and manually composed email workflow are unchanged. There is no SMTP/delivery verification or native mobile package.
- The connected tools have no native D1 snapshot interface. The verified private consistent JSON export is the available backup safeguard.

## Official references reviewed

- [Next.js 14 October 2026 security release](https://nextjs.org/blog/nextjs-security-update-october-14-2026)
- [Next.js ImageResponse advisory](https://github.com/vercel/next.js/security/advisories/GHSA-vcvr-r3jv-pc5j)
- [React server DOM advisory](https://github.com/advisories/GHSA-wx67-qw84-cm4g)
- [Remaining braces advisory](https://github.com/advisories/GHSA-vfj7-8cjw-p6xm)
- [Official npm package metadata and audit service](https://registry.npmjs.org/)
- [Lenovo IdeaPad Slim 3 15IRH10 PSREF](https://psref.lenovo.com/syspool/Sys/PDF/IdeaPad/IdeaPad_Slim_3_15IRH10/IdeaPad_Slim_3_15IRH10_Spec.pdf)
- [Intel Core i5-13420H specifications](https://www.intel.com/content/www/us/en/products/sku/232173/intel-core-i513420h-processor-12m-cache-up-to-4-60-ghz/specifications.html)
