# Batch Code Decoder

Turns the 4-digit batch code printed on a product into its production date and expiry dates. Built for market sales teams and warehouse staff who need the answer with a box in one hand.

No install, no login, no server. It's a single HTML file that runs entirely in the browser, so it keeps working in a dead spot in the warehouse once the page has loaded.

<img width="708" height="1080" alt="image" src="https://github.com/user-attachments/assets/8e1722fd-39b3-4d0e-8166-61ea148d1594" />

<img width="749" height="1388" alt="image" src="https://github.com/user-attachments/assets/9fcfdfef-4001-4e90-a7a6-23d0aa4e378d" />

---
## Demo site 

https://sabilhameed.github.io/batch-code-decoder/index.html

Access the tool using the above link to preview and test the functionality.

## The batch code

The code is the last digit of the production year followed by the day of the year, padded to three digits.

```
4 1 2 3
│ └─┴─┴── day of year (001–366)
└──────── last digit of the production year
```

| Code | Reads as | Production date |
|------|----------|-----------------|
| `4001` | Year …4, day 1 | 1 January 2024 |
| `5182` | Year …5, day 182 | 1 July 2025 |
| `0366` | Year …0, day 366 | 31 December 2020 |

Expiry is calculated from the production date, keeping the same day and month:

| Category | Shelf life |
|----------|-----------|
| Skincare and makeup | 3 years |
| Fragrance | 4 years |

Where a pack prints an expiry date directly, that printed date wins. This tool derives the date from the batch code, so it's the answer when nothing is printed, not an override.

---

## Two ways to use it

**One code.** Type the four digits. Production date, both expiry dates, days remaining and a shelf-life meter appear as you type.

**Many codes.** Switch to the *Many codes* tab and paste a list — one per line, or separated by commas, semicolons, tabs or spaces. Every code is decoded into a sortable table with a count of what's in date, expiring soon, expired and unreadable.

Batch mode is for the jobs that used to mean opening the spreadsheet: checking a delivery against a packing list, sweeping a stock count for near-expiry units, or auditing a returns pallet.

| | |
|---|---|
| **Sorting** | Click any column header. Codes that can't be read always sink to the bottom. |
| **Export CSV** | Downloads a `.csv` with both expiry dates, statuses, days remaining and a notes column. UTF-8 BOM included so Excel opens it cleanly. |
| **Copy for Excel** | Puts tab-separated rows on the clipboard — paste straight into a sheet, no import step. |
| **Limit** | 5,000 codes per run. Beyond that the first 5,000 are shown and the page says so. |

Unreadable entries aren't silently dropped. Each one keeps its row and states why it failed — wrong digit count, day `000`, day above `366` — so a mistyped line in a list of 300 doesn't quietly vanish from the count.

Both tabs share one decoding engine, so a code can never resolve differently depending on which tab you used.

---

## The ambiguity you need to know about

One digit records the year, so **every code repeats every ten years**. `5001` is both 1 January 2015 and 1 January 2025.

The tool resolves this by taking the most recent year that matches the digit and does not fall in the future, then telling you when an older reading is also possible and offering a one-tap switch. If the stock in front of you looks far older than the date shown, use that switch.

That last condition matters. A code decoding to a date that hasn't happened yet means the code was misread or the pack is a decade old, so the tool steps back rather than reporting a future production date. Enter `6365` today and you get 30 December 2016, not 2026.

In batch mode the alternate reading is recorded in the notes column rather than offered as a button, since there's nothing sensible to click per-row across hundreds of lines. The primary reading is the one used for the status counts.

---

### Getting it onto phones

Adding it to the home screen makes it behave like an installed app.

- **iPhone / Safari** — Share → Add to Home Screen
- **Android / Chrome** — three-dot menu → Add to Home screen

Worth putting those two lines in your rollout message. Most people won't think to do it, and without it they'll be hunting for the link every time.

### For the warehouse

Print a QR code pointing at the URL and stick it at goods receipt and on the pick-line pillars. Nobody has to find a message from six weeks ago.

---

## Verification

The decoding logic was checked against 4,551 sample batches (covering 2015–2026). Every row resolves to the correct production date, either as the primary reading or as the offered alternate.

This tool is pure date arithmetic.

---

## Known limits

- **Format assumption.** Every code is assumed to be four numeric digits. Alpha prefixes, plant codes and other formats will be rejected. If a line prints something different, that needs handling before rollout.
- **Category, not SKU.** Shelf life is applied by category, not looked up per product. Brand-level exceptions aren't modelled.
- **Ten-year ceiling.** The tool searches four decades back but only ever offers the two most plausible readings. Stock older than about ten years is expired under either reading anyway.
- **Batch summary uses the 3-year rule.** The in-date / expiring-soon / expired counts above the table are based on skincare and makeup shelf life. Fragrance dates are in the table, but a mixed list will read pessimistically in the summary chips.

---

## Versions

| Version | What changed |
|---------|--------------|
| **v2** *(current)* | Batch mode — paste many codes, sortable results table, CSV and clipboard export, per-row failure reasons. |
| v1 | Single-code decoding. Archived at [`archive/v1.html`](archive/v1.html). |

---

## Contributing

Hit a code the tool won't read? Open an issue with a photo of the pack. Finding format variants early is cheaper than someone confidently misreading a code and writing off good stock.

---

Internal reference tool. Contains no product, customer or commercial data — only date arithmetic.
