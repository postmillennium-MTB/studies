# Trail Visitor Spending Ledger — instructions for Claude

This repo is one file. `index.html` is the whole tool: markup, CSS, JavaScript and
data, no build step and no dependencies. It must keep running by double-clicking it.

**Read the file-map header comment at the top of `index.html` before editing
anything.** It is a table of contents plus a maintenance manual, and its "TO ADD…"
lines give the one-line edit paths.

There is a `pmr-build-standard` skill. Load it before any substantial change — this
file is the repo-specific layer on top of it, not a replacement for it.

## Who this is for

Jon has no coding background and edits through GitHub's web UI, not git.

- **Deliver complete files, not diffs.** He pastes whole files into GitHub's editor.
  A patch fragment is unusable. For a large file, say what changed and where, then
  give the whole thing.
- **Never say "add this to your CSS."** Name the file and hand over the finished
  result.
- **Name what a choice costs, not only what it buys.** No solutions, only tradeoffs.
- **Ask before restructuring.** A refactor that touches the whole file means he has
  to re-paste and re-verify the whole file. Sometimes worth it; always his call.

## Where it lives

| Thing | Where |
|---|---|
| Tool repo | `postmillennium-MTB/studies` (lowercase — renamed from `Studies`, and Pages paths are case-sensitive) |
| Served from | `https://postmillennium-mtb.github.io/studies/` |
| Site wrapper | `postmillennium-MTB/pmr-website` → `studies/index.html` |
| **Canonical URL** | `https://www.postmillenniumrenaissance.com/studies/` |

The canonical host is the **www** form: it matches the `CNAME` file in
`pmr-website` and the sitemap, so a shared link never eats a redirect. github.io is
an origin, not an address to hand anybody — `<link rel="canonical">`, `og:url` and
the Share button all point at PMR.

**This tool always runs inside a cross-origin iframe** — the PMR wrapper page, and
Pinkbike articles under the handle `redfoxrun`. Everything below about `target="_top"`,
permission policy and the postMessage bridge follows from that.

## Structure

```
<head>
  pre-paint theme shim ...... the ONLY logic outside the main script; see below
  <style>  theme palettes (default FIRST, and why), then components
<body>
  utility bar ............... PMR home link · theme switch · share
  masthead, tab bar, filter bar, panels — mostly empty shells that JS fills
  <script>
    ==== one contiguous DATA block ====
    SITES · CONF_META · UNIT_META · link icons · FOREIGN_SITES
    SHARE/HOME URLs · PMR_ORIGINS · EXCLUSIONS · THEMES · TABS
    /* ===== END OF DATA — everything below is logic ===== */
    tuning constants, render functions, theme, share, deep links, init()
```

## The rules that matter here

**One file.** No framework, no build step, no `package.json`, no CDN link, no
splitting CSS or JS out. The single file is the point.

**All data above `END OF DATA`.** Content — figures, labels, citations, URLs, theme
palettes, tab definitions — lives in that block. Logic lives below it. A label typed
inline in a render function, or a threshold living inside an `if`, is a bug.

**Anything repeating is a registry.** `SITES`, `FOREIGN_SITES`, `EXCLUSIONS`,
`CONF_META`, `UNIT_META`, `THEMES`, `TABS`. Adding one of a thing should be one line:

| To add | Do |
|---|---|
| a site | one entry in `SITES` |
| a non-USD study | one entry in `FOREIGN_SITES` — same field contract plus `cur` and `curSym`. It lists in the Sources tab and is held out of the chart. Read that array's comment before wiring up a normalized-currency view |
| a tab | write the render function, add one line to `TABS` (it becomes shareable at `/studies/#<id>` for free) |
| a theme | one entry in `THEMES` + one CSS `[data-theme="…"]` block. Nothing else — not even the pre-paint shim |
| an exclusion | one entry in `EXCLUSIONS` |

**Mobile-first.** Base styles are the phone; `@media (min-width: …)` adds the desktop.
Touch targets ≥ 40px.

