# Trail Visitor Spending Ledger

What non-local and local mountain bikers actually spend at trail systems — compiled
from published economic-impact studies, shown exactly as each source reported it.

**On PMR (canonical):** https://www.postmillenniumrenaissance.com/studies/
**Served from:** https://postmillennium-mtb.github.io/studies/

The PMR address is the one to share. github.io is the origin this file is served
from; the PMR page is what `<link rel="canonical">`, `og:url` and the tool's own
Share button all point at.

## What this tool does

Puts 21 site-level spending figures from 17 published studies side by side, without
smoothing over the three things that make them hard to compare:

1. **They measure different units.** Some report per day, some per night, some per
   trip, some per whole visit. Nothing here is converted to a common unit.
2. **They vary enormously in rigor.** Some are direct rider surveys. Some are
   benchmarks borrowed wholesale from unrelated studies. Every row carries a
   confidence tier.
3. **Most of them never measured locals at all.** 11 of the 15 MTB-specific entries
   report no resident spending figure. That gap is the finding, not a data problem.

## Interface

| Control | Effect |
|---|---|
| **Filters** (button) | Folds the controls below away. Shut by default on a phone, open from 640px up. The line under it always states what is filtered: *Showing 14 of 21 · MTB only · tiers V, M* |
| **MTB studies only** | Hides the 6 general-cycling / paved-trail comparison entries |
| **Show ranges** | Draws the high bound where a source gave a range instead of one number |
| **Show study names** (Range view only) | Unticked by default: a Range row shows only its unit until you point at it. Ticked, every study name is printed at once |
| **Confidence** chips | Show or hide each tier. **Low starts unticked**, so the tool opens showing Verified and Medium only; tick it to bring the Low rows back |
| **View: Linear / Log / Range** (By Site) | **The tool opens on Range.**  **Linear** draws bars from $0 to $650. **Log** draws the same rows as dots on a log axis with gridlines (up to $800; it starts at **$20** while every figure shown is above that, and drops to $2 as soon as a lower one is shown, so a $3 dot is never drawn at $20 — the line above the chart says which); it is never drawn as bars, because a log axis has no zero. **Range** is the original Range plot: every figure from every study on one log axis, non-local above local, each reported range drawn as a dot, a line and a dot. Every dot always shows its unit; point at a row to see the study name (on a phone, tap a dot for the tooltip), or tick **Show study names** |
| **By Site** tab | Paired non-local / local marks, one row per site, one shared dollar axis |
| **Sources** tab | Why units aren't normalized (folded shut by default; it prints open), tier definitions (also folded; each of the three tiers is a switch that shows or hides its studies, the same switch as the chips in Filters), the citation table (each note folds behind *Show note*; *Show all notes* opens them), then two folded sections: studies held out for currency, and deliberate exclusions. Every study also has **The story** chevron: a short piece of background on the *place* (Anniston gets three paragraphs, so does Bentonville). It is background written from public sources, not a sourced figure, and is labeled as such |
| **Theme switch** (top right) | Four palettes: **Daylight** (cool white, orange/blue — the most colorblind-safe; the default), **Ledger** (cream), **Neon** (1990s blacklight) and **Dusk** (warm dark, low glare). Swatches only on a phone, names from 760px up. Remembered per browser; the tool works fine if that storage is blocked |
| **Share** (top right) | Copies or hands off the **PMR** URL, with the tab you are on appended |
| **Print** (top right, printer icon) | Opens the browser's print dialog, which is also *Save as PDF*. Separate from Share. Notes are opened and the Range chart is resized for the page automatically; see *Printing and PDF* below. On a 320px phone the top bar wraps to two rows to fit it |
| **← PMR** (top left) | Back to the site. `target="_top"`, so it escapes the embed |

Hover or tap any bar or dot for the full citation and caveat.

The source table carries both cohorts as their own columns — **Non-local /
out-of-town** and **Local / resident** — with the same swatch, unit badge and
"not reported" wording the charts use, so nothing about which figure is whose
depends on having read the chart first. On a phone each study stacks into a
labelled card instead of a six-column table, which reads better and is also the
only layout that doesn't pan the page sideways (see `CLAUDE.md`).

