# Crystal Light Vodka Refresher — Retail Scan Performance Monitor

A single-file, zero-dependency HTML dashboard that tracks Crystal Light Vodka Refresher's
performance in Circana retail scan data, and places it against the full competitive set of
canned RTD / premixed cocktail brand families.

**[▶ View the live dashboard](https://<org>.github.io/<repo>/)**

<!-- ![Dashboard preview](docs/preview.png) -->

---

## What it shows

The dashboard has two views, toggled from the top bar.

### Brand view

| Block | What it answers |
|---|---|
| **Hero** | Headline L52W dollar sales and year-over-year trend. |
| **Brand scorecard** | Five KPIs: L52W dollars, dollars added vs LY, category rank, weighted distribution (CWD), and dollars per point of CWD. |
| **Distribution runway** | A gauge of CWD against the top-10 family average — how much shelf is still unclaimed. |
| **Velocity where distributed** | Dollars per point of CWD vs the brand-family median — does it sell hard where it's actually placed? |
| **Competitive set** | The full ranking of every reporting brand family, scrollable, with Crystal Light highlighted in place. |

### SKU view

Per-SKU cards for the two flavors present in the scan read (Wild Strawberry and Lemonade),
a head-to-head comparison across four metrics, and contribution charts showing each SKU's
share of brand dollars and of the dollars added versus last year.

---

## The competitive ranking

The competitive card lists **all 448 reporting brand families** inside a fixed-height scroll
window, so the card never changes the page layout no matter how long the list is.

- **Crystal Light is auto-centered on load** and highlighted at its current rank.
- **Bars use a log scale** with decade gridlines labeled `$1K → $100M`. The set spans
  $514.16M at the top to $2 at the bottom; on a linear scale every brand below the top ten
  would render as an invisible sliver. An exact dollar column sits beside each bar, because
  a log bar communicates magnitude, not precision.
- **Filter box** narrows the list by brand name, with a live match count.
- **"Jump to Crystal Light"** clears any filter, re-centers the list, and flashes the row.
- Other Barrel One Collective families in the set carry a secondary highlight and a `B1C` tag.

---

## PowerPoint export

The **Export for PowerPoint** button renders both views to 2560 × 1440 PNGs (exact 16:9,
contain-fit, never cropped) and downloads them:

- `Crystal_Light_Brand_16x9.png`
- `Crystal_Light_SKU-Level_16x9.png`

The export is not a screenshot of the page — it composes a separate wide slide layout
off-screen, then rasterizes it with [html2canvas](https://html2canvas.hertzen.com/)
(vendored inline, MIT). For the slide, the competitive card collapses from the scrollable
448-family list to a clean **rank 78–94 neighborhood slice**, with the filter toolbar removed
and bars re-scaled linearly within the slice, since log bars across a $590K–$427K band would
all look identical.

Everything is client-side. Nothing is uploaded anywhere.

---

## Metric definitions

| Metric | Definition |
|---|---|
| **CWD (Max)** | Circana's Category Weighted Distribution — weights each store by its category sales rather than counting doors equally. "(Max)" is the peak weekly value in the period. |
| **Dollar Sales per Point of CWD** | Total dollar sales ÷ CWD %. A velocity measure: how hard the product sells relative to the availability it has. |
| **Dollar Trend vs LY** | % change in L52W dollars versus the prior year-ago 52 weeks. New items ramping off a small base produce very large percentages; the dashboard renders anything above +1000% as `new`. |
| **Rank (Dollar)** | Position when every brand family (or SKU) in the read is ordered by L52W dollars, highest first. Brand and SKU ranks are separate universes. |

---

## Data source & scope

- **Source:** Circana, Total US — Multi Outlet + Convenience
- **Period:** Latest 52 weeks ending **7/26/2026**
- **Package Type:** Cans only, to give a true read on the RTD / canned cocktail segment
- **Competitive set:** Premixed Cocktails + seltzer-centric RTD brand families — **448 families / $2.13B**

Every number in the dashboard comes from the supplied scan extract. No external data,
estimates, or projections have been added.

> **Note on brand naming.** Brands are labeled from the *Brands & SKUs* column of the extract,
> not the *Brand Family Colors* grouping column. Those columns are identical for most brands,
> but the grouping column rolls several Barrel One Collective brands under one label — which
> is why Crystal Light reads as *Crystal Light Vodka Refresher* rather than *Barrel One Collective*.

---

## Repository layout

```
├── index.html                          # GitHub Pages entry point (copy of the dashboard)
├── crystal_light_scan_dashboard.html   # the dashboard — single self-contained file
└── data/
    ├── Dollar Rank with CWD.csv        # brand-family level extract (448 rows)
    └── Dollar Rank with CWD (1).csv    # SKU level extract
```

The CSVs are UTF-16, tab-delimited exports from Circana Liquid Data / CMA.

---

## Technical notes

- **One file, no build step.** All CSS, JavaScript, data, product imagery, and the html2canvas
  library are inlined. Open the `.html` locally or serve it statically — no bundler, package
  manager, or server-side code.
- **No external runtime dependencies.** The only network request is to Google Fonts
  (Bricolage Grotesque, Hanken Grotesk, Fraunces); the dashboard degrades gracefully to system
  fonts offline.
- **No browser storage.** Nothing is written to `localStorage` or cookies.
- **Responsive.** Layout adapts down to phone widths; the competitive list keeps its scroll
  window and drops the dollar column on narrow screens.
- **Accessibility.** Semantic headings, ARIA roles on the view toggle, and a
  `prefers-reduced-motion` guard that disables all animation.
- **Size:** ~547 KB, the bulk of which is the inlined html2canvas library and product imagery.
- **Browsers:** current Chrome, Edge, Safari, and Firefox. The PowerPoint export needs a
  Chromium-based browser or Safari for best font rasterization.

---

## Updating with a new scan period

1. Drop the new Circana extracts into `data/`, keeping the same column layout.
2. Re-run the build script to regenerate the embedded `DATA` object and the competitive ranking.
3. Sanity-check the header stamp (period and week-ending date), the brand-family count in the
   footnote, and that Crystal Light's rank badge matches the extract.
4. Copy the rebuilt dashboard over `index.html` so GitHub Pages serves the current version.

---

## Data confidentiality

This dashboard contains licensed Circana syndicated data covering named third-party brands.
Confirm your Circana license terms before making this repository or its GitHub Pages site
publicly accessible.

---

*Maintained by Barrel One Collective. Logo and product imagery are Crystal Light Vodka
Refresher brand assets.*