**Never color alone — and tiers have no color at all.** A confidence tier is a badge
**fill treatment** (solid / tinted / dashed outline, in the theme's own ink) **plus** a
letter **plus** a word. It used to be green/amber/red; the green was the Local bar's
green and the red sat next to the Non-local bar's, so "verified" read as "local".
Every bar carries its cohort as a text label. Every value carries its unit badge.
Themes move the cohort colors, which is only safe because none of the meaning ever
rested on the color.

## Sourcing discipline — the part that is not negotiable

This tool's entire value is that it does not inflate certainty.

- **Never invent a figure, and never invent a URL.** A row with no verified link
  renders as plain text. Several rows are unlinked on purpose.
- **Every figure traces to a page number in a named study**, or is marked `calc`
  with the arithmetic written out in its note.
- **A gap beats a guess.** If a source printed no rate and none can be derived from
  its own printed numbers, the field stays empty.
- **Tier honestly.** `conf` grades *how the number was obtained*, not how good the
  study or the trail is. Chattanooga is `V` on a narrow study; that is correct and
  has been questioned once already.
- **Units are not normalized, and neither is currency.** Forcing everything to "per
  day" would require inventing an average trip length for studies that never reported
  one. Non-USD studies go in `FOREIGN_SITES` and are held out of the chart: the
  chart shares one dollar axis, and a figure in another currency drawn on it reads as
  directly comparable when it is not. A normalized view is wanted eventually, and it
  needs an FX table — but "normalized" means nothing until the screen says *which*
  rate and *as of when*. Year-of-study and today's rate answer different questions.
  Never pick one silently.
- **"via TPL" rows link the compilation as the primary source and the original study
  as a second, differently-iconed link.** The citation must point where the number
  was actually read. The two documents did not get the same scrutiny and must never
  look interchangeable.
- **Do not host study PDFs in this repo.** Copyright exposure on consultancy and
  university reports, and the credibility cost is worse than the legal one. Use
  Wayback snapshots — `archiveUrl` and `origArchiveUrl` are wired and take precedence
  over the live URL when populated.

## Recorded traps — do not "clean these up"

Several comments in `index.html` mark bugs that already happened. Deleting one to
tidy up reintroduces the bug.

- **CSS theme order.** `:root` and `[data-theme="x"]` have identical specificity,
  so whichever is declared last wins. The default theme's block (**Daylight**) goes
  **first** and carries the bare `:root` selector, which is also what paints before JS
  runs. Every other theme is a plain `[data-theme]` block after it and must state
  *every* variable itself, or it inherits Daylight's. Changing the default means
  moving that selector, moving the block, giving the old default a `[data-theme]`
  selector, setting `DEFAULT_THEME`, and updating the `theme-color` meta.
- **`--card-glow` is a transparent shadow, not `none`.** It sits inside
  comma-separated `box-shadow` lists, and `none` is illegal as a list item — it would
  kill the whole rule.
- **`fmtMoney` rounds to cents before deciding on decimals.** A naive trailing-zero
  strip turned $245.40 into "$245.4".
- **`TABS[].render` is a function name string, not a reference.** The render functions
  are declared further down the file; looking them up by name at click time dodges a
  declaration-order trap.
- **A range bar draws two segments,** solid to the low bound and hatched beyond it.
  Drawing only the high bound made GMUG's $329–$617 read as a flat "$617".
- **An unrecognized `conf` stays visible** (`filterTiers[s.conf] !== false`, not
  `=== true`) and renders a grey `?` badge instead of throwing. A typo in one data row
  used to blank the whole tab.
- **The ranges control is read from the registry in `init()` too,** not only in the
  click handler. It used to be correct on first load purely by luck of `TABS` order.
- **The source table always prints the full reported range,** ignoring
  `filterShowRanges`. That control is hidden on the Sources tab, so a viewer who
  switched it off on a chart would otherwise see bare low bounds with no way back.
- **Never put a `min-width` on `.srctable`, and never let it grow much past ~460px
  of intrinsic width on a phone.** A wide `table-layout:auto` table inside
  `overflow-x:auto` does *not* stay inside it: Chromium propagates the table's
  intrinsic width to the root scroller and the whole page pans sideways (166px at
  360px wide, measured). `max-width:100%` on the wrapper, `overflow:hidden` on the
  card, `grid-template-columns:minmax(0,1fr)`, and `contain` all fail to stop it.
  `table-layout:fixed` stops it and gives six 50px columns; `overflow-x:hidden` on
  `<html>` only blocks the *user* from panning and is unreliable on iOS. The fix in
  place is that phones get stacked cards instead of a table. **And beware the probe:
  `document.body.scrollWidth` stays at 360 and reports no problem — only
  `window.scrollX` after an attempted scroll catches this.**
- **Never hide a tooltip on `mouseleave`.** A browser synthesizes compatibility
  mouse events after a tap, in this order:
  `pointerdown → touchstart → touchend → mousemove → click → mouseleave`.
  A `mouseleave → hideTip` therefore fires straight after the `click` that showed
  it, and the tooltip never appears on a phone at all — which silently made every
  citation and caveat unreachable from both charts. Hover is gated on
  `pointerType === "mouse"` via POINTER events, which the synthesized mouse events
  cannot reach; the tap path reads its type from `pointerdown`, not from the
  click, because a click is not a PointerEvent in every browser. All of it lives
  in `wireTipTarget`, used by both the bars and the range dots — it was duplicated
  before, so this bug existed twice.
- **`showTip` clamps to the viewport LAST, on both axes, after the flips.**
  Flipping away from the right edge without clamping walks off the left one: on a
  375px screen a 270px tooltip flipped left for any tap past x=87 — 52% of the
  width — and landed at x=-175, two thirds off-screen with the text sheared
  mid-word. Flipping alone is only safe while the viewport is far wider than the
  tooltip.
- **The theme preference is stored as `"id|scheme"`, not just the id.** That is what
  lets the pre-paint shim in `<head>` set both the palette and the light/dark scheme
  without knowing the `THEMES` registry exists. The storage key string is duplicated
  in the shim on purpose; both sides say so.

- **The bar value column is a fixed width (`--val-w`).** Each bar is a percentage of
  its own track, and the track is whatever the row has left after the value beside it.
  With a value column that sized itself to its text, tracks measured 166–211px at 375px
  wide, so the same dollar amount drew up to ~27% longer or shorter depending on the
  row. The Log axis row reads the same `--lab-w` / `--val-w` so its ticks land over the
  tracks. Widen the variable if a value overflows; never let the column size itself.
- **The Range view is SVG measured at render time, and the 22 is the card.** `renderRangeView` takes the panel's width minus 22 (the `.rangewrap` card's 10px padding and 1px border, both sides). Change that padding and change the 22. It redraws only when the width really changes (a rotation), not when a phone's address bar resizes its height; the tap targets are invisible 12px circles (half a row, so neighbours never overlap), drawn last inside each row so no label or dot paints over them.
- **Every dot in the Range view shows its unit at all times; only the study NAME is hover-only.** The old Range tab's bare dots broke the rule that every value carries its unit, and hiding the name must not bring that back. The unit is its own `<text>` and the name (`.stripname`, via `fitName`) is a second one beyond it, so truncating the name can never drop the unit. Each row is one `<g class="striprow">` so that pointing at any part of it, not just the dot, reveals the name. There is deliberately no fade: it lags a cursor sweeping down the plot. The filter bar's *Show study names* checkbox (off by default, `filterShowNames`) forces every name visible via a `shownames` class on the SVG; it appears only while the active view has `names:true` in `AXES`, and `setAxis()` re-syncs it.
- **Defaults live in three named places:** `DEFAULT_THEME` (Daylight), `DEFAULT_AXIS`
  (Range) and `CONF_META[k].shown` (Low starts unticked). The default view's address
  is the bare URL, so `shareUrl()` returns no hash for it; Linear has its own
  `#linear` alias now. Do not reintroduce "the first entry is the default" — it is why
  reordering the View switch used to change what the tool opened on.