### Printing and PDF

File → Print (or *Save as PDF*) from the open tab. The print layout is its own stylesheet and changes nothing on screen.

- **What prints:** the tab you are on. The filter line (*Showing 14 of 21 · MTB only · tiers V, M*), the legend and the units caveat all print, so a page says what it is filtered to. The buttons, tabs, theme switch and Share do not.
- **Sources tab:** every note is opened for the print and closed again afterwards. Each source's web address is printed under its citation, since a link does nothing on paper. The table header repeats on each page, and each note runs full-width under its row.
- **Colors:** all four themes print on white paper and keep their own title face and hue family, with colors re-tuned for ink. The two cohorts also differ in *lightness* (2.1:1 or more in every theme), so Local and Non-local stay distinct on a black-and-white printer. Every print color is at least 4.5:1 on white.
- **Range view:** redrawn at page width while printing so its labels print at about 8.5pt rather than 7pt.
  - The zebra rows have a thin, slightly darker grey line on their top and bottom edges.
  - **Show study names** ticked: on paper the names print in their own left-justified column, with the plot to its right (there is no hover on paper). Unticked, a row prints only its unit, as before. On screen the names still sit beside the dots.
- **Boxes:** the filter line (*Showing 14 of 21 · …*) is outlined, and the legend, the View line and its one-sentence explanation share one outline.
- **Footer:** prints the full address, `www.postmillenniumrenaissance.com/studies/`. The address in the bottom-left corner of some prints (for example `postmillennium-mtb.github.io/studies/`) is the *browser's* own footer, not the page's; turn **Headers and footers** off in the print dialog to remove it.
- **In the print dialog:** turn **Headers and footers** off (the page prints its own page numbers in Chrome and Edge 131+) and leave **Background graphics** on or off as you like — the tool forces its own bars and badges to print either way. Portrait works for every view; landscape also works, it just leaves more white.

### Deep links

Every tab has its own address, and the Share button builds it for you:

| Link | Opens on |
|---|---|
| `…/studies/` | By Site, Range view (the default) |
| `…/studies/#range` | The same Range view (this was its own tab; old links still land on the same plot) |
| `…/studies/#log` | By Site, Log view |
| `…/studies/#linear` | By Site, Linear view (the bars) |
| `…/studies/#sources` | Sources |

These work through the PMR wrapper page, which forwards the hash into the frame
on load and posts later changes across. In a Pinkbike embed there is no such
bridge and the tool simply opens on its default tab.

## Reading the bars

A solid bar is a single reported figure. Where a source gave a **range**, the bar is
solid out to the low bound and hatched from there to the high bound — so GMUG's
$329–$617 reads as a range, not as "$617".

Bar length is comparable **within** a unit, not across units. Coldwater's $138.49/day
and Custer Gallatin's $146.01/trip render as near-identical bars and are not the same
claim. The unit badge on every value is load-bearing; read it.

## Data source and confidence

Every row carries a `conf` tier. This is an honesty mechanism, not cosmetic metadata.
The badge is a letter, a word and a fill — **V** solid, **M** tinted, **L** a dashed
outline — and deliberately **no color**: the tiers used to be green, amber and red, and
the green was the Local bar's green.

| Tier | Meaning | Count |
|---|---|---|
| **V** — Verified | A primary study's own direct rider survey | 7 |
| **M** — Medium | A blended/synthesis figure across trails, or reached only through a secondary compilation whose original study wasn't independently reviewed | 11 |
| **L** — Low | Borrowed from an unrelated benchmark, a heavy generalizing assumption, a proxy not specific to mountain biking, or a figure that could not be traced back to the source cited beside it | 3 |

The majority of this dataset is **Medium**, largely because the TPL Green Paper (2025)
appendix is the only route to several site figures and those original reports have not
been read directly. That is stated on every affected row rather than averaged away.

Rows marked `calc` carry arithmetic performed on a figure the source printed (for
example, dividing an annual total by trips per year). The row's note writes out the
arithmetic and says why it was done: to put the figure on the same footing as the rows
beside it, because the source printed no figure in that unit. Where a source printed no rate and none could be derived from its own
numbers, the field is left empty rather than estimated.

