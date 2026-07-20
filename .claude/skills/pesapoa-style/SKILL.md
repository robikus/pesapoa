---
name: pesapoa-style
description: Use whenever creating or editing an HTML page in this project (index.html, apps/*.html) so it matches the existing Pesa Poa look — same CSS variables, logo, fonts, cards, buttons, and component patterns. Load before writing any new page or component.
---

# Pesa Poa visual style

Single source of truth for the look of every page in this project. Copy the
`:root` block and reused classes verbatim — do not invent new colors, fonts,
radii, or shadow values. This is a mobile-first, offline, bilingual (en/sw)
site for Kenyan secondary students, styled like a friendly, game-like app —
warm paper background, bold rounded cards, "pressable" buttons with a 3D
bottom shadow.

## Core tokens (put this exact block in every page's `<style>`)

```css
:root{
  --paper:#FFF7EA; --card:#FFFFFF; --ink:#16261C; --ink-soft:#4C5A50; --line:#E7DECB;
  --green:#0E9E5B; --green-deep:#0A6F41; --green-tint:#E4F6EC;
  --coral:#E24A33; --coral-tint:#FBE6E1; --yellow:#FFC22E; --teal:#0E7C86;
  --shadow:0 6px 0 rgba(20,38,28,.12);
}
*{box-sizing:border-box}
html,body{margin:0;padding:0}
body{font-family:system-ui,-apple-system,"Segoe UI",Roboto,Arial,sans-serif;background:var(--paper);color:var(--ink);-webkit-text-size-adjust:100%;line-height:1.45}
.wrap{max-width:560px;margin:0 auto;min-height:100dvh;display:flex;flex-direction:column}
```

**Color roles** (never repurpose):
- `--paper` — page background (warm cream)
- `--card` — card/panel background (white)
- `--ink` / `--ink-soft` — primary / secondary text
- `--line` — card borders and dividers
- `--green` / `--green-deep` — primary brand action color; `-deep` is for header/HUD bg and shadow of pressable elements
- `--green-tint` — soft success/positive background fill
- `--coral` / `--coral-tint` — negative/spend/warning
- `--teal` — accent for eyebrows, links, focus outline
- `--yellow` — highlight accent (logo's "Poa", badges, points)

No other hex values. If a new state is needed (e.g. an "info" color), reuse
`--teal`/`--yellow` rather than adding a new variable, unless the user asks
for a new color explicitly.

## Typography

- Font stack: `system-ui,-apple-system,"Segoe UI",Roboto,Arial,sans-serif` — no webfonts, no Google Fonts (site must work fully offline).
- Body text: 14–16px, `color:var(--ink-soft)` for descriptive copy, `var(--ink)` for emphasized/primary text.
- Headings inside cards (`h2`/`h3`): bold, tight `line-height:1.25`, no letter-spacing.
- Eyebrows (small section labels above content): `font-size:12px;font-weight:700;letter-spacing:.08em;text-transform:uppercase;color:var(--teal)`.
- Numbers/money/scores: add `font-variant-numeric:tabular-nums` so digits don't jitter.

## Logo lockup

```html
<p class="logo">Pesa <span>Poa</span></p>
```
```css
.logo{font-size:40px;font-weight:900;letter-spacing:-.02em;color:var(--green-deep);margin:0}
.logo span{color:var(--yellow);-webkit-text-stroke:1px var(--green-deep)}
```
- "Pesa" in deep green, "Poa" in yellow with a thin green outline stroke.
- Compact header version uses the same treatment at `font-size:15px;font-weight:900` inside `.hud-brand`, with `span`/`b` colored `var(--yellow)` (no stroke needed at that size).
- Tagline directly under the logo: `.tagline{color:var(--ink-soft);font-size:16px;margin:8px 0 0}`.

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

Used on game pages to show brand + language toggle, optionally stats and a progress bar.

```css
.hud{position:sticky;top:0;z-index:10;background:var(--green-deep);color:#fff;padding:10px 12px;box-shadow:0 3px 10px rgba(0,0,0,.18)}
.langbtn{background:rgba(255,255,255,.14);border:1px solid rgba(255,255,255,.5);color:#fff;border-radius:999px;font:inherit;font-weight:800;font-size:11px;letter-spacing:.04em;padding:5px 11px;cursor:pointer}
.langbtn:active{transform:translateY(1px)}
```
For pages with running totals (money, debt, etc.), add a stats row and a thin progress bar under the HUD (`.hud-stats` grid of 3, `.progress`/`.progress-fill` with `--yellow` fill) — see `apps/household.html` for the full pattern.

## Cards

Default content card (rounded, bordered, "pressed paper" shadow):

```css
.card-like{background:var(--card);border:2px solid var(--line);border-radius:18px;padding:18px;box-shadow:var(--shadow)}
```
Used as `.lede`, `.scene`, `.recap` etc. — always `border-radius:16–18px`, always `border:2px solid var(--line)`, always `box-shadow:var(--shadow)` (never a blurred drop shadow).

Smaller nested rows/list items use `border-radius:12–16px` with the same border color, no shadow (e.g. `.game`, `.opt`).

## Buttons

Primary "pressable" button — 3D bottom-shadow that flattens on press:

```css
.btn{display:block;width:100%;margin-top:16px;cursor:pointer;background:var(--green);color:#fff;text-decoration:none;text-align:center;border:none;border-radius:14px;padding:15px;font:inherit;font-weight:800;font-size:16px;box-shadow:0 5px 0 var(--green-deep);transition:transform .08s ease}
.btn:active{transform:translateY(4px);box-shadow:0 1px 0 var(--green-deep)}
.btn:focus-visible{outline:3px solid var(--teal);outline-offset:3px}
```
Ghost/secondary variant: white background, green border, tint-colored shadow:
```css
.btn.ghost{background:#fff;color:var(--green-deep);border:2px solid var(--green);box-shadow:0 5px 0 var(--green-tint)}
```
Inline/smaller CTA (e.g. "Play →" on a card): same pressed-shadow idea at smaller scale, use `.play` class from `index.html`.

Every clickable primary element must have `:active{transform:translateY(Npx); box-shadow:...}` to keep the "pressed" feel, and a `:focus-visible` outline in `var(--teal)` for keyboard accessibility.

## Status/feedback colors

- Positive/good outcome: background `var(--green-tint)`, border `var(--green)`.
- Negative/bad outcome: background `var(--coral-tint)`, border `var(--coral)`.
- Neutral: background `#FFF6DC`, border `var(--yellow)`.
- Tag chips for spend/earn/save/points use solid fills: coral (spend), green (earn), teal (save), yellow (points, dark text `#3a2c00`).

## Motion

- Only `transform` and simple `width`/opacity transitions, always fast (`.08s`–`.4s ease`).
- Any entrance animation must be wrapped with `@media (prefers-reduced-motion: reduce){ ... {animation:none} }`.

## Bilingual content pattern (en/sw)

Every page keeps copy in a single `L` object keyed by language, not scattered in HTML:

```js
const L = { en:{ ... }, sw:{ ... } };
let LANG = "en";
function loadLang(){ try{ const v=localStorage.getItem("pesa_lang"); if(v==="en"||v==="sw") LANG=v; }catch(e){} }
function saveLang(){ try{ localStorage.setItem("pesa_lang",LANG); }catch(e){} }
function t(k){ return L[LANG][k]; }
```
The language toggle button swaps `LANG`, saves it, and calls `render()` again. New pages must follow this exact pattern (same localStorage key `pesa_lang`) so the language choice persists across pages on the site.

## Rendering pattern

Pages render into a single `#app` div via a template-literal `render()` function called on load (`loadLang(); render();`), not static server-rendered HTML with inline text — this keeps all copy centralized in `L` for translation and makes toggling language a simple re-render.

## Rules when building a new page

1. Start from the `:root` token block above, unchanged.
2. Reuse existing class names/patterns (`.wrap`, `.stage`, `.hud`, card styles, `.btn`, `.tag`, `.verdict`) instead of inventing new ones when the pattern already fits.
3. Keep everything inline in one `<style>` + one `<script>` in a single `.html` file — no external CSS/JS/font files (site must work fully offline on a shared school computer).
4. New copy must be added to both `en` and `sw` in the `L` object — never hardcode English strings in the template.
5. Max width stays 560px; this is a mobile-first single-column app, not a responsive multi-column site.
