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
| **MTB-specific studies only** | Hides the 6 general-cycling / paved-trail comparison entries |
| **Show reported ranges** | Draws the high bound where a source gave a range instead of one number |
| **By Site** tab | Paired non-local / local bars, one row per site, shared linear $ axis |
| **Range (all studies)** tab | Log-scale strip plot of every reported figure |
| **Sources & Methodology** tab | Tier definitions, the citation table, deliberate exclusions |
| **Theme switch** (top right) | Two palettes: **Ledger** (cream, the default) and **Neon** (1990s blacklight). Remembered per browser; the tool works fine if that storage is blocked |
| **Share** (top right) | Copies or hands off the **PMR** URL, with the tab you are on appended |
| **← PMR** (top left) | Back to the site. `target="_top"`, so it escapes the embed |

Hover or tap any bar or dot for the full citation and caveat.

The source table carries both cohorts as their own columns — **Non-local /
out-of-town** and **Local / resident** — with the same swatch, unit badge and
"not reported" wording the charts use, so nothing about which figure is whose
depends on having read the chart first. On a phone each study stacks into a
labelled card instead of a six-column table, which reads better and is also the
only layout that doesn't pan the page sideways (see `CLAUDE.md`).

### Deep links

Every tab has its own address, and the Share button builds it for you:

| Link | Opens on |
|---|---|
| `…/studies/` | By Site |
| `…/studies/#range` | Range (all studies) |
| `…/studies/#sources` | Sources & Methodology |

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

| Tier | Meaning | Count |
|---|---|---|
| **V** — Verified | A primary study's own direct rider survey | 6 |
| **M** — Medium | A blended/synthesis figure across trails, or reached only through a secondary compilation whose original study wasn't independently reviewed | 11 |
| **L** — Low | Borrowed from an unrelated benchmark, a heavy generalizing assumption, a proxy not specific to mountain biking, or a figure that could not be traced back to the source cited beside it | 4 |

The majority of this dataset is **Medium**, largely because the TPL Green Paper (2025)
appendix is the only route to several site figures and those original reports have not
been read directly. That is stated on every affected row rather than averaged away.

Rows marked `calc` carry arithmetic performed on a figure the source printed (for
example, dividing an annual total by trips per year). The arithmetic is written out in
the row's note. Where a source printed no rate and none could be derived from its own
numbers, the field is left empty rather than estimated.

## Known open items

- **Chequamegon / CAMBA ($245.40/day)** is tiered **Low** with `PROVENANCE UNRESOLVED`
  in its note. The figure cannot be reconciled to either number on the cited TPL page
  ($1,107/visit narrative, $1,017.18/visit appendix). It stays visible and flagged
  rather than quietly dropped or quietly kept. Resolving it requires reading Hadley &
  Trechter 2020 directly.
- **`yr` is publication year.** Four rows previously carried a different year than the
  citation beside them and were corrected to the cited year (WVU ×2 → 2019,
  Manti-La Sal → 2022, Bentonville → 2023). If any of those were deliberately recording
  a *survey* year, the fix is a separate `surveyYr` field, not a change to `yr`.

## Held out of the charts — other currencies

Studies reported in a currency other than USD are read, tiered and listed in the
Sources tab, but **not plotted**. Both charts share one dollar axis, and a figure
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
**$143.27** (n=5, range $90–$245.40).

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

Corrections are welcome and wanted — particularly on the CAMBA provenance item above.
Open an issue on this repository.
