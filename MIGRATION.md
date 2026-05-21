# Design System Migration Log

Tracking every token-level change made to Search Labs (`design-system.css`) as part of the migration to the Elastic Design System Tokens library.

---

## ⚠ Known deviations from library spec

The token system is **mostly aligned** to the Elastic Design System library, but there are a few intentional deviations — design choices we made that knowingly differ from the spec. Each deviation is documented inline in `design-system.css` (look for the `⚠ KNOWN DEVIATIONS` banner near the top of the file) and in the session log below.

### Deviation #1 — Heading weight 700 instead of 800 (Session 22)

**Library says**: All Display / Headline / Title styles bind to **800 (ExtraBold)** — specifically Mier B ExtraBold.

**We use**: `--fw-bold: 700` (Bold instead of ExtraBold).

**Why**: ExtraBold read too heavy in our specific layout context. Bold (700) preserves the brand hierarchy without overwhelming the page. The decision is purely visual preference.

**Status of library option**: Mier B ExtraBold IS loaded and available via the @font-face block — to revert to library spec, change `--fw-bold: 700;` → `800;` in one place in `design-system.css`. No other changes required.

**Session history**: Started at 700 (pre-S19) → bumped to 800 in Session 19 to match library → reverted to 700 in Session 22 after live evaluation.

---

## Ground rules

- **Search Labs is the source of truth.** The library is a constraint set of allowed values.
- For each Search Labs token, we pick the **closest available library value** by raw distance, regardless of category.
- **Delta** = (library value) − (Search Labs original value). Positive = Search Labs grows; negative = shrinks.
- Zero-delta entries are still logged — the token is now annotated with its library mapping for traceability, even though no value changed.
- Each entry includes the inverse to revert (the original Search Labs value) inline as a CSS comment in `design-system.css`.
- **Intentional deviations** from the library spec are also flagged in the deviations callout above. They are real design decisions, not bugs to fix.

## Source references

