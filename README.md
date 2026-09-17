# Trail Visitor Spending Ledger

What non-local and local mountain bikers actually spend at trail systems — compiled
from published economic-impact studies, shown exactly as each source reported it.

**Live:** https://postmillennium-mtb.github.io/studies/
**On PMR:** https://postmillenniumrenaissance.com/spending-ledger/

## What this tool does

Puts 21 site-level spending figures from 16 published studies side by side, without
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
| **Sources & Methodology** tab | Tier definitions, full citation table, deliberate exclusions |

Hover or tap any bar or dot for the full citation and caveat.

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
| **M** — Medium | A blended/synthesis figure across trails, or reached only through a secondary compilation whose original study wasn't independently reviewed | 12 |
| **L** — Low | Borrowed from an unrelated benchmark, a heavy generalizing assumption, a proxy not specific to mountain biking, or a figure that could not be traced back to the source cited beside it | 3 |

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
favicon.ico             \
favicon-32x32.png        }  referenced relatively; served from a github.io subpath
apple-touch-icon.png    /
```

## Working with Claude on this file

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
