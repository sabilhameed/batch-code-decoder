# Batch Code Decoder

Turns the 4-digit batch code printed on a product into its production date and expiry dates. Built for market sales teams and warehouse staff who need the answer with a box in one hand.

No install, no login, no server. It's a single HTML file that runs entirely in the browser, so it keeps working in a dead spot in the warehouse once the page has loaded.

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

## The ambiguity you need to know about

One digit records the year, so **every code repeats every ten years**. `5001` is both 1 January 2015 and 1 January 2025.

The tool resolves this by taking the most recent year that matches the digit and does not fall in the future, then telling you when an older reading is also possible and offering a one-tap switch. If the stock in front of you looks far older than the date shown, use that switch.

That last condition matters. A code decoding to a date that hasn't happened yet means the code was misread or the pack is a decade old, so the tool steps back rather than reporting a future production date. Enter `6365` today and you get 31 December 2016, not 2026.

---


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

---

## Contributing

Hit a code the tool won't read? Open an issue with a photo of the pack. Finding format variants early is cheaper than someone confidently misreading a code and writing off good stock.

---

Internal reference tool. Contains no product, customer or commercial data — only date arithmetic.