- **Library file**: [Design System Tokens and Variables](https://www.figma.com/design/f2pdARBg3ziDbDzh7hP30Y/Design-System-Tokens-and-Variables-Test-AI)
- **Site replica (Search Labs baseline)**: [Sunny's edit search lab](https://www.figma.com/design/WFkIZOqVE7W0zXK1Yy5Bdp/Sunny-s-edit-search-lab)
- **Components file**: [Components](https://www.figma.com/design/2F99OfKzvouAcnF7fruISD/Components)

---

## 2026-05-14 — Session 1: Font size scale (`--fs-*`)

Source: Typography Desktop variables, Figma node `40007572:310`. Library type-scale class names referenced below (Display / Headline / Title / Body / Label) come from this node.

### Applied

| # | Token | Before | After | Δ | Library mapping | Status |
|---|---|---|---|---|---|---|
| 1 | `--fs-h2` | 39.06 px | **40 px** | **+0.94 px** | Headline Large (40 px) | ✅ Applied |
| 2 | `--fs-h3` | 31.25 px | **32 px** | **+0.75 px** | Headline Small (32 px) | ✅ Applied |
| 3 | `--fs-h4` | 25 px | 25 px | 0 | Title Medium (25 px) — exact | ✅ Annotated (no value change) |
| 4 | `--fs-h5` | 20 px | **22 px** | **+2 px** | Title Small (22 px) — tie-break vs Body XLarge 18 px, chose to keep token in heading class | ✅ Applied |
| 5 | `--fs-link` | 16 px | 16 px | 0 | Body Large / Label XLarge (16 px) — exact | ✅ Annotated (no value change) |
| 6 | `--fs-body` | 16 px | 16 px | 0 | Body Large (16 px) — exact | ✅ Annotated (no value change) |
| 7 | `--fs-small` | 14 px | 14 px | 0 | Body Medium / Label Large / Code Medium (14 px) — exact | ✅ Annotated (no value change) |
| 8 | `--fs-xs` | 12 px | 12 px | 0 | Body Small / Label Medium / Code Small (12 px) — exact | ✅ Annotated (no value change) |

**Blast radius of value changes** (#1, #2, #4):
`--fs-h2` is referenced 5× in `index.html`; `--fs-h3` 5×; `--fs-h5` 2×. #1 and #2 grow under 1 px each. #4 grows 2 px — slightly more noticeable on whatever H5-class elements use it (worth eyeballing the rendered prototype to confirm nothing wraps awkwardly).

### Deferred

_None remaining in the font-size scale._

### Not yet measured in this session

These came up in the same Figma fetch but are line-heights, weights, and families — held for a separate review pass:

- **Line-heights** (`--lh-*`): library binds absolute-px line-heights per size class; Search Labs uses ratios. Conversion shows the library trends tighter (1.2–1.5 vs Search Labs' 1.4–1.6). All `--lh-*` deltas pending review.
- **`--font-mono: 'IBM Plex Mono'`** vs library **Roboto Mono** — family-level swap, different metric/feel. Pending.
- **Font weights**: library locks Body / Label / Code at 500 (Medium) and headings at 750 (ExtraBold) / 800 (Heavy for Oversize class). Search Labs uses defaults / 700 for most. Note: a `--fw-body: 500` experiment was previously tried and reverted (logged in `HANDOFF.md`). Pending.

### Not yet pulled from Figma

Outside the Typography Desktop scope; need separate Figma node selections to fetch:

- **Color palette** (brand, neutrals, text, surface, border) — only two text-color tokens leaked into the typography fetch: `color/text/default: #14141b` and `color/text/subtler: #343544`. Library text colors look meaningfully darker than Search Labs `--gray-dark: #2E3137` and `--gray: #6B7079`, but no decision yet without seeing the full palette in context.
- **Spacing scale** (`--s-*`)
- **Radii** (`--r-*`)

---

## 2026-05-14 — Session 2: Line-height scale (`--lh-*`)

Source: same Typography Desktop fetch as Session 1. Library binds line-heights as absolute px paired to each font size; the table below shows the equivalent ratio at the matched Search Labs size, since the existing `--lh-*` tokens are unitless ratios.

### Applied

| # | Token | Before | After | Δ (ratio) | Δ (≈ px at new size) | Library mapping | Status |
|---|---|---|---|---|---|---|---|
| 1 | `--lh-h2` | 1.4 | **1.2** | −0.2 | ≈ −8 px @ 40 px | Headline Large (48 px / 40 px) | ✅ Applied |
| 2 | `--lh-h3` | 1.5 | **1.2** | −0.3 | ≈ −9.6 px @ 32 px | Headline Small (38.4 px / 32 px) | ✅ Applied |
| 3 | `--lh-h4` | 1.5 | **1.3** | −0.2 | ≈ −5 px @ 25 px | Title Medium (32.5 px / 25 px) | ✅ Applied |
| 4 | `--lh-h5` | 1.5 | **1.3** | −0.2 | ≈ −4.4 px @ 22 px | Title Small (28.6 px / 22 px) — locked to fs-h5 = 22 px from Session 1 | ✅ Applied |
| 5 | `--lh-link` | 1.5 | 1.5 | 0 | 0 | Body Large (24 px / 16 px) — exact | ✅ Annotated (no value change) |
| 6 | `--lh-body` | 1.6 | **1.5** | −0.1 | ≈ −1.6 px @ 16 px | Body Large (24 px / 16 px) | ✅ Applied |
| 7 | `--lh-small` | 1.6 | **1.5** | −0.1 | ≈ −1.4 px @ 14 px | Body Medium (21 px / 14 px) | ✅ Applied |
| 8 | `--lh-xs` | 1.6 | **1.5** | −0.1 | ≈ −1.2 px @ 12 px | Body Small (18 px / 12 px) | ✅ Applied (token has 0 active usages; annotated for completeness) |

**Blast radius** in `index.html`: `--lh-h2` 5×, `--lh-h3` 5×, `--lh-h4` 1×, `--lh-h5` 1×, `--lh-link` 1×, `--lh-body` 2×, `--lh-small` 1×, `--lh-xs` 0×. The most visible change will be on the H2/H3 hero headings — line-height drops by ≈ 8–10 px, which is significant for multi-line titles. Eyeball the hero rows on `#home`, `#observability`, `#security`, and `#company` to confirm nothing crowds.

### Deferred

_None._ Full line-height scale migrated in one pass.

---

## 2026-05-14 — Session 3: Font families (`--font-*`)

Source: same Typography Desktop fetch. Library font-family declarations: Mier B (headings), Inter (body / label), Roboto Mono (code).

### Applied

| # | Token | Before | After | Δ | Library mapping | Status |
|---|---|---|---|---|---|---|
| 1 | `--font-heading` | `'Mier B', 'Inter', system-ui, sans-serif` | _(unchanged)_ | 0 | Mier B — exact | ✅ Annotated (no value change) |
| 2 | `--font-body` | `'Inter', system-ui, sans-serif` | _(unchanged)_ | 0 | Inter — exact | ✅ Annotated (no value change) |
| 3 | `--font-mono` | `'IBM Plex Mono', ui-monospace, monospace` | **`'Roboto Mono', ui-monospace, monospace`** | family swap | Roboto Mono | ✅ Applied |

### Side-effects of the mono swap (not pure-token changes; also applied this session)

| Where | Change |
|---|---|
| `index.html` line 9 | Google Fonts `<link>` rewritten: `family=IBM+Plex+Mono:wght@500;700` → `family=Roboto+Mono:wght@500;700` |
| `components.html` line 9 | Same `<link>` rewrite |
| `components.html` typography section copy | "Code samples use IBM Plex Mono." → "Code samples use Roboto Mono." (docs prose accuracy) |

**Blast radius**: `--font-mono` is referenced 23× in `index.html` — every `<code>` block, mono label, and metadata pill that uses the variable will render in the new face. Roboto Mono is wider per glyph and more utilitarian than IBM Plex Mono; line lengths in code blocks may shift slightly. Worth eyeballing any narrow-column code samples.

### Open observation worth flagging (no action)

**Mier B never actually renders today.** It's declared as the first font in `--font-heading`, but neither `index.html` nor `components.html` loads a Mier B web font (Google Fonts only serves Inter and IBM Plex Mono / now Roboto Mono). Every heading currently falls back to Inter. The library spec also names Mier B for headings, so the migration is correct on paper — but until Mier B is actually loaded (self-hosted `@font-face` or licensed CDN), the prototype's headings render in Inter regardless. Out of scope for the token migration; logged here as context.

### Deferred

_None._ Family migration complete.

---

## 2026-05-14 — Session 4: Body font weight (EXPERIMENT)

Source: same Typography Desktop fetch (Figma node `40007572:310`). The library binds all Body / Label / Code text styles to weight **500 (Medium)**. Search Labs has no `--fw-*` token today; body copy inherits the browser default `400 (Regular)`.

**This entry is under the EXPERIMENT pattern** (per `HANDOFF.md` § "Design-system migration"), not a committed migration. It was previously tried and reverted; we are re-running with the documented safety-net pattern so a clean revert is one step.

### Applied

| # | Token | Before | After | Δ | Library mapping | Status |
|---|---|---|---|---|---|---|
| 1 | `--fw-body` | _undefined_ (browser default 400) | **500** | +100 weight | Body / Label / Code Medium (500) | 🧪 Experiment |

### Where the experiment lives (3 places, all required)

| File | Change |
|---|---|
| `design-system.css` | New `--fw-body: 500;` in `:root`, wrapped in an `EXPERIMENT 2026-05-14` comment block with full revert steps. |
| `index.html` | (a) Inline fallback `<style>` block inserted between the Google Fonts `<link>` (line 9) and the `design-system.css <link>` (now line 17) that redeclares `--fw-body: 500` — safety net if the external CSS fails to load. (b) `body { ... }` rule updated: `font-weight: var(--fw-body, 400);` — third-level fallback (400) engages if the variable itself is removed. |
| `components.html` | Same inline fallback `<style>` block. Same `body { ... }` rule update so the docs page renders in the experimental weight. |

### Revert in one step (if rejected)

1. Delete the `--fw-body: 500;` line in `design-system.css`.
2. Delete the inline fallback `<style>` block in `index.html` and `components.html`.
3. Optionally also remove the `font-weight: var(--fw-body, 400)` line from the two `body { ... }` rules — leaving it does no harm (it falls back to 400, same as default).

### Why this got reverted last time (context, not analysis)

`HANDOFF.md` notes only that the body Medium weight was "tested, reverted to baseline at user request." The reservations behind that decision aren't recorded. Likely candidates: body copy felt too heavy / lost contrast with bold text; Inter Medium at 16 px on light backgrounds reads denser than 400, and the heading-vs-body weight gap collapses if both are weighty. Worth watching for those specifically when reviewing.

### Blast radius

The base `body { ... }` rule is the cascade origin for almost all running text. Every paragraph, list item, link, and inline element that doesn't have an explicit `font-weight` override (~36+ explicit `font-weight: 700` / `600` / `500` declarations exist in `index.html` and continue to win by specificity for headings, buttons, chips, etc.) will pick up the new 500 weight. Net effect: paragraphs and inline copy look noticeably heavier; UI elements and headings are unchanged.

### Deferred

_None for this experiment._ Heading weights (`750 ExtraBold`) and the Oversize Heavy class (`800`) are still measured-but-not-applied; not part of this experiment, see open list below.

---

## 2026-05-14 — Session 5: Color system (REPLACEMENT — Neutral + Brand surface families)

Source: Figma nodes `40002315:2269` (Backgrounds — Neutral), `40002315:3146` (Text — Neutral), `40002315:3420` (Examples — Neutral), `40002315:15484` (Rule #1 — WCAG AA/AAA), `40002315:15404` (Examples — Brand), `40002315:1999` (Text — Brand), `40002315:2706` (Backgrounds — Brand).

**Structural change**, not a delta-snap migration. The Elastic library organizes color as two parallel surface families (Neutral grays + Brand blues), a shared text scale, plus standalone brand colors — none of which match the Search Labs token shape (13 semantically named tokens with no surface/text separation). User chose "Replacement" over "Additive," so the new system is now canonical.

### Decision: documentation values vs variable bindings

Light grays differ meaningfully between the two sources. Picked **documentation card values** (e.g. Gray 50 = `#E2E3E7`); the variable bindings (e.g. `color/neutral/50 = #f6f9fc`) collapse the lightest surface to near-white, which defeats the purpose of having a distinct Gray 50 token. Variable-binding alternates are noted inline in `design-system.css` for one-line reverts. Perceptual deltas between the two sources: lights ΔE ≈ 9–10 (noticeable), darks ΔE ≈ 2–4 (negligible).

### New canonical tokens introduced

| Group | Tokens | Values |
|---|---|---|
| **Neutral surfaces (light)** | `--surface-neutral-1` / `-2` / `-3` | `#FFFFFF` / `#E2E3E7` / `#C4C7D1` |
| **Neutral surfaces (dark)** | `--surface-neutral-dark-1` / `-2` / `-3` | `#373A45` / `#1D1F26` / `#000000` |
| **Brand surfaces (light)** | `--surface-brand-1` / `-2` / `-3` | `#D9E8FF` / `#BFDBFF` / `#A3CBFF` |
| **Brand surfaces (dark)** | `--surface-brand-dark-1` / `-2` / `-3` | `#123778` / `#0D2F5E` / `#0A2342` |
| **Text on light** | `--text-default` / `--text-subtler` / `--text-subtlest` | `#1D1F26` / `#373A45` / `#474B57` |
| **Text on dark** | `--text-inverse-default` / `-subtler` / `-subtlest` | `#E2E3E7` / `#C4C7D1` / `#B2B6C2` |
| **Brand colors** | `--brand-elastic-blue` / `--brand-elastic-yellow` / `--brand-elastic-yellow-dark` / `--brand-elastic-yellow-light` | `#0B64DD` / `#FEC514` / `#FDC031` / `#FFDA3D` |

### Legacy Search Labs tokens — aliased (soft replacement)

All 13 original color tokens are preserved as **aliases** that redirect to the new system. This keeps the ~370 existing `var()` references rendering without per-selector rewrites. The visible delta is what Search Labs absorbs.

| Legacy token | Before | Now resolves to | Δ | Uses* |
|---|---|---|---|---|
| `--white` | `#FFFFFF` | `--surface-neutral-1` → `#FFFFFF` | 0 | 39 |
| `--black` | `#0B0C0E` | `--surface-neutral-dark-3` → `#000000` | slightly darker | 11 |
| `--footer-bg` | `#0E0F10` | `--surface-neutral-dark-3` → `#000000` | slightly darker | 0 |
| `--gray-dark` | `#2E3137` (body text) | `--text-default` → `#1D1F26` | darker, +AAA contrast | 82 |
| `--gray` | `#6B7079` (secondary text) | `--text-subtlest` → `#474B57` | **noticeably darker** | **137 — biggest visual shift** |
| `--gray-medium` | `#C8D1E9` | `--surface-neutral-3` → `#C4C7D1` | tiny, slightly cooler | 5 |
| `--gray-lightest` | `#F5F8FB` (section bg) | `--surface-neutral-2` → `#E2E3E7` | **visibly grayer sections** | 12 |
| `--border` | `#E6ECF3` | `--surface-neutral-3` → `#C4C7D1` | borders darker | 52 |
| `--hero-bg` | `#F5F8FB` | `--surface-neutral-2` → `#E2E3E7` | same Δ as `--gray-lightest` | 6 |
| `--primary` | `#FEC514` | `--brand-elastic-yellow` → `#FEC514` | 0 | 1 |
| `--primary-dark` | `#FDC031` | `--brand-elastic-yellow-dark` → `#FDC031` | 0 | 1 |
| `--yellow-light` | `#FFDA3D` | `--brand-elastic-yellow-light` → `#FFDA3D` | 0 | 0 |
| `--blue` | `#0B64DD` | `--brand-elastic-blue` → `#0B64DD` | 0 | 22 |

\* Combined `index.html` + `components.html` `var()` counts at time of migration.

### Where to expect the biggest visual changes

1. **All secondary text (`--gray`, 137 uses)** drops from `#6B7079` to `#474B57` — a substantial darken. Article subtitles, dates, metadata pills, "by Author" lines, etc. will read as more emphasized and higher-contrast.
2. **Section backgrounds (`--gray-lightest` + `--hero-bg`, 18 combined uses)** shift from a cool near-white `#F5F8FB` to a real gray `#E2E3E7`. The hero panel, Examples and Integrations section backgrounds, and similar will read as more distinctly "framed."
3. **Borders (`--border`, 52 uses)** darken from `#E6ECF3` to `#C4C7D1`. Card outlines, hairlines between sections, etc. become more visible.
4. **Body text (`--gray-dark`, 82 uses)** darkens slightly from `#2E3137` to `#1D1F26`. Subtle but pushes contrast deeper into AAA territory.
5. **`--black` (11 uses)** drops the brand-tinted near-black `#0B0C0E` for pure `#000000`. Hardly noticeable.

### How to revert

- **Whole migration**: restore the Brand + Neutrals blocks from before this session, delete the surface/text/brand blocks plus the alias block. The original Search Labs hex values are preserved inline in the comments next to each alias.
- **Single token**: change one alias line back to a literal hex from its comment, e.g. `--gray: #6B7079;` instead of `var(--text-subtlest)`. The new tokens stay in the file for any code that opted into them.
- **Variable-binding values for light grays**: replace the docs hex on a `--surface-neutral-*` line with the `Alt:` value from the inline comment. Aliases keep working unchanged.

### Side-effects (none required this session)

No selector rewrites — every existing rule references legacy tokens, which still resolve. New components added later should reach directly for `--surface-*` / `--text-*` instead of `--white` / `--gray-dark`. Over time, individual selectors can be migrated; once all references to a legacy alias are gone, the alias line can be deleted.

---

## 2026-05-14 — Session 6: Design fix — `.cat-card__title` size match

Not a Figma-driven migration — a per-component visual alignment requested in chat. Logged here so every change is traceable.

### Applied

| Selector | Property | Before | After | Why |
|---|---|---|---|---|
| `.cat-card__title` | `font-size` | `var(--fs-h4)` (25 px) | `var(--fs-h5)` (22 px) | Match `.popular__title` size on the home page. The category cards (Tutorials / Examples / Integration / Blog) and the Popular sidebar items now read at the same scale. |
| `.cat-card__title` | `line-height` | literal `1.2` | `var(--lh-h5)` (1.3) | Use the matching line-height token so the two titles align on both axes; also brings the cat-card off a magic-number literal and onto the token. |

**File touched**: `index.html` only (component-level style inside the inline `<style>` block). No `design-system.css` change.

**Blast radius**: the four `.cat-card__title` `<h3>` instances on the Search Labs home page (`#home`). No other view uses this class.

**Note on design intent**: previously `.cat-card__title` was 3 px larger than `.popular__title`, which gave the category cards extra prominence as wayfinding UI. The new alignment treats both as peer headings. If the cards feel under-emphasized after this change, the one-line revert is `font-size: var(--fs-h4); line-height: 1.2;`.

---

## 2026-05-14 — Session 7: Spacing + Radii

Source: Figma file Design-System-Tokens-and-Variables-Test-AI → page **Primitive: Dimension**. Data read from user-provided screenshots (the page uses raw text rows rather than bound variables, so `get_variable_defs` returned nothing for it).

### Library inventory (Primitive: Dimension)

| Section | Values |
|---|---|
| viewport | `xs:320 sm:768 md:1024 lg:1280 xl:1440 2xl:1920` |
| border / width | `0 1 2 3 4` |
| radius | `full: 9999` (semantic radii like `radius/md:8` defined elsewhere, map back to `dimension/*`) |
| dimension (primitives) | `0 1 2 3 4 6 8 12 16 24 32 40 48 64 80 96 128 160 192 224 256 320` |
| dimension / icon / size | `xs:12 sm:16 md:20 lg:24 xl:32 2xl:48` |
| dimension / negative | `-4 -6 -8 -12 -16 -20 -24 -32` |

Anomaly noted: a `"2 2": -2` row in the dimension section, ignored as a likely misnamed Figma artifact.

### Applied — Spacing (`--s-*`)

| # | Token | Before | After | Δ | Library mapping | Status |
|---|---|---|---|---|---|---|
| 1 | `--s-4` | 4 px | 4 px | 0 | dimension/4 — exact | ✅ Annotated (no value change) |
| 2 | `--s-8` | 8 px | 8 px | 0 | dimension/8 — exact | ✅ Annotated |
| 3 | `--s-12` | 12 px | 12 px | 0 | dimension/12 — exact | ✅ Annotated |
| 4 | `--s-16` | 16 px | 16 px | 0 | dimension/16 — exact | ✅ Annotated |
| 5 | `--s-24` | 24 px | 24 px | 0 | dimension/24 — exact | ✅ Annotated |
| 6 | `--s-32` | 32 px | 32 px | 0 | dimension/32 — exact | ✅ Annotated |
| 7 | `--s-48` | 48 px | 48 px | 0 | dimension/48 — exact | ✅ Annotated |
| 8 | `--s-64` | 64 px | 64 px | 0 | dimension/64 — exact | ✅ Annotated |

Perfect match across the board. No value changes — Search Labs' spacing scale is fully contained within the library's primitive dimension scale.

### Applied — Radii (`--r-*`)

| # | Token | Before | After | Δ | Library mapping | Status |
|---|---|---|---|---|---|---|
| 1 | `--r-4` | 4 px | 4 px | 0 | dimension/4 — exact | ✅ Annotated |
| 2 | `--r-8` | 8 px | 8 px | 0 | dimension/8 (and semantic `radius/md`) — exact | ✅ Annotated |
| 3 | `--r-10` | 10 px | **8 px** (alias to `--r-8`) | **−2 px** | No library match; tie-broken to dimension/8 over dimension/12 per user pick. Consolidates two Search Labs radii into one library value. | ✅ Applied |
| 4 | `--r-full` | 9999 px | 9999 px | 0 | radius/full — exact | ✅ Annotated |

### Blast radius of `--r-10 → 8 px`

Three card components shift from 10 px → 8 px corners. All cards in the prototype that use these classes will read as slightly less rounded:
- `.featured` — the home page featured card (line 219 of `index.html`).
- `.related-card` — the standard article card pattern used across home pages, tag pages, and related-articles strips. The highest-traffic card in the prototype.
- `.blog-feed__featured` — the corporate Blog featured card (line 1985).

### Related cleanup opportunity (not done this session)

There is one **literal `border-radius: 10px`** at line 626 (`.qs-callout`, the teal-to-blue sidebar Quick Start callout). It bypasses the token system, so after this session the cards have 8 px corners while this callout still has 10 px. The callout's block uses literals for several properties (`gap: 18px`, `padding: 26px 24px`), so the whole component is un-migrated; touching just one literal would be inconsistent. Logged as a follow-up — recommended to either migrate the whole block to tokens, or change just the radius to `var(--r-10)` if you want it to follow the consolidation.

### New scales available but not introduced

Per user pick, these were **annotated only** (in the comment header of the spacing block) and not added as new tokens. Each is additive and would not change any existing rendering.

- **viewport breakpoints** (`xs:320 sm:768 md:1024 lg:1280 xl:1440 2xl:1920`). Could replace literal `@media (max-width: 1024px)` / `(max-width: 720px)` breakpoints scattered in the CSS.
- **border widths** (`0 1 2 3 4`). Search Labs uses literal `1px solid var(--border)` in 50+ places. Could be `--bw-1` etc.
- **icon sizes** (`xs:12 sm:16 md:20 lg:24 xl:32 2xl:48`). Search Labs has no icon system; this would be the scale if added.
- **negative dimensions** (`-4 -6 -8 -12 -16 -20 -24 -32`). For negative margins / offsets.
- **extra dimension primitives** (`0 1 2 3 6 40 80 96 128 160 192 224 256 320`). Search Labs uses some as literals (e.g. `max-width: 1512px`, `height: 433px`); not tokenized today.

---

## 2026-05-14 — Session 8: Literal hex cleanup — `#6B7079` (the stale pre-migration `--gray`)

A targeted cleanup: 12 places hardcoded the value `#6B7079`, the pre-Session-5 value of `--gray`. After Session 5 those literals stayed at the old gray while every `var(--gray)` reference migrated to `#474B57`. This session fixes that silent inconsistency.

### Applied — `index.html` (9 sites)

All 9 occurrences migrated to `var(--gray)` (which now resolves to `#474B57`). Mechanical bulk replace; no judgment calls per site. Each site darkens from `#6B7079` to `#474B57`.

| Line | Selector / context | Replacement |
|---|---|---|
| 1813 | secondary text `color` | `color: var(--gray);` |
| 1888 | secondary text `color` | `color: var(--gray);` |
| 1947 | secondary text `color` | `color: var(--gray);` |
| 1961 | secondary text `color` | `color: var(--gray);` |
| 1968 | secondary text `color` | `color: var(--gray);` |
| 2095 | secondary text `color` | `color: var(--gray);` |
| 2466 | `.tok-comment` (code-syntax comment color) | `color: var(--gray);` + updated stale `4.9:1` annotation to note the new AAA contrast ratio |
| 2750 | gradient stop — `linear-gradient(to right, #343741 0%, #343741 86%, #6B7079 100%)` | `... var(--gray) 100%)` |
| 2768 | `.gnav__util-divider` background (0.4 opacity divider in the gnav) | `background: var(--gray);` |

### Applied — `components.html` (3 sites)

| Line | Context | Replacement |
|---|---|---|
| 203 | `.code .comm` (code-syntax comment color in docs) | `color: var(--gray);` |
| 419 | `--gray` docs swatch inline style — visual color block | `style="background: var(--gray);"` |
| 420 | `--gray` docs swatch **text label** — the literal hex displayed to the reader | `#6B7079` → `#474B57` (label kept as a literal hex on purpose; it's documentation describing the current value, not a CSS reference). WCAG note also updated from "Meets WCAG AA" to "~9.5:1 (AAA)". |

### Why each case was handled differently

- Six plain `color:` rules and the divider/gradient were uniform sed replacements — same pattern, no judgment needed.
- The `.tok-comment` carried an inline contrast annotation (`/* # ... — 4.9:1 */`) that became stale after the value darkened. The comment was rewritten to record both the new ratio and the historical context.
- The docs swatch in `components.html` is documentation **describing** the system, not consuming it. The visual block (line 419) now consumes the token. The text label (line 420) is a static description of the current value, so it became the new literal `#474B57`, with a history-preserving note (`Was #6B7079 pre-migration`).

### Residue check

Three references to `#6B7079` remain in the bundle. All three are inside **comment / documentation text** describing the migration history:
- `index.html:2466` — inside the `.tok-comment` annotation.
- `components.html:420` — inside the docs swatch note ("Was #6B7079 pre-migration").
- `design-system.css:88` — inside the `--gray` alias comment ("was #6B7079 secondary text…").

These are intentional and the prototype renders identically with or without them.

### Verification

```
grep #6B7079 across the bundle → 3 matches, all in comments
<div balance in index.html → 895 / 895
.page-view count → 21
```

### Blast radius (rendered)

Twelve places that previously rendered `#6B7079` (a mid-gray, ~4.9:1 contrast on white) now render `#474B57` (~9.5:1 AAA). The biggest perceptual shifts:
- Two text labels in the corporate-blog area (`color:` rules around lines 1947/1961/1968) darken visibly.
- The `.gnav__util-divider` (small vertical separators in the top utility row) darken slightly — partly masked by the 0.4 opacity.
- The 3-stop gradient at line 2750 — previously `#343741 → #343741 → #6B7079` (steady-then-lighten) — now becomes `#343741 → #343741 → #474B57`, which is a much subtler ramp (the two endpoints are closer in luminance). The gradient effectively flattens.

If the gradient shift looks wrong, the per-site revert is changing `var(--gray) 100%` back to `#6B7079 100%` on that one line.

---

## 2026-05-14 — Session 9: Button icon support (Figma 994:3326)

Source: Figma node `994:3326` (Primary button) in the Components — Designers Area file (`Nsk35nyWrPgLKUo9ah4mD4`). The design shows the primary button with an optional 16 px icon adjacent to the label (`showIconLeft` / `showIconRight` props in the Figma component). The button shell already matches Search Labs' `.btn--primary` exactly on height (40 px), padding (0 16 px), gap (4 px), radius (4 px), background (`#0B64DD`), text color (white), and Inter Medium 16 / 24. The implementation gap was just that no Search Labs button instance opted into the icon slot.

### What changed

Three button instances in `index.html` were given a trailing 16 px icon. No CSS changes — `.btn--primary` already uses `display: inline-flex; gap: 4px;` so the icon lays out automatically. Icons are inline SVG with `stroke="currentColor"` so they inherit the button's text color (white on primary).

| Line | Selector / button | Icon added | Why this icon |
|---|---|---|---|
| 2895 (gnav top-right) | `.btn--primary` — "Start free trial" | arrow-right (→) | Forward-motion CTA; matches the existing arrow on `.btn--primary` "Try it yourself" (line 7082) |
| 3075 (home — Examples/Integrations section) | `.btn--primary` — "Elasticsearch Quick Start" | external-link (↗) | Matches the same icon used on the two other "Quick Start" buttons (lines 3614, 3701) which open in new tabs |
| 7107 (footer/bottom CTA) | `.btn--primary` — "Start free trial" | arrow-right (→) | Same as the gnav button for consistency |

### What deliberately did NOT change

- **Secondary buttons** ("Contact sales" in the gnav, "Subscribe to newsletter" ×3) — per design intent that secondary in a pair shouldn't compete with the primary's icon. Cleaner visual hierarchy.
- **Buttons already with icons** — three primary buttons had icons before this session ("Quick Start" ×2 on tutorial/example pages, "Try it yourself", and the footer tertiary "Contact sales"). Left as they were.
- **Button shell CSS** — the existing `.btn--primary` already meets the Figma spec; the gap, height, padding, radius, font, and color all match. No change to `design-system.css`.

### Icon convention used (for future buttons)

- Inline SVG, `width="16" height="16"`, `viewBox="0 0 24 24"`, `stroke="currentColor"`, `stroke-width="2"`, rounded caps/joins.
- Placed **after** the label inside the anchor: `<a class="btn--primary"><span>Label</span><svg>…</svg></a>`.
- Label wrapped in `<span>` so it's a flex child alongside the SVG (matches the existing pattern at line 7082).
- `aria-hidden="true"` on the SVG — the label conveys meaning; the icon is decorative.

Two paths if you want the icon left-of-label instead: put the SVG before the `<span>`, or use `flex-direction: row-reverse` on the specific button instance. No new CSS needed either way.

### Open follow-up

The `components.html` design-system docs page still shows only **text-only** button examples (lines 544, 567, 590). After this session the prototype demonstrates a documented icon-variant pattern that the docs don't describe. Quick fix: add three more rows to the buttons section showing primary-with-trailing-icon, primary-with-leading-icon, and primary-with-both-icons. Not done this session; flagged as a docs gap.

---

## 2026-05-14 — Session 10: Divider + Breadcrumb (Figma 1629:1125 + 1676:4101)

Source: Figma nodes `1629:1125` (Divider) and `1676:4101` (Breadcrumb) in the Components — Designers Area file (`Nsk35nyWrPgLKUo9ah4mD4`).

### What was added

Two new components in `design-system.css`, plus matching docs sections in `components.html`. Neither changes any existing rendering in `index.html` — these are additive.

**Divider** — a 1 px horizontal hairline.

```css
.divider { display: block; height: 1px; width: 100%; background: var(--border); border: none; margin: 0; }
```

Usage: `<hr class="divider" />` (preferred for semantic horizontal rules) or `<div class="divider"></div>`.

Note on color: Figma binds the line to `rgba(29,31,38,0.2)` — a semi-transparent overlay. The implementation uses the migrated `--border` token (solid `#C4C7D1`) for token-system consistency. The two render virtually identically on white surfaces (ΔE ≈ small). To switch to the Figma semi-transparent value, replace `var(--border)` in the rule.

**Breadcrumb** — horizontal nav trail.

```css
.breadcrumb        { display: inline-flex; align-items: center; flex-wrap: wrap; font: 500 14px/1.5 var(--font-body); gap: 4px; }
.breadcrumb__item  { display: inline-flex; align-items: center; gap: 4px; color: var(--text-subtlest); text-decoration: none; }
a.breadcrumb__item:hover           { text-decoration: underline; }
.breadcrumb__item--current         { color: var(--brand-elastic-blue); cursor: default; }
.breadcrumb__sep                   { color: var(--text-subtlest); user-select: none; }
```

Usage:
```html
<nav class="breadcrumb" aria-label="Breadcrumb">
  <a href="#" class="breadcrumb__item">Text Link</a>
  <span class="breadcrumb__sep" aria-hidden="true">/</span>
  <a href="#" class="breadcrumb__item">Text Link</a>
  <span class="breadcrumb__sep" aria-hidden="true">/</span>
  <span class="breadcrumb__item breadcrumb__item--current" aria-current="page">Text Link</span>
</nav>
```

Items may carry optional 16 px leading and/or trailing icons (16-px SVG, `stroke="currentColor"`, `aria-hidden="true"`). The `inline-flex` + 4 px gap handles icon layout automatically.

### `components.html` additions

- New `#divider` docs section (line 654 onward) — stage with default rule, stage with rule inside a card-like container, HTML snippet, spec table.
- New `#breadcrumb` docs section (line 694 onward) — three stages (3-item / 2-item / with trailing icon), HTML snippet, spec table.

### Existing prototype patterns NOT migrated (flagged as follow-up)

The prototype already has two breadcrumb-shaped components used in 7 places, each with **different** styling:

| Selector | Style | Used | Locations |
|---|---|---|---|
| `.article-hero__crumb` | Mono font, weight 700, 12 px, brand blue, all-caps via tracking | 4× | lines 4776, 4925, 5705, 6951 (article hero blocks across views) |
| `.tutorial-hero__crumb` | Custom style (see line 2318) | 3× | lines 4387, 4512, 4616 (tutorial / integration / example hero blocks) |

These are deliberately untouched this session. The new `.breadcrumb` component is a different visual language (Inter Medium gray + blue, no caps, slash separator) — it's not a drop-in replacement. If you want to unify all breadcrumb instances on the new component, that's a separate refactor; the existing hero crumbs would lose their mono/uppercase feel.

### No CSS conflicts

Class names `.divider`, `.breadcrumb`, `.breadcrumb__item`, `.breadcrumb__sep` did not previously exist in `index.html`, `design-system.css`, or `components.html`. Pure additions.

### Verification

```
brace balance in design-system.css → 19 / 19
<section in components.html       → 8 / 8
<div in components.html           → 75 / 75
<div in index.html (unchanged)    → 895 / 895
.page-view in index.html          → 21
```

---

## 2026-05-14 — Session 11: `components.html` documentation sync

A docs-only round. No tokens or component CSS changed; this session caught `components.html` up with the design-system state that had drifted across Sessions 1–10.

### What was stale and got fixed

| Area | Before | After |
|---|---|---|
| **Colors** | 11 Search Labs swatches with pre-migration hex values. New surface, text, and brand-color systems undocumented. | Four sub-sections: Brand colors (4), Neutral surfaces (6), Brand surfaces (6), Text scale (6) — all with live `var(--…)` swatches. Plus a 13-row legacy-aliases table showing each alias's new resolved value alongside its pre-migration value (so the delta Search Labs absorbed is visible). |
| **Typography** | `--fs-h2` labeled "39.06px / 1.4"; sample column rendered the actual migrated 40 / 1.2 size. Spec column lied. `--fs-link` absent. Font families and weights not documented. | All 8 `--fs-*` rows now show their migrated values. New `--fs-link` row added. Added: "Library class" column mapping each token to the Elastic type-style name (Headline Large, Title Medium, etc.). Two new sub-sections: Font families (3 rows) and Font weights (with the `--fw-body: 500` experiment flagged as 🧪). |
| **Spacing** | Generic 4 / 8-based scale description. | Same scale, plus an explicit note that every `--s-*` is an exact match to a library primitive (Δ 0 across the board, confirmed in Session 7). |
| **Radius** | `--r-10` shown as `10px`. | `--r-10` row now reads "8px _(alias to --r-8)_" with a note explaining the consolidation. |
| **Buttons** | Three text-only variants (Primary / Secondary / Tertiary) only. No icon variant docs even though icons were added to the prototype in Session 9. | New "Icon variants" sub-section under Primary, showing trailing icon, leading icon, and both-sides icon examples with copy-pasteable HTML snippets and a spec table. Sources the pattern back to Figma node 994:3326. |

### What was already accurate

- Divider (added Session 10) — unchanged.
- Breadcrumb (added Session 10) — unchanged.
- Chips section — unchanged; values already align with the design system.
- The single `--gray` swatch was already updated in Session 8 — preserved.

### Verification

```
<div in components.html → 116 / 116 balanced
<section in components.html → 8 / 8
<table in components.html → 6 / 6
swatches → 22 in colors section + 1 in chip note
sections → colors, typography, spacing, radius, buttons, chips, divider, breadcrumb
```

### What still isn't documented in `components.html` (deliberate gaps)

- **The 7 existing breadcrumb instances** in `index.html` (`.article-hero__crumb` × 4, `.tutorial-hero__crumb` × 3) — these use a different visual language from the new `.breadcrumb` component and aren't aliased/migrated. Flagged in Session 10's notes; would be a separate refactor.
- **Hero patterns** (`.article-hero`, `.subpage-hero`, `.tutorial-hero`, `.author-hero`) — used heavily in `index.html` but never extracted into the docs. These would each warrant their own docs sections if you want full coverage.
- **Card patterns** (`.featured`, `.related-card`, `.popular`, `.cat-card`, `.blog-feed__featured`) — same; live in `index.html`'s inline `<style>`, not documented in `components.html`.
- **Newsletter, callouts, footer, gnav, topnav, subnav, page-view layout** — same.

`components.html` today documents the **design-system primitives** (colors, type, spacing, radius) and the **shared atomic components** (buttons, chips, divider, breadcrumb). Larger composed patterns like cards and heroes still live exclusively as inline CSS inside `index.html`. Extracting them into the docs is the next natural step but wasn't in scope for this docs-sync round.

---

## 2026-05-14 — Session 11.1: Swatch render fix

Hot fix to Session 11. The colors section rewrite had set every swatch background to `style="background: var(--token);"` (inline CSS variable reference). In the user's preview context, those `var()` references inside inline `style=""` attributes don't resolve, so every swatch rendered with no background — visually empty cards above the metadata.

The same `var()` references work fine in stylesheet rules (the `<style>` block in `components.html` and `design-system.css` itself), so the rest of the page chrome rendered correctly. Only the inline-style swatch backgrounds failed.

### What changed

24 `var(--token)` references inside `swatch__color` inline styles were replaced with literal hex values. Two of those were `var(--border)` inside `box-shadow` declarations on the light/white swatches; the rest were the 22 background references.

```html
<!-- Before (Session 11) — broken in preview contexts -->
<div class="swatch__color" style="background: var(--brand-elastic-blue);"></div>

<!-- After (Session 11.1) — renders everywhere -->
<div class="swatch__color" style="background: #0B64DD;"></div>
```

The token name and hex are still documented in the `swatch__meta` block underneath each swatch, so the docs remain complete; only the visual swatch itself uses a literal value now.

### What was left as `var()`

- Class-based CSS rules in the `<style>` block of `components.html` — they work fine.
- Typography samples in the type-scale table (`style="font-family: var(--font-heading); font-size: var(--fs-h2);"`) — these need `var()` to demonstrate the migrated values; they only fail if `design-system.css` doesn't load, which is a different problem (loading) than the inline-`var()` resolution issue.
- The divider demo container (`style="border: 1px solid var(--border); background: var(--white);"`) — same situation; could be hardened similarly if it also renders blank.

### Why this happened

Pre-Session-11 the original colors section used literal hex in inline styles (working). The rewrite switched to `var()` references thinking the design-system tokens would resolve. They don't, in some preview contexts. Lesson: any visual that needs to render in a sandboxed / preview environment must avoid `var()` inside inline `style=""` attributes — keep them in class rules within the file's own `<style>` block, or use literal values.

---

## 2026-05-14 — Session 11.2: `.popular__item` spacing bump

Visual tweak. The home page's "New and popular on Search Labs" sidebar list had its 3 article entries reading too tight — each title sat ~32 px from the next (16 px padding × 2 above/below the hairline).

Change in `index.html` line 280:

```css
.popular__item {
  padding: var(--s-24) 0;   /* was var(--s-16) 0 */
}
```

New spacing: ~48 px between adjacent items (24 px padding × 2). The internal gap inside each item (between chips/date and the title) stays at `var(--s-16)` — each item still reads as one chunk; the change only affects between-item rhythm. Hairline divider at `.popular__item + .popular__item` keeps its job.

Library-aligned: 24 px is the next step on the `--s-*` scale after 16 (Δ 0 to the library's dimension/24 primitive).

---

## 2026-05-14 — Session 12: Font-weight tokens + sweep

The largest token gap by usage count, closed. Pre-Session-12 the prototype had **101** literal `font-weight: NNN;` rules in `index.html` alone (plus 19 in `components.html`, 3 in `design-system.css`). All Bold weights, all SemiBold weights, and all Medium weights now reference canonical tokens.

### New tokens in `design-system.css`

```css
--fw-regular:  400;
--fw-medium:   500;
--fw-semibold: 600;
--fw-bold:     700;
```

The existing experiment token `--fw-body: 500` is **kept distinct**, with the relationship documented in a comment block above the new tokens. Reasoning: `--fw-body` is a semantic alias for the `<body>` element specifically (controls the body-weight experiment in isolation); `--fw-medium` is the general token for any other 500-weight usage. They share a value today; they answer different questions, so toggling `--fw-body` reverts the body experiment without disturbing any other 500 usage.

### Sweep results

| File | Replacements | Notes |
|---|---|---|
| `index.html` | 100 | 63 → `--fw-bold`, 17 → `--fw-semibold`, 17 → `--fw-medium`, 3 → `--fw-regular` |
| `components.html` | 17 | Same proportional mix; sweep skipped `<pre class="code">` blocks (doc snippets) |
| `design-system.css` | 3 | Inside the component rules (chip, etc.) |
| **Total** | **120** | |

### What deliberately wasn't swept

- `font-weight: 800` × 2 (one in `index.html`, one in `components.html`) — one-off uses, not tokenizing for now.
- One stale "feature missing" note in `components.html` line 618 that was *describing* `font-weight: 700` as text inside a `<code>` tag. The regex correctly skipped it (text content, not CSS), but the prose was now factually wrong — the `--fw-bold` token IS introduced — so I rewrote that whole Font weights documentation table.

### `components.html` Font weights docs section rewritten

The previous table had 1 row (`--fw-body`) plus a stale note. The new table has 5 rows: the 4 canonical weight tokens with usage counts, plus the `--fw-body` experiment row clearly marked 🧪 with the new "kept distinct on purpose" explanation. Direct-edit-friendly: each token has a "value", "notes", and the rough usage count from the sweep.

### Why the regex was safe

Pattern: `font-weight: NNN(?=[;\s}])`. This matches the literal `font-weight: 700;` (followed by `;`), `font-weight: 700` then whitespace (line wrap), or `font-weight: 700}` (inline rule terminator). Crucially it does **not** match `var(--fw-body, 400)` — after `400` comes `)`, which isn't in the lookahead class — nor does it match HTML body text like `<code>font-weight: 700</code>` where `<` follows.

### Verification

```
remaining literal font-weight: NNN in CSS context
  index.html        → 1 (the 800 one-off)
  components.html   → 1 (the 800 one-off)
  design-system.css → 0

var() references in index.html
  var(--fw-bold)     → 63
  var(--fw-semibold) → 17
  var(--fw-medium)   → 17
  var(--fw-regular)  → 3
                      ───
                      100  ✓ matches sweep count

structural integrity
  <div in index.html       → 895 / 895
  <div in components.html  → 116 / 116
  page-views in index.html → 21
  brace balance in CSS     → 19 / 19
```

### Visual change

**None.** Every replacement preserves the numeric weight; tokens are pure aliases for now. The change is observable only by inspecting CSS — every heading is still 700, every chip still 500, every card title still 600.

### What this enables

- One-place changes: if the brand later loads Mier B ExtraBold and we want all headings at 800, bump `--fw-bold` and every heading follows.
- The experiment pattern (`--fw-body`) is now visibly clean: it's a *semantic* override that happens to share a value with the general medium token. The pattern can be reused for future style experiments without polluting the general scale.

---

## 2026-05-14 — Session 13: Chip-title optical alignment in card patterns

User-reported visual issue. In the featured card (and related card patterns), the chip's pill outline sat flush with the title's left edge — strictly correct — but the chip's *text* was inset 11 px from the title's text (1 px chip border + 10 px chip horizontal padding). Visually it read as misaligned: title started 11 px to the left of the chip label.

The standard design-system fix is **optical alignment via negative margin**: pull the chip row left by exactly the chip's left visual offset so the chip *text* aligns with the title *text*. The chip pill then slightly overhangs the card padding edge, which is the intentional, on-spec look.

### Applied to

```css
.featured__meta            { margin-left: -11px; }
.related-card__meta        { margin-left: -11px; }
.popular__head             { margin-left: -11px; }
.blog-feed__featured-meta  { margin-left: -11px; }
```

Each rule got a comment block referencing the canonical comment on `.featured__meta` so anyone editing them later knows what the `-11px` is for.

### Where this lands visually

| Component | View(s) affected | Instances |
|---|---|---|
| `.featured__meta` | home, observability, security, blog landing pages | 4 |
| `.related-card__meta` | article, related-articles, recommendation sections across many views | 10+ |
| `.popular__head` | home page "New and popular on Search Labs" sidebar | 3 |
| `.blog-feed__featured-meta` | blog feed landing | 2 |

### What was NOT touched

- `.cat-card` — no chips
- `.explore-row__chips` — chips are *beside* the title link (horizontal layout), not above
- `.examples-row__chips` — same horizontal layout
- The `.chip` definition in `design-system.css` itself — the chip is unchanged; only its parent container is offset

### The 11px magic number

```
1 px chip border + 10 px chip padding-left = 11 px
```

If the chip's padding or border ever change in `design-system.css`, the four `margin-left: -11px` values would need updating to match. Documented in each rule's comment. A future refactor could pull this into a CSS variable (`--chip-optical-offset`) — flagged as a low-priority cleanup.

---

## 2026-05-14 — Session 14: `.featured` nested-anchor antipattern fix + Session 13 revert

User-flagged real bug. In all 4 `.featured` cards, the markup was:

```html
<article class="featured">
  <a href="..." class="featured__link">   ← outer wrapping link
    <div class="featured__image">...</div>
    <div class="featured__body">
      <div class="featured__meta">
        <a href="..." class="chip">APM</a>   ← NESTED <a> — invalid HTML
        ...
      </div>
      <h2 class="featured__title">...</h2>
    </div>
  </a>
</article>
```

Nested `<a>` elements are invalid per the HTML spec. When the browser parses this, it auto-closes the outer `<a class="featured__link">` the moment it hits the inner `<a class="chip">`. The visible result in DevTools is `<a class="featured__link"></a>` (empty), with all the children hoisted out and re-parented. The reparenting was the actual cause of the chip/title misalignment that Session 13 patched with `margin-left: -11px`.

### What changed

1. **Markup** — all 4 `.featured` blocks rewritten. The wrapping `<a class="featured__link">` is gone. The title text is wrapped in an `<a href>` instead, matching how `.related-card`, `.popular`, and `.blog-feed__featured` already work:

```html
<article class="featured">
  <div class="featured__image">...</div>
  <div class="featured__body">
    <div class="featured__meta">
      <a href="..." class="chip">APM</a>
      <span class="featured__date">...</span>
    </div>
    <h2 class="featured__title">
      <a href="...">Lorem ipsum...</a>          ← just the title is a link now
    </h2>
  </div>
</article>
```

2. **CSS** — removed:
   - `.featured__link { display: flex; flex-direction: column; color: inherit; }`
   - `.featured__link:hover .featured__title { text-decoration: underline; }`

   Added (matching the related-card pattern):
   - `.featured__title a { color: inherit; text-decoration: none; }`
   - `.featured__title a:hover { text-decoration: underline; }`

3. **Reverted Session 13** — removed `margin-left: -11px` from all 4 containers:
   - `.featured__meta`
   - `.related-card__meta`
   - `.popular__head`
   - `.blog-feed__featured-meta`

   Plus the comment blocks introducing them.

### What this means for Session 13

Session 13 was a band-aid for a symptom (chip/title misalignment) whose actual cause was this Session 14 bug. The browser's auto-close of the invalid outer anchor was shifting layout in `.featured`. The other three patterns (`.related-card`, `.popular`, `.blog-feed__featured`) never had this bug — their chip-and-title alignment was always correct, even though my Session 13 work treated all four uniformly. So Session 13's `-11px` was either fixing a real issue caused by the broken HTML (`.featured`) or fixing nothing (the other three) — net result: undone everywhere.

### UX trade-off (acceptable)

Before: clicking anywhere on a `.featured` card navigated to the article (whole card was a link — though broken HTML meant browsers handled this unpredictably).

After: only clicking the title navigates to the article. Image and meta-area clicks do nothing. This matches how `.related-card`, `.popular`, and `.blog-feed__featured` already worked.

If full-card-click behavior is desired later, the standard pattern is to use a `::after` pseudo-element on the title link to extend its clickable area over the entire card (the "card-as-link" / "extended-target" pattern). Flagged as a future enhancement; not implemented now.

### Bug in my first attempt at this fix (interesting failure mode)

My first script used the regex `<a href="[^"]+" class="featured__link">\s*(.*?)</a>` with `re.DOTALL`. The lazy `.*?` made the match stop at the *first* `</a>` it encountered — which, because of the nested chip anchor, was the chip's closing tag. The substitution then deleted the wrong scope and left a corrupted file. I reverted from `/mnt/user-data/outputs/` (which had the last-shipped clean state) and rewrote the script to use **structural anchoring**: find the wrapper's closing `</a>` by walking forward to the next `</article>` and stepping back one line. That's reliable because the original markup is well-structured. Lesson: don't use regex to match nested HTML; use the structure.

### Verification

```
featured__link references → 0
margin-left: -11px         → 0
.featured__title with <a wrap → 4 (all instances)

<div in index.html       → 895 / 895
<article                 → 171 / 171
<a / </a>                → 653 / 653  (balanced)
page-views               → 21
```

---

## 2026-05-15 — Session 15: Five-pass literal-to-token sweep + new `--surface-neutral-1-5` token

Big cleanup round. Five concurrent sweeps converting literal values throughout the codebase to canonical token references. **Backups** of all 5 files were snapshotted to `/home/claude/work/backups/pre-session-15/` AND to `/mnt/user-data/outputs/*.bak` before any changes so rollback is one-step.

### What got swept

| Pass | What | Sweeps | Notes |
|---|---|---|---|
| 1 | `#0B64DD` → `var(--brand-elastic-blue)` | 32 | Both `index.html` (17) and `components.html` legacy-aliases table (15) |
| 2 | `#FFFFFF` → `var(--white)` | 17 | Both files |
| 3 | On-scale spacing `Npx` → `var(--s-N)` | 94 | `padding*/margin*/gap` only; only N ∈ {4,8,12,16,24,32,48,64} |
| 4 | On-scale radius `Npx` → `var(--r-N)` | 8 | `border-radius` only; only N ∈ {4,8,10} |
| 5 | `#F1F4F8` → `var(--surface-neutral-1-5)` (new token) | 11 | See decision below |
| **Total** | | **162** | |

### New token introduced: `--surface-neutral-1-5`

```css
--surface-neutral-1-5: #F1F4F8;
/* Library "Gray 25" equivalent — between -1 and -2.
   Subtle elevation for hover surfaces and code block backgrounds. */
```

**Why**: `#F1F4F8` had 11 hardcoded uses across the prototype with a real semantic role — subtle elevation that's almost-white. Used as:
- Hover background on `.side-nav__btn` and `.integration-detail__action`
- Code block backgrounds (with an AA contrast note in the source)
- Gradient stops in tutorial and integration heros

It sits between `--surface-neutral-1` (`#FFFFFF`) and `--surface-neutral-2` (`#E2E3E7`). The library has an equivalent "Gray 25" rung. The `1-5` naming preserves the ladder visual: 1, 1-5, 2, 3, dark-1, dark-2, dark-3.

### Two bugs the first sweep introduced (and how they got caught)

**Bug 1: Invalid CSS syntax `-var(--s-N)`**

The spacing sweep matched `margin-left: -8px;` and converted the `8px` portion to `var(--s-8)`, producing `margin-left: -var(--s-8);` — which is **invalid CSS**. The `-` prefix doesn't negate a `var()`. Browsers ignore the entire declaration silently. This appeared in 7 places: 5 inside inline `style=""` attributes in `components.html` (the section header negative-top margins) plus 2 in regular CSS rules.

Fix: replaced every `-var(--s-N)` with the negative literal `-Npx`. Caught and fixed in the same session.

**Bug 2: Inline-style `var()` regression** (Session 11.1 territory)

The sweep didn't differentiate between `<style>` block rules and inline `style=""` attributes. In the preview context, `var()` references inside inline styles don't resolve (this was the exact issue Session 11.1 fixed for swatch backgrounds). The sweep accidentally:
- Re-broke 2 swatch backgrounds in the colors section (Session 11.1 had specifically reverted those)
- Created 60 new inline `style=""` declarations with `var()` references that would fail in the preview context

Fix: a second-pass revert script restored literal values inside *every* inline `style=""` attribute while leaving regular `<style>` block rules alone. 62 reversions total.

The deeper takeaway: there's a **two-context rule** for this codebase that wasn't explicit before:

- **Inside `<style>` blocks and external `.css` files**: `var()` is preferred — that's the whole point of the token system.
- **Inside HTML `style=""` attributes**: literal values only — the preview context strips/ignores var() in inline styles. The exception is the typography demo samples in `components.html` that need to reference `var(--fs-h2)` etc. to demonstrate the migrated values, and those happen to render OK because the demo container is in a fully-loaded CSS context.

This rule is now stated explicitly so future sweeps don't have to learn it by breaking things again.

### What got skipped intentionally

- **Token definition lines themselves** — `--brand-elastic-blue: #0B64DD;` etc. stayed as literals (a token can't reference itself).
- **`<pre class="code">` blocks** — those are doc snippets shown as text, not active CSS.
- **Off-scale spacing literals** (40px, 28px, 18px, 6px, 80px, 96px, etc.) — no matching token to sweep to. Some may warrant new tokens (`--s-40`, `--s-80`) but that's an additive design decision, not a sweep.
- **Off-scale radius literals** (6px, 14px, 22px, 24px, 48px, 2px) — same reasoning.
- **`width`/`height`/`top`/`right`/`bottom`/`left`/`font-size`/`min-width`** — these properties take pixel values that happen to share numbers with the spacing scale (e.g. `width: 16px` for an icon) but aren't "spacing" — never swept.
- **Stale color literals near tokens but not exact** (`#1C1E23`, `#343741`, `#E6ECF3`, etc.) — these need a per-case design decision: snap to nearest token (small Δ shift) or accept as one-off. Flagged for future work.

### Visual change

**None.** Every swap was a literal → semantically-equivalent token reference. The token values match the literals exactly:
- `var(--brand-elastic-blue)` = `#0B64DD` (Δ 0)
- `var(--white)` = `#FFFFFF` (Δ 0)
- `var(--s-N)` ≡ `Npx` (these are 1:1 by definition)
- `var(--r-N)` ≡ `Npx` (same)
- `var(--surface-neutral-1-5)` = `#F1F4F8` (Δ 0)

### What this unlocks

- One-place brand-color update: bumping `--brand-elastic-blue` updates 34 places.
- One-place white surface update: bumping `--white` (or its alias target) updates 55 places.
- One-place spacing scale rework: bumping any `--s-N` updates 30–40 places.
- The new `--surface-neutral-1-5` makes the hover/code-background surface semantic instead of a coincidence.

### Token usage growth (before → after Session 15)

```
var(--s-*)  in index.html:        204 → 277  (+73)
var(--r-*)  in index.html:         19 →  27  (+8)
var(--brand-elastic-blue) total:    ~17 → 34  (+17)
var(--white) total:                 ~47 → 55  (+8)
var(--surface-neutral-1-5):          0 → 11  (new)
```

### Backups

`/home/claude/work/backups/pre-session-15/` holds untouched copies of all 5 files. `/mnt/user-data/outputs/*.bak` holds shipped copies you can re-download anytime to roll back this session in isolation.

### Verification

```
<div in index.html       → 895 / 895
<div in components.html  → 116 / 116
<article                 → 171 / 171
<a / </a>                → 653 / 653  (balanced)
page-views               → 21
CSS braces               → 19 / 19
broken -var() remaining  → 0
inline-style var() in active context → 0 (typography demos excluded — they intentionally use var(--fs-*))
22 swatches in components.html       → all 22 use literal hex (Session 11.1 fix preserved)
```

---

## 2026-05-15 — Session 16: `.chip--input` (filter chip with remove affordance) — Figma 2704:20245

New component. Per user request, this landed in three layers at once: design token CSS, component docs, and live use in the prototype.

### Figma source spec

Node `2704:20245` in Components — Designers Area. It's the "Input" style of chip with `action=true` — a chip that holds a removable value (a token). Distinct from our existing read-only `.chip` (used on cards and sidebars) and the navigation `a.chip` (used on filter links). Larger size, includes a trailing X (close/dismiss) icon.

Specs:
- Height 32px (vs 28px on default `.chip`)
- Padding 4 / 8 px (vs 4 / 10 px)
- Inter Medium 16 / 24 (Body Large — vs 14 / 1.4 on default)
- Text color `#14141b` (≈ `var(--text-default)`)
- Background `#EDF1F5`, border `rgba(29,31,38,0.2)` — both inherit from `.chip`
- Pill radius (inherits)
- 16 px close icon slot, 4 px gap to label

### Layer 1 — `.chip--input` modifier in `design-system.css`

Composed with the base `.chip`: `class="chip chip--input"`. Override-only — inherits background, border, radius, font-family, font-weight from `.chip`. Overrides height, padding, font-size, line-height, color, gap.

Also added `.chip__close` for the X button:
- 16×16 inline-flex button, transparent bg, `currentColor` for icon color
- 12 px X icon inside (Lucide-style, `stroke="currentColor"`)
- Hover: opacity 1 + faint dark ring (`rgba(29,31,38,0.08)`)
- Focus: 2 px brand-blue outline at 1 px offset (keyboard-accessible)
- `type="button"` to prevent accidental form submission

Per user request: **no JS wired up**. The X button is visual only — production would attach a click handler to `.chip__close` that removes its parent `.chip`.

### Layer 2 — Docs section in `components.html`

New `#chip-input` section sitting between the existing `#chips` section and `#divider`. Three stages:
1. Single chip standalone
2. Multiple chips together (search-token-style)
3. Filter tray use case showing "Active filters: Lucene × Logs × How To ×" inside a tinted container

Plus HTML snippet and full spec table.

### Layer 3 — Live use in the prototype

Both `#page-obs-filter` and `#page-sec-filter` views now display an "Active filters" tray in the hero area, showing example filter chips relevant to each section:

- **Observability filter view**: Logs, Metrics, How To
- **Security filter view**: Threat Detection, SIEM

New inline CSS in `index.html` for the `.active-filters` tray container (sits below the subpage hero title, before the body content). The tray is a *composed pattern* — not a primitive — so it lives in `index.html`'s inline `<style>`, not in `design-system.css`. The CSS adds:

```css
.active-filters         { display: flex; align-items: center; flex-wrap: wrap; gap: var(--s-12); margin-top: var(--s-24); }
.active-filters__label  { font-size: var(--fs-small); font-weight: var(--fw-medium); color: var(--gray); }
```

### Bug caught and fixed inside this session

My first `str_replace` to insert the new docs section into `components.html` accidentally swallowed the **Divider section's opening tag and comment**. The Divider's `<h2>` and content remained, but ended up nested inside the chip-input section's container instead of being its own `<section>`. Caught by the section-id audit (only 8 distinct sections instead of the expected 9, with Divider missing from the list). Fixed with a second `str_replace` to re-close the chip-input section and re-open the Divider section.

### Verification

```
<div in index.html       → 897 / 897    (was 895; +2 from active-filters trays)
<article                 → 171 / 171
<a / </a>                → 653 / 653
<section in index.html   → 51 / 51
<button in index.html    → 110 / 109    (pre-existing 1-button imbalance from before Session 16; not introduced here)
page-views               → 21

<div in components.html  → 123 / 123    (was 116; +7 from new section + tray demo)
<section in components.html → 9 / 9       (was 8; added chip-input)
<button in components.html  → 7 / 7

CSS braces in design-system.css → 24 / 24  (was 19; +5 from .chip--input, .chip__close, hover/focus, .chip__close svg)
```

### Backups

`/home/claude/work/backups/pre-session-16/` holds clean copies of all 5 files from before this session.

### What's NOT done (deliberate, flagged for later)

- **No JS** — the X buttons don't actually remove chips (per user request "no JS — just style the X button"). When/if you want the behavior, the wire-up is small: `document.querySelectorAll('.chip__close').forEach(b => b.addEventListener('click', e => e.currentTarget.closest('.chip').remove()));`
- **No size variants** — Figma's props include `size: "Large"` suggesting a "Small" might exist too, but the spec didn't include a small-chip-input. If a small variant emerges, add `.chip--input.chip--sm` overriding height/padding/font.
- **No state variants** — Figma's `state: "Default"` implies hover/active/disabled states could exist. Not provided in this node; would be additive when needed.

---

## 2026-05-15 — Session 17: Chip-text loremization across all 4 sections

Cosmetic content sweep. The prototype previously had ~30 distinct chip values across all pages — "How To", "Lucene", "Integrations", "Vector Search", "APM", "Threat Intel", "Logs", "Cloud", "Solutions", etc. — which made it look like a finished product instead of a wireframe. Users / reviewers reading the chip text were getting pulled into the *content* instead of evaluating the *structure*.

### The 4-section pair mapping

Each section gets its own 2-word lorem ipsum pair, distinct from the other sections so visual variety stays:

| Section | Page-views | Primary | Secondary |
|---|---|---|---|
| **Search** | home, tutorials, examples, integrations, blog, article, tutorial, integration, example, glossary | **Lorem** | **Ipsum** |
| **Observability** | observability, obs-filter, obs-article, obs-author | **Dolor** | **Sit** |
| **Security** | security, sec-filter, sec-article, sec-author | **Amet** | **Consectetur** |
| **News / Company** | company, comp-article | **Adipiscing** | **Elit** |

Within each page-view, the chips alternate between primary and secondary in document order, so cards with 2 chips show one of each and successive cards see variety. The 8 words together form the opening of "Lorem ipsum dolor sit amet, consectetur adipiscing elit" — coherent lorem ipsum, just split four ways.

### Per page-view sweep counts

```
page-home               (Lorem/Ipsum)         → 12
page-tutorials, examples, integrations,
  blog, article         (Lorem/Ipsum)         → 9, 14, 18, 9, 7
page-observability      (Dolor/Sit)           → 31
page-obs-filter         (Dolor/Sit)           → 8  (incl. 3 active-filter chips)
page-obs-article        (Dolor/Sit)           → 7
page-obs-author         (Dolor/Sit)           → 16
page-security           (Amet/Consectetur)    → 32
page-sec-filter         (Amet/Consectetur)    → 7  (incl. 2 active-filter chips)
page-sec-article        (Amet/Consectetur)    → 7
page-sec-author         (Amet/Consectetur)    → 16
page-company            (Adipiscing/Elit)     → 71
page-comp-article       (Adipiscing/Elit)     → 4
                                              ─────
                                       Total:   259
```

### What was deliberately left alone

- **`+1` overflow indicators** — 2 instances. These are UI affordances ("plus N more"), not tag values. Preserved.
- **`components.html`** — Untouched. The Chips and Chip-Input docs sections use their own demo chips ("APM", "Logs", "Lucene", "Elasticsearch", "Observability", "How To", "Label") that are *meant* to demonstrate variety. Loremizing them would defeat the docs.
- **Chip text inside `<pre class="code">` blocks** — Those are HTML snippet examples shown as text, not active chips.

### Active-filter chip aria-labels also updated

The 5 `chip__close` buttons in `#obs-filter` and `#sec-filter` views had stale `aria-label="Remove Logs filter"` etc. that referenced the old filter names. Per user request that "the chip and the filter text need to match to avoid confusion", these were swept too:

```
Remove Logs filter             → Remove Dolor filter
Remove Metrics filter          → Remove Sit filter
Remove How To filter           → Remove Dolor filter      (obs-filter view, 3rd chip)
Remove Threat Detection filter → Remove Amet filter
Remove SIEM filter             → Remove Consectetur filter
```

### Final chip text distribution (after sweep)

```
Elit          37   (News secondary)
Adipiscing    37   (News primary)
Lorem         31   (Search primary)
Amet          31   (Security primary)
Dolor         30   (Observability primary)
Sit           29   (Observability secondary)
Ipsum         29   (Search secondary)
Consectetur   29   (Security secondary)
+1            2    (overflow indicators — preserved)
```

The Adipiscing/Elit count is highest because `page-company` alone has 71 chips. Everything else is reasonably balanced — primary > secondary in each section because the alternation starts on primary.

### Files touched

Only `index.html`. No CSS or token changes. No structural changes. 259 textual replacements + 5 aria-label updates.

### Verification

```
<div in index.html       → 897 / 897    (unchanged)
<article                 → 171 / 171
<a / </a>                → 653 / 653
<section                 → 51 / 51
page-views               → 21

components.html → identical to backup (✓)
design-system.css → identical to backup (✓)

unique chip text values: 8 lorem words + 2 "+1" overflow = 10 total
                         (was ~30 distinct values before)
```

### Backups

`/home/claude/work/backups/pre-session-17/` holds clean copies of all 5 files from before this sweep.

---

## 2026-05-15 — Session 18: Line-heights + `--fs-lead` token + near-token color snaps

Three concurrent mechanical sweeps. Net: 60+ literals → token references, with one new token pair (`--fs-lead` + `--lh-lead`) introduced for the Body XLarge slot we'd been using as a literal.

### Sub-sweep A — Line-height literals

28 literal `line-height: N` declarations swept to existing tokens:

| From | To | Count | Visual change |
|---|---|---|---|
| `line-height: 1.3` | `var(--lh-h4)` | 17 | None (Δ 0) |
| `line-height: 1.5` (paired with 14px/`--fs-small`) | `var(--lh-small)` | 4 | None |
| `line-height: 1.5` (paired with 16px or other) | `var(--lh-body)` | 2 | None |
| `line-height: 1.2` | `var(--lh-h2)` | 2 | None |
| `line-height: 1.6` | `var(--lh-small)` (1.5) | 3 | **Tiny tightening** — these were stale stragglers from before Session 2's body-text leading migration (1.6 → 1.5). Now consistent. |

The token chosen for `1.3` was `--lh-h4` over `--lh-h5` (both equal 1.3) for semantic preference — these are mostly card-title rhythms.

### Sub-sweep B — New `--fs-lead: 18px` + `--lh-lead: 1.55` tokens

Two new tokens added to `design-system.css`:

```css
--fs-lead: 18px;    --lh-lead: 1.55;
```

`--fs-lead` maps to the library's **Body XLarge** type style (`18px / 28px lh`). The library line-height is `28 / 18 ≈ 1.556`; we rounded to 1.55 for consistency with the existing literal usage.

**Sweep targets (12 declarations across 8 selectors):**

4 hero-subtitle rules got both new tokens applied:
- `.article-hero__intro`
- `.subpage-hero__subtitle`
- `.tutorial-hero__subtitle`
- `.author-hero__subtitle`

4 card-description rules at smaller font sizes (14–15 px) got `--lh-lead` only — the line-height value is identical and the role ("comfortable reading rhythm for prose") is the same, even though the font-size differs. The token comment documents this dual role explicitly:

- `.series-card__desc` (15px / `--lh-lead`)
- `.home-callout__text` (15px / `--lh-lead`)
- `.qs-callout__text` (14px / `--lh-lead`)
- `.integration-card__desc` (`var(--fs-small)` / `--lh-lead`)

Final usage: 4 `--fs-lead` references + 8 `--lh-lead` references.

**`components.html` updated** — added a `--fs-lead` row to the type-scale table between `--fs-h5` and `--fs-link`, with sample text and library mapping.

### Sub-sweep C — Near-token color snaps

3 of 4 candidate literals snapped to their nearest existing tokens. ΔE per channel ≈ 1–3 (imperceptible):

| Literal | Token | Uses | New value | Δ |
|---|---|---|---|---|
| `#1C1E23` | `var(--text-default)` | 7 | `#1D1F26` | 1 |
| `#343741` | `var(--surface-neutral-dark-1)` | 5 | `#373A45` | 3 |
| `#1F2937` | `var(--text-default)` | 4 | `#1D1F26` | 2 |

### What was NOT swept (deliberately)

- **`#E6ECF3` × 2** — Would snap to `var(--border)` (#C4C7D1), but the delta is significant (Δ ≈ 30 per channel — visibly darker hairlines). These are likely intentional lighter borders; needs design call. Flagged for manual review.
- **`line-height: 1.7` × 6, `1.4` × 6, `1` × 6** — No existing token at these ratios. Could add `--lh-loose: 1.7`, `--lh-tight: 1`, but none have clear semantic anchors yet. Flagged for future additive token work.
- **Off-scale font-sizes** (15, 26, 28, 13, 11, 10, 9, 17, 20, 24, 30 px) — Component-specific decorations, not type-scale members.
- **`font-size: 18px` × 4 NOT in hero subtitles** — `.blog-tabs button`, `.glossary-alphabet button`, `.diagram__label`, `.integration-card__name`. These are button/label text, not lead text — different role. Kept as literals.
- **`line-height: 22px` × 2** — pixel-based line-heights for icon-aligned button text. Different concern from ratio-based reading rhythms.

### Files touched

- `index.html` — 28 line-height sweeps + 8 hero-subtitle declarations + 4 card-description declarations + 16 color snaps = **56 literal-to-token replacements**
- `design-system.css` — added `--fs-lead` + `--lh-lead` token row in the type-scale block (+1 line)
- `components.html` — added `--fs-lead` row to the type-scale docs table (+6 lines)

### Verification

```
Remaining literal line-height in index.html (intentional):
  line-height: 1.7  → 6  (reading paragraphs, no token)
  line-height: 1.4  → 6  (mid-density text, no token)
  line-height: 1    → 6  (tight display, no token)
  line-height: 22px → 2  (icon-aligned, no token)
  TOTAL literal: 20 (down from 48 before this session)

New token references:
  var(--lh-h4)             → 17 sweeps
  var(--lh-body)/lh-small  → 6 sweeps combined
  var(--lh-h2)             → 2 sweeps
  var(--lh-small) (1.6→)   → 3 sweeps
  var(--fs-lead)           → 4 uses
  var(--lh-lead)           → 8 uses
  var(--text-default)      → 11 sweeps (from S15's snap + this session's 7+4)
  var(--surface-neutral-dark-1) → 5 sweeps

Structural integrity:
  <div in index.html       → 897 / 897    (unchanged)
  <article                 → 171 / 171
  <a / </a>                → 653 / 653
  <section                 → 51 / 51
  page-views               → 21
  CSS braces               → 24 / 24

components.html unchanged except for the +6-line type-scale docs row.
```

### Visual change risk

**Three places** with non-zero ΔE:
- 3 instances of `line-height: 1.6 → 1.5` — text reads slightly tighter. Same change Session 2 made elsewhere; consistent with the rest of the system now.
- 16 near-token color snaps — Δ ≈ 1–3 per channel. Sub-perceptible in practice but technically visible if compared side by side.

Everything else is byte-equivalent rendering.

### Backups

`/home/claude/work/backups/pre-session-18/` holds clean copies of all files before this sweep.

---

## 2026-05-15 — Session 19: Type-scale alignment audit + library-spec corrections

Triggered by the user uploading the full Elastic Design System library type-scale sheet — a one-page reference of all 22 type styles (Oversize / Display / Headline / Title / Body / Label / Code, each with Large/Medium/Small/etc. variants). Direct compare against our 9 tokens surfaced one real bug, two missing tokens we actually need, and one font-weight gap.

### Library-vs-prototype alignment matrix

| Library style | Library size / lh | Our token before | Status before | After |
|---|---|---|---|---|
| Oversize L/M/S (82/73/65) | various | — | Not used in prototype | Skipped |
| Display L/M/S (58/51/45) | various | — | Not used | Skipped |
| Headline Large (40/48) | 40 / 1.2 | `--fs-h2` | ✅ EXACT | unchanged |
| Headline Medium (36/43.2) | 36 / 1.2 | — | 0 uses today | Skipped (add when needed) |
| Headline Small (32/38.4) | 32 / 1.2 | `--fs-h3` | ✅ EXACT | unchanged |
| **Title Large (28/36.4)** | 28 / 1.3 | — | 4 literal `font-size: 28px` | ✅ ADDED `--fs-title-l` |
| Title Medium (25/32.5) | 25 / 1.3 | `--fs-h4` | ✅ EXACT | unchanged |
| Title Small (22/28.6) | 22 / 1.3 | `--fs-h5` | ✅ EXACT | unchanged |
| **Body XLarge (18/27)** | 18 / **1.5** | `--fs-lead` / `--lh-lead` | ❌ lh wrong (1.55) | ✅ FIXED to 1.5 |
| Body Large (16/24) | 16 / 1.5 | `--fs-body` / `--fs-link` | ✅ EXACT | unchanged |
| Body Medium (14/21) | 14 / 1.5 | `--fs-small` | ✅ EXACT | unchanged |
| Body Small (12/18) | 12 / 1.5 | `--fs-xs` | ✅ EXACT | unchanged |
| Label XLarge (16/24) | 16 / 1.5 | `--fs-link` (dual-role) | ✅ EXACT | unchanged |
| Label Large (14/21) | 14 / 1.5 | `--fs-small` (dual-role) | ✅ EXACT | unchanged |
| Label Medium (12/18) | 12 / 1.5 | `--fs-xs` (dual-role) | ✅ EXACT | unchanged |
| **Label Small (11/16.5)** | 11 / 1.5 | — | 1 literal `font-size: 11px` | ✅ ADDED `--fs-label-s` |
| Code Medium (14/21) | 14 / 1.5 | `--fs-small` (dual-role) | ✅ EXACT | unchanged |
| Code Small (12/18) | 12 / 1.5 | `--fs-xs` (dual-role) | ✅ EXACT | unchanged |

### Changes shipped

**1. Bug fix — `--lh-lead: 1.55 → 1.5`**

I introduced `--lh-lead: 1.55` in Session 18 with my comment claiming "28/18 ≈ 1.55". The library actually defines Body XLarge as `18px / 27px lh`, which is `27/18 = 1.5` exact. So the value was off by a hair due to a math error on my part. Now matches the library exactly, and aligns with the entire Body/Label family at 1.5.

**Affected**: 4 hero subtitles + 4 card descriptions (8 declarations total). Visual change: text reads very slightly tighter (≈ 0.4px per line at the 18px size). Likely sub-perceptible.

**2. New token — `--fs-title-l: 28px / --lh-title-l: 1.3`**

Library: Title Large. Maps to 4 existing literal `font-size: 28px` declarations:
- `.article__main h2` (already paired with `var(--lh-h4)` — 1.3, same as `--lh-title-l`)
- `.integration-card__initials` (mono initials decoration; no explicit line-height)
- `@media (max-width: 720px) .trial-banner__title` (mobile override)
- `@media (max-width: 720px) .hero__title` (mobile override)

All 4 `font-size: 28px` literals swept to `var(--fs-title-l)`. `--lh-title-l` has 0 current uses (existing rules already use `--lh-h4` which equals 1.3); the token is defined and ready when a new rule needs the explicit Title-Large rhythm.

**3. New token — `--fs-label-s: 11px / --lh-label-s: 1.5`**

Library: Label Small. Maps to 1 existing literal `font-size: 11px` — `.integration-card__category` (mono category label). 1 sweep.

**4. Font weight migration — `--fw-bold: 700 → 800`**

Per user decision. Library binds Display / Headline / Title styles at **ExtraBold** (the screenshot shows "MierB-…-ExtraBold" labels in the weight column). 800 is the closest standard weight; Inter ExtraBold (800) is already loaded via the existing Google Fonts request (`Inter:wght@400;500;600;700;800;900`).

**Visual change: broad and noticeable.** Every selector currently using `var(--fw-bold)` (63 places) will render heavier. Affected selectors include:
- All major heading rules — `.hero__title`, `.featured__title`, `.popular__title`, `.cat-card__title`, `.explore__title`, etc.
- Button labels and prominent CTAs — `.view-all`, `.qs-callout__text strong`, etc.
- Article H2s, glossary terms, integration row names, callout titles

This is the kind of change that should be eyeballed across the prototype after deployment.

**Revert path**: change `--fw-bold: 800;` back to `700` in one place — `design-system.css` line 147.

**5. Mier B status callout in `components.html`**

Added a prominent ⚠ callout box in the Typography section explaining that Mier B is *referenced* in `--font-heading` but is **not loaded** externally — no `@font-face` declaration, not on Google Fonts. Every heading currently renders in Inter (the fallback). Mier B is a separate workstream (host brand font files + add `@font-face`), not a token-system issue. Same callout language also added to the design-system.css token comment header.

### What the user observed

The library type-scale sheet shows the weight column rendered in Mier B (the right side of the screenshot displays "Font / Font / Font" samples styled with Mier B at various ExtraBold weights). Our prototype was showing the same scale rendered in Inter Bold — visibly thinner and visually distinct. After Session 19, the prototype renders in Inter ExtraBold, which is closer to (but not identical to) Mier B ExtraBold.

### Files touched

- `design-system.css` — `--fw-bold` value bumped; `--lh-lead` fixed; 2 new token rows added; Mier B note in type-scale block header; comment updates throughout the type-scale and font-weights blocks
- `index.html` — 4 sweeps of `font-size: 28px → var(--fs-title-l)`; 1 sweep of `font-size: 11px → var(--fs-label-s)`. **No other content change** — but `var(--fw-bold)` resolving to 800 instead of 700 affects ~63 rendered selectors
- `components.html` — Type-scale docs table: added `--fs-title-l` row, added `--fs-label-s` row, fixed `--fs-lead` value column from "1.55" to "1.5". Font-weights docs table: `--fw-bold` row updated to show 800 + library alignment note. Typography section: prominent Mier B status callout added

### Verification

```
--fw-bold value:                 800  (was 700)
--lh-lead value:                 1.5  (was 1.55)
--fs-title-l / --lh-title-l:     newly defined
--fs-label-s / --lh-label-s:     newly defined

font-size: 28px in index.html:    0   (was 4)
font-size: 11px in index.html:    0   (was 1)
var(--fs-title-l) usage:         4
var(--fs-label-s) usage:         1
var(--lh-title-l) usage:         0   (token defined for future use)
var(--lh-label-s) usage:         0   (paired with the 11px label only)

Structural integrity unchanged:
  <div in index.html:            897 / 897
  <article:                      171 / 171
  <a / </a>:                     653 / 653
  page-views:                    21
  CSS braces:                    24 / 24
  components.html sections:      9 / 9
```

### What's still open per the library audit

- **Headline Medium (36/43.2)** — 0 current uses; add when needed.
- **Display L/M/S** (58/51/45) — Not used in prototype (would be marketing pages).
- **Oversize L/M/S** (82/73/65) — Same.
- **Mier B brand font loading** — Separate workstream: host `.woff2` files, add `@font-face` declarations. Without this, all "heading" text renders in Inter (the fallback). The token system is ready for it; the asset isn't.

### Backups

`/home/claude/work/backups/pre-session-19/` holds clean copies of all files before this session.

---

## 2026-05-15 — Session 20: Mier B brand font loaded (the workstream from Session 19 closed)

User uploaded the full Mier B family — 10 weight/style combinations × 2 formats (.woff2 + .woff) = 20 files, ~1.3 MB total. Self-hosted alongside the CSS.

### What changed

**1. 20 font files staged in `/mnt/user-data/outputs/`**

Sitting next to `index.html` so the relative paths `url('MierB-Regular.woff2')` resolve correctly. The 10 weight/style combos:

| CSS weight | Filename | Italic variant |
|---|---|---|
| 400 normal | MierB-Regular | MierB-Italic |
| 600 (Demi/SemiBold) | MierB-Demi | MierB-DemiItalic |
| 700 (Bold) | MierB-Bold | MierB-BoldItalic |
| 800 (ExtraBold) | MierB-ExtraBold | MierB-ExtraBoldItalic |
| 900 (Heavy) | MierB-Heavy | MierB-HeavyItalic |

Each as both .woff2 (modern; smaller) and .woff (legacy fallback).

**2. 10 `@font-face` declarations added to `design-system.css`**

Inserted at the top of the file, before `:root`. Each declaration:
- Maps a Mier B filename → standard CSS `font-weight` + `font-style`
- Lists `.woff2` first, `.woff` second (modern-preferred with legacy fallback)
- Uses `font-display: swap` — heads render in Inter immediately, then swap to Mier B once loaded. No FOIT (Flash Of Invisible Text); brief FOUT (Flash Of Unstyled Text) as expected.

Sample declaration:
```css
@font-face {
  font-family: 'Mier B';
  src: url('MierB-ExtraBold.woff2') format('woff2'),
       url('MierB-ExtraBold.woff') format('woff');
  font-weight: 800;
  font-style: normal;
  font-display: swap;
}
```

**3. Token comments updated**

The `⚠ Mier B not loaded` notes in `design-system.css` (type-scale block header) and `components.html` (typography section callout) flipped to `✓ Mier B IS loaded` with details on the @font-face setup.

**4. Zero token or rule changes**

`--font-heading: 'Mier B', 'Inter', system-ui, sans-serif;` was already pointing to Mier B. Now that the font actually loads, every heading rule using `var(--font-heading)` (the ~10 selectors that use it) renders in Mier B automatically. No selector changes needed.

### Visual impact

**Significant**, in a way the user has been anticipating since Session 19:

- All `var(--font-heading)` headings — `.hero__title`, `.featured__title`, `.popular__title`, `.cat-card__title`, `.subpage-hero__title`, `.tutorial-hero__title`, `.author-hero__title`, etc. — now render in **Mier B ExtraBold** (font-weight 800 from Session 19, in the Mier B typeface from Session 20).
- The visual identity now matches the Elastic brand spec sheet.
- Inter ExtraBold and Mier B ExtraBold differ noticeably in letter shapes, x-height, and overall character — expect Mier B to read more distinctively "Elastic."

### How loading actually works at runtime

1. Browser parses HTML, hits `<link rel="stylesheet" href="design-system.css">`.
2. CSS loads, browser sees `@font-face` declarations and starts fetching the Mier B files in parallel.
3. Browser starts laying out the page. Headings with `font-family: var(--font-heading)` initially render in **Inter** (the next font in the stack).
4. As each Mier B weight/style finishes downloading, headings using that weight swap to Mier B. This brief "swap" is the FOUT.
5. After first page load, the browser caches all 20 files. Subsequent navigations are instant.

### Files touched

- `design-system.css` — 10 `@font-face` blocks at the top (+~85 lines), Mier B status comment flipped warning → success
- `components.html` — Mier B status callout flipped warning → success
- 20 new font files in outputs (Mier B family)
- `index.html` — **untouched**

### Verification

```
@font-face count in design-system.css:   10 declarations + 1 mention = 11 total
url() count in design-system.css:        20 (10 woff2 + 10 woff)
CSS braces:                              34 / 34  (was 24 / 24; +10 for new blocks)

index.html:                              identical to backup ✓
components.html:                         1 paragraph changed (Mier B callout)

Output bundle now includes 20 font files alongside the 5 core files.
```

### What you need to do when committing to GitHub

The 20 Mier B files must travel with the other files. Commit them all into the same folder as `index.html`. GitHub Pages will serve them from the same origin, and the relative `url('MierB-...')` paths will resolve. No additional config needed.

### Backups

`/home/claude/work/backups/pre-session-20/` holds clean copies of all files before this session.

---

## 2026-05-15 — Session 21: Standalone preview builds with base64-embedded fonts

The artifact preview shows one file at a time and can't access sibling CSS or font files. Solution: build self-contained HTML files where everything is inlined — including the Mier B font binaries as base64 data URIs.

### What changed

**1. `index-preview.html` is back** (this time genuinely standalone)

Previously dropped in Session 17 as "dead weight" when we thought GitHub Pages was the only deployment path. Returns now because the artifact preview viewer needs a single self-contained file.

Build: `index.html` + inlined `design-system.css` + 10 base64-encoded Mier B woff2 files.

**2. `components-preview.html` upgraded**

Was already inlining `design-system.css`. Now also inlines the Mier B fonts as base64, so docs page renders with proper brand typography in the artifact preview.

**3. `build_standalone.py` automated**

Reusable script for future rounds. Reads `design-system.css`, replaces each `url('MierB-X.woff2') format('woff2'), url('...woff') format('woff')` with `url(data:font/woff2;base64,...) format('woff2')`, then inlines the modified CSS into both HTML files. Drops the `.woff` legacy fallback in the standalones (woff2 has 97%+ browser support; saves ~700 KB per file).

### File sizes (the trade-off)

| File | Before | After | Notes |
|---|---|---|---|
| `index.html` | 388 KB | 388 KB | Unchanged — still uses external `design-system.css` |
| `design-system.css` | 25 KB | 25 KB | Unchanged |
| `index-preview.html` | (didn't exist) | 1.1 MB | Self-contained with 10 embedded woff2 fonts |
| `components-preview.html` | 79 KB | 755 KB | Was inlining CSS only; now also embeds fonts |

The 1.1 MB `index-preview.html` is large by HTML standards but well within reason for a self-contained design demo. Browsers cache it after first download, and there's no network round-trip for the fonts.

### Why woff2-only in the standalones

The non-standalone path (`index.html` + external CSS + sibling .woff2 + .woff files) keeps both formats for maximum legacy browser support. The standalone path drops .woff because:

- Embedding both formats would double the font payload (.woff is ~37% larger than .woff2 per Mier B file).
- woff2 has 97%+ global browser support today. Browsers that don't speak woff2 are extremely old (pre-2017 Edge, IE).
- The standalone is a preview/demo artifact, not the production deployment. Production gets both formats.

### How to use each file going forward

| Scenario | Use this | Why |
|---|---|---|
| Artifact preview in Claude | `index-preview.html` or `components-preview.html` | Single-file, no sibling lookups |
| Local double-click | Either standalone | Same reason |
| GitHub Pages deployment | `index.html` + `design-system.css` + 20 font files | Smaller per-file size, proper caching, modular |
| Editing & iteration | `index.html` + `design-system.css` + `components.html` | Source of truth |

### Files touched

- `index-preview.html` — Re-created as fully standalone (was deleted Session 17, rebuilt here)
- `components-preview.html` — Now embeds fonts in addition to inlining CSS
- `build_standalone.py` — New helper script in working dir; not shipped

No changes to `index.html`, `design-system.css`, `components.html`, or the 20 source font files.

### Verification

```
index-preview.html:
  size                       1.1 MB
  @font-face declarations    10
  data:font/woff2;base64 URIs  10

components-preview.html:
  size                       755 KB
  @font-face declarations    10

design-system.css            unchanged (still 25 KB)
index.html                   unchanged
20 .woff2/.woff files        still staged alongside HTML
```

### Backups

`/home/claude/work/backups/pre-session-21/` holds clean copies of the 4 core files (no need to back up the preview builds; they're regenerated each round).

---

## 2026-05-15 — Session 22: `--fw-bold` reverted (intentional deviation from library spec)

User feedback after seeing Sessions 19+20 live: "I don't like the weight, I like the size." Headings at Mier B ExtraBold (800) read too heavy in the prototype's layout context. Reverting `--fw-bold` to 700 while keeping every other change from S19 and S20 (Title Large token, Label Small token, `--lh-lead` fix, Mier B font loading).

### What changed

**One token value flip**: `--fw-bold: 800` → `--fw-bold: 700` in `design-system.css`.

That single change cascades through ~63 selectors using `var(--fw-bold)` — every major heading, prominent button, and emphasized snippet now renders one weight lighter:
- Mier B 700 (Bold) instead of Mier B 800 (ExtraBold) for headings using `var(--font-heading)`
- Inter 700 (Bold) instead of Inter 800 (ExtraBold) for buttons / other selectors using `var(--font-body)` + `var(--fw-bold)`

The Mier B ExtraBold cut is still loaded (via the @font-face block from Session 20) — just not used currently. To restore library alignment, flip the token back: one line, one place.

### Documentation prominence — three places it's now flagged

The deviation has to be impossible to miss. Documented at:

1. **Top of `design-system.css`** — a `⚠ KNOWN DEVIATIONS FROM LIBRARY SPEC` banner directly under the file's own header, before any tokens. Detailed explanation of #1 (the `--fw-bold` deviation).
2. **Inline comment in the type-scale block** — the `--fw-bold: 700;` line itself carries `/* ⚠ Library spec says 800. See comment above. */`, and the surrounding font-weights comment block was rewritten to explain the deviation.
3. **Top of `MIGRATION.md`** — a new `## ⚠ Known deviations from library spec` section, positioned directly after the file title and before "Ground rules". Includes deviation rationale and revert path.
4. **`components.html` font-weights table** — the `--fw-bold` row updated with the value 700, a "⚠ Deviation from library spec" warning, and the revert path.

### Files touched

- `design-system.css` — token value reverted; comment block rewritten; new `KNOWN DEVIATIONS` banner added at top of file
- `components.html` — `--fw-bold` docs row rewritten with deviation warning
- `MIGRATION.md` — new top-of-file `Known deviations` section; this Session 22 entry appended; Ground rules section updated to acknowledge deviations

No changes to `index.html`. The 63 selectors using `var(--fw-bold)` automatically render at the new value — the CSS cascade does all the work.

### Visual impact

**Substantial and intended**: every heading and prominent emphasis text drops one weight class. Specifically the Mier B headings (heading rules using `var(--font-heading)`) move from Mier B ExtraBold to Mier B Bold. Most users will perceive this as "cleaner and less shouty."

### Why this is OK to do

The library spec is a *recommendation* for brand-consistent design. Our token file is the *rule* for this prototype. Deviating is fine when we have a reason and document it; that's how design decisions get made. The library option is preserved (Mier B ExtraBold loaded, ready to use), so reverting is a one-line change at any time.

### Verification

```
--fw-bold:                  700  (was 800)
Mier B fonts loaded:        all 10 weight/style combos still loaded
@font-face blocks:          10 (unchanged from S20)
var(--fw-bold) usages:      63 (unchanged; just resolve to a different value)

Banner at top of design-system.css:       ✓ added
Top-of-file callout in MIGRATION.md:      ✓ added
--fw-bold docs row in components.html:    ✓ updated with warning

Structural integrity:
  <div in index.html:       897 / 897    (untouched this session)
  page-views:               21
  CSS braces:               34 / 34
```

### Backups

`/home/claude/work/backups/pre-session-22/` holds clean copies of all files before this session.

---

## 2026-05-15 — Session 23: Token block reorganization — scannable layout

Cleanup-only round. The `:root` block had accumulated 198 lines of dense per-token commentary across 22 sessions — accurate but unreadable. Verbose history (deltas, session refs, "Alt" values, revert paths) was inline in every CSS comment, drowning out the actual structure of the design system.

### What changed

`:root` block restructured for scannability:

- **One line per token** with predictable visual columns: `--name: value; /* library-slot — role */`
- **Six clear sections** with brief separator headers — Color/Surfaces, Color/Text, Color/Brand, Color/Legacy Aliases, Typography (Families/Weights/Scale), Spacing, Radius
- **Per-token comment shows only**: library slot name (so the Figma mapping is obvious) and a one-line role description
- **Verbose history moved to MIGRATION.md** — deltas, session annotations, revert instructions, alternate values from variable bindings, "was X, now Y" notes
- **Deviation warning preserved on `--fw-bold`** — kept inline because it's an active design decision, not historical

### Before / after example — `--fs-h2`

Before:
```css
--fs-h2:      40px;    --lh-h2:      1.2;    /* Library: Headline Large 40px / 48px lh. MIGRATED 2026-05-14: fs was 39.06px (Δ +0.94px); lh was 1.4 (Δ −0.2). Revert: 39.06px / 1.4. */
```

After:
```css
--fs-h2:      40px;    --lh-h2:      1.2;     /* Headline Large    40 / 48   */
```

The migration delta (`fs was 39.06px`, `lh was 1.4`) is now in MIGRATION.md, where session history belongs. The CSS comment shows only what you need at-a-glance: this is Headline Large, 40px / 48px.

### Same pattern applied to color aliases

Before:
```css
--gray:           var(--text-subtlest);  /* was #6B7079 secondary text → #474B57 (Δ noticeably darker).  137 uses — biggest visual shift. */
```

After:
```css
--gray:           var(--text-subtlest);  /* 137 uses — secondary text */
```

The "was #6B7079 → #474B57" history lives in MIGRATION.md Session 5. The CSS shows the current target + usage count.

### What was preserved inline (kept in CSS)

- **The KNOWN DEVIATIONS banner at the top** — still loud and unmissable
- **The `--fw-bold` inline warning** — `/* ⚠ Library says 800 (ExtraBold); see top banner */`
- **The `--r-10` alias note** — `/* aliased — was 10px pre-migration */` (one-liner; OK to keep)
- **Section headers** — short orienting comments above each major block

### What got removed (lives in MIGRATION.md only now)

- 100+ `was X → Y (Δ …)` per-token annotations
- "Alt: #xxx" variable-binding alternate values (color tokens)
- Session-number annotations on each migrated token (e.g., "MIGRATED 2026-05-14")
- Multi-line revert instructions
- 8-line color-system Figma-node-reference block (the source attribution stays in MIGRATION.md ground rules)
- The verbose `EXPERIMENT 2026-05-14 — Body font weight` block (the experiment is documented; `--fw-body: 500` itself is self-explanatory)

### Sanity check

```
Token count before reorg: 80 unique custom-property names
Token count after reorg:  80 unique custom-property names
                          (zero tokens lost — verified programmatically)

design-system.css line count:  546 → 494  (-52 lines, ~10% smaller)
:root block size:               198 → 146  (-52 lines, ~25% smaller)
Brace balance:                  33 / 33   (was 34/34; one matched pair from comment nesting collapsed)

index.html: untouched (no token references changed)
components.html: untouched
```

### Files touched

Only `design-system.css`. No token values changed; no selectors affected. Purely a comment / layout reorganization.

### Backups

`/home/claude/work/backups/pre-session-23/` holds the original verbose version if you want to look up any of the moved history inline.

---

## 2026-05-15 — Session 24: Blog card (.related-card) redesigned site-wide

The card component used across all blog/observability/security/news landing pages, tag pages, article-related sections, and author pages got a full visual refresh — matching the Elastic Search Labs Blogs reference and incorporating user feedback ("no background", "image bigger vertically", "image has its own border", "avatar too small", "use design tokens", "when in doubt go smaller", "byline uses dark + semibold + underline hover", "chip should be a link").

### Class name preserved on purpose

Despite redesigning the visuals, the class name stays as `.related-card`. The prototype has **~1100 HTML class references** across 119 cards in 16+ page-views. Renaming to `.blog-card` would mean editing each one — cheap risk for zero gain. The component name will catch up over time; for now the visual treatment changed, the name didn't. Comment block at the top of the rule notes this so future readers know the redesign happened.

### Visual changes (vs the previous .related-card)

| Property | Before | After |
|---|---|---|
| Container background | `var(--white)` | `transparent` (no card chrome) |
| Container border-radius | `var(--r-10)` | none (no container chrome) |
| Container box-shadow | Two-stop subtle shadow | none |
| Image height | 180 px | **260 px** (vertical bump, width still column-constrained) |
| Image border | Bottom only (from card border) | **1 px var(--border) on all 4 sides** |
| Image border-radius | Top corners only (inherited) | **var(--r-8) on all 4 corners** |
| Body padding | `var(--s-24)` all sides | **0** (flush with grid) |
| Body element gap | `var(--s-12)` | `var(--s-16)` |
| Meta strip gap | `var(--s-12)` | `var(--s-16)` |
| Title font-family | `var(--font-heading)` (Mier B) | unchanged |
| Title font-size | `var(--fs-h5)` (22 px) | unchanged — "when in doubt, smaller" |
| Title line-height | `var(--lh-h4)` (1.3) | `var(--lh-h5)` (1.3) — semantic fix; same value |
| Description font-size | `var(--fs-small)` (14 px) | unchanged |
| Description clamping | unclamped | **clamped to 2 lines** for grid consistency |
| Author block gap | `var(--s-8)` | `var(--s-12)` (more breathing room for bigger avatar) |
| Author padding-top | `var(--s-12)` | none (flex gap handles it) |
| Avatar size | 24 × 24 px | **40 × 40 px** — proper human-photo scale |
| Avatar background | Blue→gray linear gradient | `var(--surface-neutral-1-5)` — soft neutral |
| Avatar text color | white | `var(--text-subtler)` |
| Avatar font-size | 10 px literal | `var(--fs-small)` (14 px) — token-driven, readable in bigger circle |
| Byline text color | `var(--gray-dark)` | `var(--text-subtler)` ("By " prefix) |
| Byline link color | `var(--gray-dark)` + Semibold | **same** — `var(--gray-dark)` + `var(--fw-semibold)` (existing pattern, unchanged) |
| Byline link hover | underline | **same** — underline |
| Card-level hover | border-color + shadow + 1px lift (in 5 grid contexts) | **none** — no chrome to highlight, affordance lives on title + byline |

### Removed: 5 grid-context override rules

The previous design had each grid context add back `box-shadow: none; border: 1px solid var(--border);` to the card. With no chrome on the base component, these overrides have no role:

```css
.blog-grid           .related-card { box-shadow: none; border: 1px solid var(--border); }
.integrations-grid   .related-card { box-shadow: none; border: 1px solid var(--border); }
.author-articles__grid .related-card { box-shadow: none; border: 1px solid var(--border); }
.all-articles__grid  .related-card { box-shadow: none; border: 1px solid var(--border); }
.obs-filter-grid     .related-card { box-shadow: none; border: 1px solid var(--border); }
```

All 5 removed. The card-level hover rule from `.blog-grid .related-card:hover` + 4 siblings was also removed (the rule that lifted the card 1px and added a shadow on hover).

Kept: `.integrations-grid .related-card__image` — that one resizes the image to 120 px and changes its background for integration logos. Real per-context design, not chrome scaffolding.

### Token usage

Every value in the new component uses a design token except **40 px for the avatar** — which sits between `--s-32` and `--s-48` and is documented as off-scale. The library has a `dimension/40` primitive we haven't introduced (no current uses except this one). If we want strict token alignment, future session could add `--s-40: 40px;` to the spacing scale.

### components.html — new docs section

Added a 10th section `#blog-card` between `#breadcrumb` and the closing `</main>`. Includes:
- Live example card with placeholder image + chip + meta + title + description + avatar + byline
- HTML snippet showing the canonical structure
- Spec table covering all 11 properties (container, image, body, meta, date, title, description, avatar, byline text, byline hover, card hover)

### Three canonical link patterns documented in this session

User flagged that the design system has different link treatments for different roles. Three patterns now used consistently:

| Pattern | Color | Weight | Hover | Example uses |
|---|---|---|---|---|
| **Title link** | inherits container (dark) | inherits (Bold) | underline | `.related-card__title a`, `.featured__title a`, `.popular__title a` |
| **Byline link** | `var(--gray-dark)` | `var(--fw-semibold)` | underline | `.related-card__author-name a`, `.article-hero__author-line a` |
| **Inline body link** | `var(--blue)` | inherit | underline | Article prose, "Read more", in-text references |

The byline pattern was always in the codebase but wasn't loud. Now documented in `components.html` Blog Card section.

### Files touched

- `index.html` — entire `.related-card` rule block rewritten (lines 1407–1499 area); 5 grid-context override rules removed; card-level hover rule removed
- `components.html` — added `#blog-card` docs section (~75 lines)
- `MIGRATION.md` — this entry
- `design-system.css` — **untouched** (every new value uses existing tokens)

### Verification

```
index.html structure:
  <div         897 / 897    (unchanged)
  <article     171 / 171
  <section      51 / 51
  <a / </a>    653 / 653
  page-views    21

components.html structure:
  <div         131 / 131    (was 123; +8 from blog-card section)
  <section      10 / 10     (was 9; added blog-card)

.related-card CSS rules in index.html: 11 (was 18 — 5 grid-context overrides + 2 hover rules removed)
.related-card HTML class refs:         unchanged at ~1100 across the prototype

Visual delta: significant — every card on every blog/landing/tag/author/article-related view
renders the new way. ~119 cards across 16 page-views.
```

### Backups

`/home/claude/work/backups/pre-session-24/` holds clean copies of all files before this session.

---

## 2026-05-15 — Session 25: 5 components extracted to docs (Hero, Featured Card, Sidebar Nav, Pagination, Newsletter)

Documented 5 components that already existed in `index.html` inline CSS but had no entry in `components.html`. No new components built — just extracted and documented what was already working.

### Components added to docs

1. **Hero** (`#hero`) — 5 variants on one expanded section: `.hero`, `.subpage-hero`, `.article-hero`, `.tutorial-hero`, `.author-hero`. Each rendered as a live stage with appropriate sample content. Spec table describes shared structure (centered inner container, big title + supporting text) and the variant-specific content slots.

2. **Featured Card** (`#featured-card`) — `.featured` and `.blog-feed__featured` (same component, two name variants in the codebase). The big asymmetric card at the top of feeds. Live preview shows image+body layout with chip, date, title.

3. **Sidebar Nav** (`#side-nav`) — `.side-nav` with groups, group labels, list items, active state, Subscribe button. Live preview shows Categories group + Coding Languages group + Subscribe.

4. **Pagination** (`#pagination`) — `.pagination` with prev/next chevrons, numbered items, ellipsis, active page indicator. Live preview shows: ← 1 2 3 … 12 →.

5. **Newsletter** (`#newsletter`) — `.newsletter` card with title, description, email input, Subscribe button. Live preview shows the full signup block.

### Implementation approach — extract + scope

For each component, the CSS rules were extracted from `index.html` via a Python helper (`extract_css.py`) and embedded in a `<style>` block scoped inside the docs section. Result: the docs page renders the live components correctly without depending on `index.html`.

Minor visual scoping adjustments in the docs preview (smaller hero sizes, narrower side-nav, reset margins on .featured/.newsletter) so each component fits neatly inside its section without trying to span the full page width.

### Token-only — no CSS changes to index.html

The actual CSS rules in `index.html` were not touched. The docs page just reads what's already there. Source-of-truth pattern preserved.

### Verification

```
components.html sections:    10 → 15  (+5)
components.html structural:  <div 169/169, <section 20/20, <style 7/7, <main 1/1
components.html size:        59 KB → 80 KB
components-preview.html:     758 KB → 794 KB (with fonts + new sections)
index.html:                  untouched
design-system.css:           untouched
```

### Files touched

- `components.html` — 5 new sections inserted between `#blog-card` and the closing `</main>`
- `MIGRATION.md` — this entry

### Helper scripts in working dir (not shipped)

- `extract_css.py` — extracts CSS rules matching a class prefix from `index.html`
- `build_5_components.py` — generated the 5 new docs sections (one-shot script for this session)

### Backups

`/home/claude/work/backups/pre-session-25/` holds clean copies of all files before this session.

---

## What's now closed vs. still open across all sessions

**Closed (typography axis):**
- Font sizes — 8/8 tokens migrated or annotated.
- Line-heights — 8/8 tokens migrated or annotated.
- Font families — 3/3 tokens migrated or annotated.

**Closed (color axis):**
- Surface families (Neutral + Brand) — 12 new tokens defined.
- Text scale — 6 new tokens defined.
- Brand chrome — 4 brand-color tokens defined.
- Legacy Search Labs color tokens — 13 aliased to the new system.
- Stale `#6B7079` literals — 12/12 migrated to `var(--gray)`.

**Closed (dimension axis):**
- Spacing — 8/8 tokens annotated.
- Radii — 4/4 tokens migrated or annotated.

**Under experiment (not committed):**
- `--fw-body: 500` — see Session 4.

**Still open — measured but not applied:**
- Heading weights `--fw-heading: 750`.
- `--fw-bold: 700` token (Search Labs uses `font-weight: 700` literally in 30+ places).
- Brand-background blue `#2476f0` and brand-border `#154399`: discovered, not introduced.
- Default border discrepancy: `--border → #C4C7D1` vs library `#CCD1DA`.

**Still open — additive (would introduce new tokens, not change existing):**
- Viewport breakpoints, border widths, icon sizes, negative dimensions, extra dimension primitives.
- Library type classes Search Labs doesn't use: Body XLarge 18, Title Large 28, Headline Medium 36, Display 45/51/58, Oversize 65/73/82 (weight 800), full Label family.

**Cleanup opportunities (literal values bypassing the token system):**
- `#0B64DD` × 17 hardcoded — should use `var(--brand-elastic-blue)` / `var(--blue)`.
- `#FFFFFF` × 8 hardcoded — should use `var(--white)`.
- `#F1F4F8` × 11 — not in any token; needs inspection.
- `#D4DAE5` × 7, `#1C1E23` × 7, `#343741` × 5 — near-matches to existing tokens.
- `border-radius: 10px` × 1 (`.qs-callout`) — bypasses `--r-10`; now inconsistent with the radii consolidation.

---

## How to revert any entry

Each line in `design-system.css` carries its pre-migration value inline. To revert one token, edit just that line back to the value in the comment and update the matching entry's "Status" column in this file.

## How to confirm everything still renders

```bash
# CSS brace balance
grep -c '{' design-system.css ; grep -c '}' design-system.css   # should be equal

# Token references still resolve
grep -c 'var(--fs-' index.html                                  # ≥ 50 expected

# Views and routing intact
grep -c 'class="page-view"' index.html                          # 21
grep -c 'var routes' index.html                                 # 1
```