- **`#range` is the Range view, `#log` is the Log view.** `#range` was briefly pointed at Log during Pass 2, which was wrong: existing links to it meant the two-lane plot.
- **The Log axis draws dots, never bars.** A bar's length is its value from a baseline,
  and a log axis has no zero, so a log-scaled bar's length means nothing. Dots claim
  only a position, which is all a log axis can honestly give.
- **Confidence badges use `--tb-ink` / `--tb-ground`,** redefined inside `.tooltip`,
  because the tooltip is dark on the Ledger and a badge drawn from the page's ink
  would be invisible there.
- **The theme-name labels appear at 760px, not 640px.** Four labelled pills plus the
  full "Post Millennium Renaissance" link overflow a 640px screen by 2px and pan the
  page. Below 760px the switch is swatches only, and `aria-label` carries the name.
- **`.filterbody[hidden]{display:none}` is load-bearing.** `.filterbody` is `display:flex`,
  which outranks the `hidden` attribute; without the explicit rule the folded filter
  controls stay visible.
- **`--head-font` must never be set to `inherit`.** On a custom property that keyword
  means "take the parent's custom property", not "use the body font". Write the stack out.
- **The tabs' ARIA is one piece.** `role=tab` + `aria-selected` + `aria-controls` +
  `role=tabpanel` + roving `tabindex` + the arrow-key handler work together. Removing
  any one leaves a worse widget than having none.

## Iframe consequences

- **The PMR home link needs `target="_top"`.** Without it the whole site loads inside
  the embed. This has bitten before.
