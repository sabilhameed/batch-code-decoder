# Security

This is a single-page tool that runs entirely in the browser. It has no server, no
backend, no authentication and no network calls. It stores nothing — no cookies, no
`localStorage`, no telemetry. Batch codes typed or pasted into the page never leave
the device.

It handles no product, customer or commercial data. The only input is a 4-digit
numeric batch code; the only output is date arithmetic derived from it.

## Review history

**August 2026 — Shiseido EMEA IT / cybersecurity review (v3).**
Two findings, both fixed in v3 before release:

| # | Finding | Fix |
|---|---------|-----|
| 1 | CSV/TSV formula injection. Pasted input is echoed into the export, so a value opening with `=`, `+`, `-`, `@`, TAB or CR could execute as a formula — or render as a clickable link — when the export was opened in Excel. | `sanitizeForSpreadsheet()` prefixes such values with an apostrophe. Applied on **both** export paths: the CSV download and the copy-for-Excel TSV. |
| 2 | Incomplete character escaping in `esc()`, the function that renders unreadable input back into the results table. | `"` and `'` added to the escaped set, alongside the existing `&`, `<`, `>`. |

### Note on the formula guard

Values the tool computes itself — day counts, year, day-of-year — bypass the guard
and stay numeric:

```js
if (typeof v === "number" && isFinite(v)) return String(v);
```

Without this, the `-` in the guard's character class prefixes every negative
"days left" value on expired stock, which imports into Excel as text and breaks
sorting and filtering on that column. Everything arriving from the paste box is a
string and still passes through the guard, so the injection path stays closed.

## Reporting

Open an issue, or contact the maintainer directly for anything that shouldn't be
public.
