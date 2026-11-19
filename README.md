# Treasure Bowl Price Filler

**v1.0.0** · [Changelog](CHANGELOG.md)

A single-page browser tool for Thailand store operators. It fills the **Campaign Price** column of a
Lazada, Shopee or TikTok Treasure Bowl (TB) submission file from the brand's commercial price file.

Files are read in the browser with [SheetJS](https://sheetjs.com/). Nothing is uploaded — no server,
no storage, no analytics.

## Use it

Open the published page, or clone and open `index.html` in any modern browser.

1. **Pick the platform** — `Auto` reads it from the TB file's own columns; Lazada / Shopee / TikTok
   force it.
2. **Drop the commercial price file.** The tool finds the TB code row on whichever tab suits the
   platform and reports what it read.
3. **Drop one or more TB files.** The TB code is taken from the filename (after `#`, else whichever
   code matches the price file); a dropdown lets you override it per file.
4. **Map prices**, review the per-file summary, then download.

The download is your own file. The `.xlsx` is unzipped, only the worksheet XML is edited — Campaign
Price cells get their value and unmatched rows are removed — and every other part of the archive
(styles, fills, fonts, number formats, data validation, conditional formatting, other sheets) is
written back untouched. Filename, sheet name and layout are unchanged.

## What each platform matches on

| Platform | Key in the TB file | Column written |
| --- | --- | --- |
| Lazada | Seller SKU, then Shop SKU | `Campaign Price（Mandatory）` |
| Shopee | Variation ID only | `Campaign Price` (never *Recommended* Campaign Price) |
| TikTok | SKU ID, then Product ID | `Campaign price` |

The platform also decides which tab of the commercial file is used, and a key column naming another
platform is never used — a TikTok run will not silently match on "Lazada Product ID". If no suitable
key column exists anywhere, the run still happens but the page says which column it fell back to.

## Commercial file formats it understands

Two layouts, found automatically:

- **Wide** — one column per Treasure Bowl, codes on a header row labelled `Treasure Bowl ID`,
  `TBS ID`, `TB code`, `link code`, `Seller SKU / TBS ID`, or unlabelled (then the codes must sit
  over price columns). The price column under each code is used; if that column is a `%Discount`,
  the paired price column beside it is used instead.
- **Long** — one row per SKU × Treasure Bowl, with a TB code column, a SKU column and a price column.

Other details it handles: combined cells such as `383455/383582` (both codes share one price
column), TB codes of 4–20 digits (Lazada runs 6, Shopee 15), typo'd metadata labels
(`camapign name`), and dates that would otherwise look like codes. Codes shorter than six digits are
flagged as likely typos.

## Development

`index.html` is the whole application — no build step, no dependencies to install. SheetJS 0.18.5
(reading) and fflate 0.8.2 (zip in/out for the in-place edit) are loaded from cdnjs; to run fully
offline, download `xlsx.full.min.js` and `fflate.min.js` and point the two `<script src>` tags at
local copies.

## Releasing

The version lives in two places: `VERSION` at the top of the script in `index.html` (shown in the
page footer) and the heading in [CHANGELOG.md](CHANGELOG.md). Bump both in one commit, then tag it:

```sh
git tag -a v1.1.0 -m "v1.1.0"
git push origin main --tags
```

GitHub Pages redeploys from `main` on push, so the tag marks the release and the site follows the
branch.

## Licence

MIT — see [LICENSE](LICENSE).
