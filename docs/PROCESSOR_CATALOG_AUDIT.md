# Processor catalog audit - Team Tracker v1.3

Reviewed: 17 October 2026.

Expanded 2,079 distinct named processor labels to **4,454**, across **16 named manufacturer choices** and one manual Other / custom choice. There are **173 manufacturer/family paths** including manual paths, and **4,454 named processor mappings**.

Corrected or retired **661 original CPU tuples**. Every exact historical tuple remains accepted by shared validation for existing records and backups. The edit form displays an old choice only when it is the original unchanged saved CPU selection. New dropdown choices use the corrected hierarchy. Raw API clients may still submit an exact known historical tuple.

Intel -> Core i5 13th Gen -> Core i5-13420H is now a named selectable path. Manufacturer spelling is AMD.

## Per-manufacturer named mappings

| Manufacturer | Before | After | Manufacturer source reviewed | Earlier references retained |
| --- | ---: | ---: | ---: | ---: |
| AMD | 723 | 1,183 | 1,163 | 20 |
| Ampere | 0 | 25 | 25 | 0 |
| Apple | 18 | 46 | 46 | 0 |
| Broadcom | 0 | 7 | 7 | 0 |
| IBM | 0 | 12 | 12 | 0 |
| IBM / Motorola | 7 | 21 | 21 | 0 |
| Intel | 1,310 | 2,961 | 2,909 | 52 |
| MediaTek | 0 | 38 | 38 | 0 |
| Microsoft | 0 | 3 | 3 | 0 |
| Motorola | 5 | 18 | 18 | 0 |
| NVIDIA | 0 | 7 | 7 | 0 |
| Other / custom | 0 | 0 | 0 | 0 |
| Qualcomm | 16 | 67 | 67 | 0 |
| Raspberry Pi | 0 | 1 | 1 | 0 |
| Rockchip | 0 | 6 | 6 | 0 |
| Samsung | 0 | 39 | 39 | 0 |
| VIA | 0 | 20 | 20 | 0 |

Counts are inventory labels and parent mappings, not physical stock, distinct silicon dies, ordering codes or guaranteed current availability. Historical and discontinued chips are useful for existing equipment.

## Scope and evidence

- Reviewed inputs retain a primary source URL and note for each processor. Exact suffixes and explicit variants come from retrieved manufacturer tables, comparison sheets, product/support lists and named archived datasheet entries; no numeric SKU combinations are generated.
- Intel: Core laptop/desktop comparison sheets and ARK family tables; server, embedded and discontinued Xeon/Atom/Pentium/Celeron/Intel Processor lists. Corrects generation parsing, malformed aliases and family headings that were incorrectly treated as processor SKUs.
- AMD: complete retrieved client, server and embedded specification tables plus explicit archived model tables. Current Ryzen AI/Max/PRO/Z, Threadripper and embedded lines have their own families. Preliminary EPYC 9006 specifications are excluded from additions.
- Other manufacturers: sourced computer, tablet, server and single-board-computer chips. Apple M/A series, Qualcomm X/X2 and earlier computing chips, and historical PowerPC/68000 references are included. Companion/security and smartwatch chips are outside this inventory scope.
- Earlier reference labels without a new exact manufacturer match retain their original source and review date. The CSV explicitly distinguishes these from this audit's manufacturer-reviewed mappings. Absence from a current public table is not proof that an OEM or regional part does not exist.
- This is not every CPU ever sold, every OEM/custom ordering code, a live retail availability feed, or a hardware/OS compatibility certification. Unlisted products use Other / custom plus Exact processor.

## Reproduction and checks

`python3 scripts/refresh-processor-catalog.py` first reconstructs the prior machine-model expansion, then applies `shared/processor-catalog-reviewed.json` and explicit `shared/processor-catalog-corrections.json`. The model refresh command also reapplies the CPU stage by default. Both produce the same final catalog.

`shared/processor-catalog-audit.json` records before/after counts, additions, refreshed provenance, retired families and every correction. `shared/processor-catalog-legacy.json` stores the exact historical tuples. `shared/processor-source-audit.json` records retrieved-source evidence. `public/processor-catalog.csv` is the complete active named hierarchy with per-row provenance and review status.

Run `node scripts/test-processor-catalog.mjs`, `node scripts/test-catalog.mjs`, `node scripts/test-model-catalog.mjs`, application API/frontend checks and typechecking. Test results are recorded separately in `qa/processor-catalog.json`. The independent CPU suite exercises all canonical hierarchy paths, wrong parents, legacy validation, full-list search, synthetic API saves and complete backup/restore.

All 25,751 non-CPU catalog rows are identical to the model stage used for this rebuild, including all 23,280 machine-model labels. The model catalog has its own review date and audit. This source update changes no operational database records, schema, shared access code or secret settings.

Rendered-browser and physical-phone testing remain unverified; component and HTTP checks do not establish physical hardware compatibility.
