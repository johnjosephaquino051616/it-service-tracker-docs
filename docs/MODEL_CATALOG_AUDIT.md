# SinCityNotary Team Tracker - Make/brand and model audit

Reviewed: 18 October 2026. Application edition: v1.3.

## Result

Added **18,759** source-verified brand/device/model mappings in this review, including **3,078 Acer mappings**. The catalog now contains **23,855 named mappings**, **23,280 distinct model/family labels**, and **30,568 total reference rows after the retained processor stage.

All 13 named manufacturer choices were reviewed using official Philippine and worldwide product, support and historical inventories. Existing choices and their provenance remain available. Employee records, Asset IDs, report/request links, uploaded files, shared access and schema are unchanged.

## This review by brand

| Make / brand | Before this review | Added | Current named mappings |
| --- | ---: | ---: | ---: |
| ASUS | 223 | 2,014 | 2,237 |
| Acer | 221 | 3,078 | 3,299 |
| Apple | 207 | 127 | 334 |
| Dell | 821 | 1,100 | 1,921 |
| Dynabook | 125 | 1,961 | 2,086 |
| Framework | 25 | 1 | 26 |
| Fujitsu | 526 | 4,353 | 4,879 |
| HP | 801 | 1,818 | 2,619 |
| Intel / ASUS NUC | 133 | 133 | 266 |
| Lenovo | 1,615 | 2,275 | 3,890 |
| MSI | 177 | 1,506 | 1,683 |
| Microsoft | 63 | 56 | 119 |
| Samsung | 157 | 337 | 494 |

Counts are distinct brand/device/model selections, not physical stock or unique configurations. Existing family aliases remain; an exact platform and a regional order code can be separate legitimate choices.

## Sources and category checks

- Acer: complete retrieved PH/US support sitemaps, the Philippine Windows compatibility inventory, Philippine store and official international specifications. **Aspire A715-59G** is verified in Acer Philippines support and selectable under **Acer / Laptop**. Selected ambiguous Veriton chassis were resolved with specific official product text or labeled product images.
- ASUS and Lenovo: 223 Philippine/global ASUS support sitemaps, 153 ROG specification pages, Lenovo public PH support taxonomy, current/withdrawn PSREF and historical support lists. Components, racks, switches, storage infrastructure and announced-only candidates were excluded. Confirmed convertibles and Tiny/q/Nano machines use their correct types; Yoga Slim/S clamshells remain laptops.
- Dell and HP: official archived Dell model datasheets and DriverPackCatalog, HP Philippines paginated product listings and worldwide Image Assistant platform inventory. Failed HP product ranges were recovered using smaller requests. Aggregate applicability lists were excluded; all-in-one, mini PC, detachable and workstation categories were checked.
- Dynabook and Fujitsu: manufacturer annual/manual/support archives from 1996 onward, Japanese regional indexes and worldwide server inventories. Named tablets and convertibles, all-in-ones and FUTRO thin clients were reviewed separately from laptops and desktops.
- MSI and Samsung: Philippine/global inventories, historical support applicability lists, exact US retail variants and Samsung regional manufacturer price/support records. Accessories and service codes were excluded.
- Apple, Microsoft, Framework and Intel / ASUS NUC: official current/historical model identification and documentation, Surface System SKU tables, Framework Chromebook Edition and Intel/ASUS NUC support documents. Wildcard patterns, board-only patterns and explicitly unreleased products were excluded.

Independent review caught and removed Acer headset/monitor collisions (PHR200, PHR550, PHW350, PHW910 and PT167Q), duplicate bare chassis aliases and misclassified convertible, tiny desktop, thin-client and all-in-one choices. Four unresolved Veriton chassis remain excluded rather than assigned a guessed type.

## Use

Choose **Laptop → Acer → Model**, search **A715-59G**, then select **Aspire A715-59G**. Search operates over the full selected brand/type list before its 100-result display limit. Each new choice is validated only for its documented manufacturer and device type.

Exact model appears only for Model → Other / custom. Installed build appears only if Operating system, Edition or OS version is Other / custom. Hidden saved text is retained; this catalog update does not alter those visibility rules.

Download `model-catalog.csv` under Help & guide to inspect all named mappings, sources, review dates and notes. `shared/model-catalog-reviewed.json` retains the source-backed inputs; `shared/model-catalog-audit.json` keeps the original 389-row model baseline and cumulative counts. `shared/model-source-audit-2026-10-18.json` records this review’s input hashes, source manifests, category checks and exclusions.

## Rebuild and validation

Rebuild with `python3 scripts/refresh-model-catalog.py`. The processor stage is reapplied automatically, so a model refresh preserves the corrected CPU hierarchy. Per-entry review dates are retained, and a newer baseline date cannot be overwritten by an older input. An isolated replay verifies byte-identical data and CSV outputs.

Run the model, processor, catalog, frontend and API suites, type checking and the production build. Tests exercise every new model mapping, all manufacturer/type boundaries, full-list search and exact A715-59G selection, prior-model compatibility, synthetic create/edit/export/restore and peripheral exclusion. Results are recorded in `qa/`. No production data is used by these checks.

## Coverage limits

This is an extensive manufacturer-sourced reference, not a certification that every regional or historical orderable SKU ever released is present. Public inventories differ by region and may omit archived products. Product availability and compatibility are not established by catalog membership. Use Other / custom and the exact device label for a remaining unlisted device. Source-specific limitations and unresolved chassis are recorded in the source audit.

Interactive browser and physical-device testing remain unverified.
