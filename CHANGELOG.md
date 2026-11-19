# Changelog

All notable changes to this project are recorded here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the project uses
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

The running version is shown in the page footer, and is set by `VERSION` at the top of the script
in `index.html` — bump it in the same commit as the entry below.

## [Unreleased]

## [1.0.0] - 2026-11-19

First public release. One HTML file, no build step, nothing leaves the browser.

### Added

- **Campaign Price mapping** from a brand's commercial price file into Lazada, Shopee and TikTok
  Treasure Bowl submission files, many files per run.
- **Platform detection** from the TB file's own columns, with Lazada / Shopee / TikTok override
  chips. The platform also decides which tab of the commercial file is read, and a key column
  naming another platform is never used.
  - Lazada matches on Seller SKU, then Shop SKU.
  - Shopee matches on Variation ID only.
  - TikTok matches on SKU ID, then Product ID.
- **Two commercial layouts**: wide (one column per Treasure Bowl, codes on a header row) and long
  (one row per SKU × Treasure Bowl).
- **TB code row found by label** — `Treasure Bowl ID`, `TBS ID`, `TB code`, `TB No.`, `link code`,
  `Seller SKU / TBS ID` — or, unlabelled, by codes sitting over price columns.
- **Combined code cells** such as `383455/383582`: both codes register and share one price column.
- **Discount-column handling**: a code sitting over a `%Discount` column resolves to the paired
  price column beside it.
- **TB code from the filename** — after `#`, else whichever code matches the price file, else a
  6-digit code — with a per-file dropdown to override, and no pre-selection when nothing matches.
- **In-place output**: the submitted `.xlsx` is unzipped and only its worksheet XML is edited, so
  styles, fills, fonts, number formats, column widths, data validation, other sheets and the
  filename all survive untouched. Falls back to rebuilding the sheet if the zip library is missing.
- **Per-file summary** before download: SKUs filled, removed, found without a price, the column
  matched on, the campaign and price tier, and the first 300 mapped rows.
- **Checks on the price file**: codes shorter than six digits are flagged as likely typos, dates are
  never read as codes, and the page states which tab, row and key columns it read.

[Unreleased]: https://github.com/watthana-d/treasure-bowl-price-filler/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/watthana-d/treasure-bowl-price-filler/releases/tag/v1.0.0
