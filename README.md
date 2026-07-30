# csv-to-anything

Convert CSV or TSV input to JSON, SQL inserts, Markdown table, or TypeScript types. Single HTML file. Browser-only.

**Live demo:** https://0xelitesystem.github.io/csv-to-anything/

## Why

You hit this loop constantly: get a CSV from someone, need it as JSON for an API, or as SQL inserts for a database, or as a Markdown table for a doc, or as TypeScript types for a typed codebase. Most tools do exactly one of these. This does all four, instantly, in one file.

## Use it

Open `index.html` in any browser, or visit the hosted demo at `https://0xelitesystem.github.io/csv-to-anything/` once Pages is enabled.

1. Paste CSV or TSV into the left panel.
2. Pick a delimiter (auto-detect works for most cases) and whether the first row is a header.
3. Switch tabs in the right panel: JSON, SQL, Markdown, TypeScript.
4. Copy or download.

## What gets converted

Type inference runs per column based on the data. Recognized types:

- `integer` (matches `/^-?\d+$/`)
- `number` (matches `/^-?(?:\d+(?:\.\d+)?|\.\d+)$/`)
- `boolean` (exactly `true`, `false`, `TRUE`, `FALSE`, `yes`, `no`, `Y`, `N`, `0`, `1`; the match is case sensitive, so `True` and `YES` stay strings)
- `date` (anything starting with `YYYY-MM-DD`)
- `string` (everything else)

A digit string only counts as `integer` or `number` if the JS number renders back to the same text. That keeps `01234` and `12345678901234567890` as strings instead of turning them into `1234` and a rounded id.

Empty cells become `null` in JSON, `NULL` in SQL, blank in Markdown, and `null` is included as a permitted value in TypeScript types.

## Output formats

**JSON**: pretty-printed array of objects, type-coerced.

**SQL**: `INSERT INTO "<table>" (...) VALUES (...);` per row. Identifiers double-quoted, strings single-quoted with `''` escape. Booleans become `TRUE`/`FALSE`. The output includes a header comment reminding you to review before running on a live database.

**Markdown**: padded table with `|` separators and a `-` rule row. Column widths auto-compute from header and data. A `|` inside a cell is backslash-escaped and a newline inside a cell becomes `<br>`, so one CSV row always stays one table row.

**TypeScript**: `export interface ${name} { col: type | null; }`. Property names are quoted only if they're not valid identifiers, and quoted names are escaped as JS string literals.

## CSV parsing

The parser handles:

- Quoted fields with `"`
- Escaped quotes inside fields (`""` becomes `"`)
- Embedded newlines inside quoted fields
- `\r\n` and `\n` line endings
- Trailing empty rows

It does NOT handle:

- BOM (byte order mark), strip before pasting if your CSV has one
- Excel-specific quirks like leading `=` formulas

## Tech

- Single HTML file, ~390 lines
- Vanilla JS, no frameworks, no dependencies, no build
- Light and dark themes with OS preference detection
- WCAG AA contrast on both themes
- Tested in current Chrome, Firefox, Safari

## What it doesn't do

- Doesn't upload your data anywhere. Everything runs locally.
- Doesn't validate SQL against your actual schema. The output is a starting point.
- Doesn't infer JSON Schema. Use `json-to-typescript` or a dedicated tool for that.
- Doesn't streaming-process huge files. Targeted at human-scale CSVs (< 100k rows).

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT. See [LICENSE](LICENSE).

## Related

- [json-to-typescript](https://github.com/0xelitesystem/json-to-typescript), the inverse direction, JSON to TS
- [regex-tester-with-explainer](https://github.com/0xelitesystem/regex-tester-with-explainer), test regex patterns
- [og-image-generator](https://github.com/0xelitesystem/og-image-generator), generate Open Graph cards
- [single-file-saas-template](https://github.com/0xelitesystem/single-file-saas-template), ship a SaaS in one HTML file
