---
name: pesapoa-style
description: Use whenever creating or editing an HTML page in this project (index.html, apps/*.html) so it matches the existing Pesa Poa look — same CSS variables, logo, fonts, cards, buttons, and component patterns. Load before writing any new page or component.
---

# Pesa Poa visual style

Single source of truth for the look of every page in this project. Copy the
`:root` block and reused classes verbatim — do not invent new colors, fonts,
radii, or shadow values. This is a mobile-first, offline, bilingual (en/sw)
site for Kenyan secondary students. The visual language was migrated from a
playful game-app look to a calmer, more professional "business tool" look
(adapted from a sister project's design system) — clean white cards on a
light grey page, flat buttons with a subtle hover-lift, Arial throughout, a
forest-green + gold brand palette. Copy still reads warm and encouraging;
the chrome should read calm and trustworthy, not bouncy or arcade-like.

## Core tokens (put this exact block in every page's `<style>`)

```css
:root{
  --paper:#F8F9FA; --card:#FEFEFE; --ink:#343A40; --ink-soft:#6C757D; --line:#E9ECEF;
  --green:#40916C; --green-deep:#1E4D35; --green-tint:#D8F3DC;
  --coral:#E63946; --coral-tint:#FEE2E2; --yellow:#E9C46A; --teal:#1E40AF;
  --shadow:0 2px 12px rgba(0,0,0,.08);
  --r:14px; --rs:8px;
}
*{box-sizing:border-box}
html,body{margin:0;padding:0}
body{font-family:Arial,sans-serif;background:var(--paper);color:var(--ink);-webkit-text-size-adjust:100%;line-height:1.45}
.wrap{max-width:560px;margin:0 auto;min-height:100dvh;display:flex;flex-direction:column}
```

**Color roles** (never repurpose):
- `--paper` — page background (light grey)
- `--card` — card/panel background (near-white)
- `--ink` / `--ink-soft` — primary / secondary text
- `--line` — card borders and dividers (thin, 1px — not the old thick 2px game-card border)
- `--green` / `--green-deep` — primary brand action color; `-deep` is for header/HUD bg and darker headings
- `--green-tint` — soft success/positive/stat-tile background fill
- `--coral` / `--coral-tint` — negative/spend/warning
- `--teal` — **note:** this is now Mitumba's info-blue (`#1E40AF`), not a green-family teal. It's the accent used for eyebrows, focus rings, and — on `apps/savings.html` specifically — the entire page's chrome (HUD, logo, buttons), so that Savings keeps a visually distinct identity from Household's green theme. Keep `index.html`'s `--teal` value identical to `savings.html`'s, since `index.html`'s `.dot.c-teal` previews Savings' color on the homepage game list.
- `--yellow` — highlight accent (logo's "Poa", badges, points) — actually a muted gold (`#E9C46A`), not a bright yellow
- `--r` — card/box border-radius (14px)
- `--rs` — button/input border-radius (8px)

No other hex values, except the small set of badge-specific pastel pairs
documented under Tags/badges below (mirroring how the reference design
system also allows one-off badge colors beyond its root tokens).

`apps/savings.html` additionally defines `--teal-deep:#1E3A8A` (a darker
blue, parallel to how `--green-deep` relates to `--green`) for its HUD
background and biggest numbers/logo.

## Typography

- Font stack: **`Arial, sans-serif`** only — no webfonts, no Google Fonts,
  no system-ui stack (site must work fully offline, and Arial was
  deliberately chosen as the one consistent font across the whole product
  line). Never add a second `font-family` or a `<link>` to a font service.
- Body text: 14–16px, `color:var(--ink-soft)` for descriptive copy,
  `var(--ink)` for emphasized/primary text.
- Headings inside cards (`h2`/`h3`): `font-weight:700`, tight
  `line-height:1.25`, no letter-spacing.
- Eyebrows (small section labels above content): `font-size:12px;font-weight:700;letter-spacing:.08em;text-transform:uppercase;color:var(--teal)`.
- Numbers/money/scores: add `font-variant-numeric:tabular-nums` so digits
  don't jitter. Big numeric displays (`.tool-big`, `.end-score`) are
  `font-weight:700` (not 900 — the old style used near-black 900 weight
  everywhere; the new look uses 700 max, reserving 900 only for `.hud-brand`
  wordmark and `.logo`... actually `.logo` is now `font-weight:800`, and only
  `.hud-brand`/`.module` stay at 900 for the compact header wordmark).

## Logo lockup

```html
<p class="logo">Pesa <span>Poa</span></p>
```
```css
.logo{font-size:40px;font-weight:800;letter-spacing:-.02em;color:var(--green-deep);margin:0}
.logo span{color:var(--yellow)}
```
- "Pesa" in deep green, "Poa" in gold. **No text-stroke/outline effect** —
  that was a game-style flourish that's been dropped in favor of a clean
  wordmark, matching the calmer visual language.
- Compact header version keeps `font-size:15px;font-weight:900` inside
  `.hud-brand`, with `span`/`b` colored `var(--yellow)`.
- Tagline directly under the logo: `.tagline{color:var(--ink-soft);font-size:16px;margin:8px 0 0}`.
- On `apps/savings.html`, the logo/HUD color is `var(--teal-deep)` /
  `var(--teal)` instead of green, per its distinct module identity above.

## Layout shell

Every page is a single centered column, max 560px wide, full viewport height:

```html
<div class="wrap" id="app"> ... </div>
```
```css
.wrap{max-width:560px;margin:0 auto;min-height:100dvh;display:flex;flex-direction:column}
.stage{flex:1;padding:18px 16px 28px;display:flex;flex-direction:column}
```

## HUD (sticky top bar)

```css
.hud{position:sticky;top:0;z-index:10;background:var(--green-deep);color:#fff;padding:10px 12px;box-shadow:0 2px 8px rgba(0,0,0,.2)}
.langbtn{background:rgba(255,255,255,.14);border:1px solid rgba(255,255,255,.5);color:#fff;border-radius:999px;font:inherit;font-weight:700;font-size:11px;letter-spacing:.04em;padding:5px 11px;cursor:pointer;transition:background .15s ease}
.langbtn:hover{background:rgba(255,255,255,.22)}
```
No `:active{transform:...}` press-effect on the lang toggle anymore — hover
darkening only, matching the flatter interaction style.

For pages with running totals (money, debt, etc.), add a stats row and a
thin progress bar under the HUD (`.hud-stats` grid of 3, `.progress`/
`.progress-fill` with `--yellow` fill) — see `apps/household.html`.

## Cards

```css
.card-like{background:var(--card);border:1px solid var(--line);border-radius:var(--r);padding:18px;box-shadow:var(--shadow)}
```
Used as `.lede`, `.scene`, `.recap`, `.stat`, `.verdict` etc. — always
`border-radius:var(--r)` (14px), always a **thin 1px border** (not the old
2px), always `box-shadow:var(--shadow)` — a soft, ordinary drop shadow
(`0 2px 12px rgba(0,0,0,.08)`), **not** the old offset "pressed paper"
shadow (`0 6px 0 ...`). There is no more colored/offset shadow anywhere in
the system.

## Buttons

Buttons are now **flat with a subtle hover-lift** — no 3D press-shadow, no
bounce:

```css
.btn{display:block;width:100%;margin-top:16px;cursor:pointer;background:var(--green);color:#fff;text-decoration:none;text-align:center;border:none;border-radius:var(--rs);padding:15px;font:inherit;font-weight:700;font-size:16px;transition:background .15s ease,transform .15s ease}
.btn:hover{background:var(--green-deep);transform:translateY(-1px)}
.btn:focus-visible{outline:2px solid var(--teal);outline-offset:2px}
```
Ghost/secondary variant: white background, colored border, no shadow:
```css
.btn.ghost{background:#fff;color:var(--green-deep);border:1px solid var(--green)}
```
Inline/smaller CTA (`.play` on `index.html`) follows the same flat +
hover-darken + `translateY(-1px)` pattern at a smaller scale — no
`box-shadow`, no `:active` offset.

Every clickable primary element keeps a `:focus-visible` outline in
`var(--teal)` (or `var(--green)` on the Savings page, to contrast against
its blue chrome) for keyboard accessibility — dropping the press-shadow
didn't mean dropping accessibility affordances.

## Status/feedback colors

- Positive/good outcome: background `var(--green-tint)`, border `var(--green)`.
- Negative/bad outcome: background `var(--coral-tint)`, border `var(--coral)`.
- Neutral: background `#FEF3C7` (soft gold tint), border `var(--yellow)`.

## Tags/badges

Pill-shaped, **pastel background + darker matching text** — not the old
solid-fill-with-white-text look:
```css
.tag{font-size:13px;font-weight:700;padding:5px 10px;border-radius:999px;font-variant-numeric:tabular-nums}
.tag.spend{background:#FEE2E2;color:#991B1B}
.tag.earn{background:#D8F3DC;color:#1E4D35}
.tag.save{background:#DBEAFE;color:#1E40AF}
.tag.pts{background:#FEF3C7;color:#92400E}
```
These four pastel pairs (red, green, blue, gold) are the only badge colors
in the system — reuse them for any new tag/status-chip rather than
introducing a new pair, unless the user asks for a new category.

## Motion

- Only `background`, `transform` (a 1px hover-lift), and simple
  `width`/opacity transitions, always fast (`.15s`–`.4s ease`). No bounce,
  no press-flatten, no scale animations.
- Any entrance animation must be wrapped with
  `@media (prefers-reduced-motion: reduce){ ... {animation:none} }`.

## Bilingual content pattern (en/sw)

Unchanged — this is specific to Pesa Poa's product requirements (Kenyan
secondary students), not something adopted from the reference design
system, so it stays exactly as before:

```js
const L = { en:{ ... }, sw:{ ... } };
let LANG = "en";
function loadLang(){ try{ const v=localStorage.getItem("pesa_lang"); if(v==="en"||v==="sw") LANG=v; }catch(e){} }
function saveLang(){ try{ localStorage.setItem("pesa_lang",LANG); }catch(e){} }
function t(k){ return L[LANG][k]; }
```
The language toggle button swaps `LANG`, saves it, and calls `render()`
again. New pages must follow this exact pattern (same localStorage key
`pesa_lang`) so the language choice persists across pages on the site.

## Rendering pattern

Also unchanged — pages render into a single `#app` div via a
template-literal `render()` function called on load (`loadLang(); render();`),
keeping all copy centralized in `L` for translation.

## Rules when building a new page

1. Start from the `:root` token block above, unchanged.
2. Reuse existing class names/patterns (`.wrap`, `.stage`, `.hud`, card
   styles, `.btn`, `.tag`, `.verdict`) instead of inventing new ones when the
   pattern already fits.
3. Keep everything inline in one `<style>` + one `<script>` in a single
   `.html` file — no external CSS/JS/font files (site must work fully
   offline on a shared school computer).
4. New copy must be added to both `en` and `sw` in the `L` object — never
   hardcode English strings in the template.
5. Max width stays 560px; this is a mobile-first single-column app, not a
   responsive multi-column site.
6. Buttons/cards stay flat (thin border, soft ordinary shadow, hover-lift) —
   don't reintroduce the old 3D press-shadow or thick 2px borders.
7. Font stays Arial only; radii come from `--r` (cards) / `--rs`
   (buttons/inputs); tags use the four pastel pairs above.