## Known open items

- **Chequamegon / CAMBA is resolved.** It used to be tiered **Low** with
  `PROVENANCE UNRESOLVED`: the $245.40/day read through the TPL Green Paper could not be
  traced to anything, and it turns out that figure, and the Green Paper's two per-visit
  numbers, appear nowhere in the original report. The row now comes from Hadley &
  Trechter 2020 itself (Table 1-3, p.16): **$200/day**, a mean, *expected* spending by
  non-local visitors, which the report's authors derived by dividing average trip
  spending by days in the area (nights plus one). The row is **Verified**.
- **`yr` is publication year.** Four rows previously carried a different year than the
  citation beside them and were corrected to the cited year (WVU ×2 → 2019,
  Manti-La Sal → 2022, Bentonville → 2023). If any of those were deliberately recording
  a *survey* year, the fix is a separate `surveyYr` field, not a change to `yr`.

## Held out of the charts — other currencies

Studies reported in a currency other than USD are read, tiered and listed in the
Sources tab, but **not plotted**. The chart shares one dollar axis, and a figure
in another currency drawn on it reads as directly comparable when it isn't.

Currently one: **Nelson–Tasman, NZ** (BERL 2018 for Nelson City Council).
NZ$150/day non-local, no resident figure, tiered **Low**. That tier is the whole
story — the NZ$150 is an analyst assumption stated three times in the report and
never sourced, it is per *visitor* where the visitor count adds a non-riding
partner for every other rider, and the "retained expenditure" that looks like a
local figure is the same assumption applied to residents travelling *out* of the
region. It is a good worked example of what a headline "$17.1 million economic
impact" is actually built on.

A normalized-currency view is the plan once there are a few more. It needs an
exchange-rate table, and a rate is only honest if the screen says which one and
as of when — year-of-study and present-day rates answer different questions.

## Excluded on purpose

Named in the Sources tab, with reasons: Marquette MI (qualitative, no spending survey),
Iowa 2025 (commuting focus, no isolated out-of-state MTB figure), Tasmania (relative
figure only, in AUD), and the TPL Green Paper's own Coldwater appendix entry (could not
be traced into the underlying Boozer et al. 2012 text). A gap beats a guess.

## What this tool does not do

- **It does not tell you what a rider will spend at your trail.** These are 21 figures
  from other places, in other years, measured different ways.
- **It does not adjust for inflation.** Figures are nominal, as published, 2012–2023.
- **It does not normalize units**, and resists doing so. Forcing everything to "per day"
  would require inventing an average trip length for studies that never reported one.
- **It is not a substitute for a local survey.** Its best use is bounding a projection
  and showing a council how wide the published range actually is.

## Relationship to the Trail Investment ROI Calculator

The [ROI Calculator](https://postmillenniumrenaissance.com/roi/) takes a per-visitor
spending assumption as an input. This tool is the evidence base behind that number.
For reference, the median of the MTB-specific *per-day* non-local figures here is
**$143.27** (n=5, range $90–$200).

## Files in this repository

```
index.html              the tool — single file, zero dependencies, opens by double-click
README.md               this file
CLAUDE.md               conventions and traps for anyone (or anything) editing index.html
favicon.ico             \
favicon-32x32.png        }  referenced relatively; served from a github.io subpath
apple-touch-icon.png    /
```

## Working with Claude on this file

Read `CLAUDE.md` first — it has the conventions, the one-line edit paths, and the
list of comments in `index.html` that are recorded traps rather than decoration.

To add sites: paste the current `index.html` into a new chat along with your candidate
studies, and ask for entries following the same sourcing discipline as the existing
rows — one line per site in the `SITES` array, a page number in `src`, an honest `conf`
tier, and a `note` naming the caveat. A gap is better than a guess; if a figure can't
be traced to a page, it doesn't go in, or it goes in tiered **L** and flagged.

The file's own header comment is a file map. Read it before editing.

## License and attribution

Compiled by Jon Lontai for Post Millennium Renaissance.
Underlying figures belong to their cited authors and publishers.

## Contact and corrections

Corrections are welcome and wanted. Open an issue on this repository.
