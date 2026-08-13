# Batch Code Decoder

Turns the 4-digit batch code printed on a product into its production date and expiry dates. Built for market sales teams and warehouse staff who need the answer with a box in one hand.

No install, no login, no server. It's a single HTML file that runs entirely in the browser, so it keeps working in a dead spot in the warehouse once the page has loaded.

---

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

## Two builds

| File | Style | Use it for |
|------|-------|-----------|
| `index.html` | GINZA — soft neomorphic surfaces, Shiseido red accent, Cormorant Garamond and Sora | Store counters, offices, anything customer-facing |
| `decoder-flat.html` | High-contrast flat, system fonts | Goods receipt, pick lines, harsh lighting, cheap screens |

Neomorphism trades contrast for elegance. Those soft shadows depend on subtle luminance differences and they flatten under fluorescent strips or at low screen brightness. Text stays at full contrast in both builds, but the sense of depth doesn't survive a bright warehouse. Pick per audience — there's no reason sales and logistics need the same link.

Both builds share identical decoding logic.

---

## Deploying it

GitHub Pages is the shortest path to a URL you can share.

1. **Settings → Pages**
2. Source: **Deploy from a branch**, branch `main`, folder `/ (root)`
3. Save, wait about a minute

Your URL will be `https://<org-or-user>.github.io/<repo>/`. Paste that into email or WhatsApp — share the link, never the `.html` file. Sent as an attachment, iOS opens it in a preview that blocks JavaScript, and staff get a page where typing does nothing.

Private repos need a paid GitHub plan for Pages. If that's a blocker, any static host or internal web server works the same way: it's one file with no backend.

### Getting it onto phones

Adding it to the home screen makes it behave like an installed app.

- **iPhone / Safari** — Share → Add to Home Screen
- **Android / Chrome** — three-dot menu → Add to Home screen

Worth putting those two lines in your rollout message. Most people won't think to do it, and without it they'll be hunting for the link every time.

### For the warehouse

Print a QR code pointing at the URL and stick it at goods receipt and on the pick-line pillars. Nobody has to find a message from six weeks ago.

---

## Verification

The decoding logic was checked against all 4,551 rows of the source spreadsheet (`Production_date_calculator.xlsx`, covering 2015–2026). Every row resolves to the correct production date, either as the primary reading or as the offered alternate.

Three defects in that spreadsheet are worth recording, since they're the reason this tool exists rather than a lookup table:

- **31 December 2020 is missing.** Batch `0366` returns nothing. 2020 was a leap year and the row was never added.
- **169 duplicated rows in 2024**, roughly mid-July onward.
- **899 duplicated batch codes** across the two tabs, from the ten-year repeat described above. A `VLOOKUP` against that file silently returns whichever it hits first.

The spreadsheet also stops at 2026. This tool is pure date arithmetic, so it doesn't expire.

---

## Repo contents

```
├── index.html            # GINZA build (default, served by Pages)
├── decoder-flat.html     # high-contrast build
└── README.md
```

No build step and no dependencies. The GINZA build embeds subsetted WOFF2 fonts as base64 so it renders identically offline; that's why it's 76KB against 15KB for the flat build. Edit either file directly and commit.

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