- **External links:** `target="_blank" rel="noopener noreferrer"`.
- **The Web Share API and the async clipboard are gated on permission policy in a
  cross-origin frame.** The PMR wrapper sets `allow="web-share; clipboard-write"`.
  A Pinkbike embed does not, which is exactly why the share path falls back to
  `navigator.clipboard` and then to `document.execCommand("copy")`. Do not delete the
  old `execCommand` path; it is the only one that survives with no permission policy.
- **A hash on the outer page cannot be read from in here.** Two mechanisms cover it:
  the wrapper appends the hash onto the iframe `src` at load, and posts a `pmr-hash`
  message on later changes. **Always check `e.origin` against `PMR_ORIGINS` before
  acting on any message.** On Pinkbike neither fires and the tool opens on its default
  tab — that is the required behaviour, not a degraded one.
- **The tool tells the wrapper its theme** (`pmr-theme`) so the wrapper can repaint
  its own background and `theme-color`. The wrapper cannot read this origin's
  `localStorage`, so it has to be told.
- **`localStorage` is a preference store only.** The tool must work identically with
  it empty, blocked or full of junk — which it will be in a third-party iframe on
  Safari. Every read and write is wrapped in `try/catch`.

## Before handing anything back

- Opens correctly by double-clicking the local file
- Renders at phone width with **no horizontal page scroll** — check with
  `window.scrollTo(9999,0)` then read `window.scrollX`, on every tab, at 320px and
  360px. `body.scrollWidth` will lie to you. Touch targets ≥ 40px
- Every theme switches cleanly, and the default paints correctly before JS runs
- New palette values clear WCAG AA (4.5:1) against both `--paper` and `--card`
- Home link `target="_top"`; external links `target="_blank" rel="noopener"`
- Favicons stay relative (no leading slash) — this is served from a github.io subpath
- Deep links work directly and through the wrapper
- Data block still ends at `END OF DATA` with no content below it
- File-map header still matches the file's actual sections
- README updated, including any counts quoted in it

## Known open items

**Data** — none of these figures has been verified against its source document;
the sessions that built this could not open external PDFs.

- **Eastern Trail.** The row says $242.35/day; the Eastern Trail Alliance's own page
  says $118/day across 251,978 users. Roughly 2×. Possibly all-users vs.
  non-local-only, possibly a TPL transcription error. The highest-value thing to check.
- **CAMBA `PROVENANCE UNRESOLVED`.** $245.40/day cannot be traced to the cited TPL
  page ($1,107/visit narrative, $1,017.18/visit appendix). Tiered `L` and flagged.
  The original UW–River Falls report is linked, so this is resolvable by reading it.
- **Manti-La Sal author list.** Cited as "Maples, Rehm & Bradley 2022"; Outdoor
  Alliance names only Maples and Bradley. Unverified.
- **Nelson–Tasman (BERL 2018) is the one row read against its source document.**
  It is in `FOREIGN_SITES`, tiered `L`, held out of the charts. Its NZ$150/day is an
  unsourced analyst assumption, stated three times and never cited; it is per
  *visitor*, where the visitor count adds a non-riding partner for every other rider;
  and the report's "retained expenditure" is that same assumption applied to locals
  travelling *out* of the region, so it is not a resident-spending figure and `lo` is
  null. If more foreign studies are added, this is the row to model them on.
- **Zero Wayback snapshots exist.** Every link is a live publisher URL carrying rot
  risk. `archiveUrl` / `origArchiveUrl` are wired and take precedence once populated.
- **Five rows unlinked:** `pikes-overnight`, `pikes-daytrip`, `central-ohio`,
  `james-river`, `bentonville`.

**Build**

- **No headline stat.** "11 of 15 MTB studies never surveyed locals" is the strongest
  finding in the dataset and is currently only visible by scrolling past "not
  reported" eleven times.
- **Accessibility.** The tabs are now a proper ARIA tab set (`aria-selected`,
  `role="tabpanel"`, roving `tabindex`, arrow/Home/End keys). What is still missing is
  keyboard access to the chart marks themselves — bars and dots open their tooltip on
  hover or tap only. The Sources tab carries every note, so no content is lost, but the
  chart is mouse- and touch-only.
- **Cross-unit comparison tension.** On By Site, $138.49/day and $146.01/trip render
  as near-identical bars on a shared linear axis (and as near-identical dots on the log
  one). Grouping the rows by unit would remove the invitation; not done. The filter-bar note warns against
  the comparison the geometry invites. Considered and accepted; an open design
  question, not a bug.
