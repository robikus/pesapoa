---
name: mitumba-style
description: Use whenever creating or editing an HTML template (backend/templates/*.html) or CSS (backend/static/css/style.css) in the Mitumba Shop Manager project so it matches its existing look — same CSS variables, logo, fonts, cards, buttons, and component patterns. Load before writing any new page or component in that project. Not applicable to this repo (pesapoa) — see pesapoa-style for this project's look.
---

# Mitumba Shop Manager visual style

Single source of truth for the look of every page in the Mitumba Shop
Manager project. Copy the `:root` block and reused classes verbatim from
`backend/static/css/style.css` — do not invent new colors, fonts, radii, or
shadow values. This is a desktop-and-mobile business/ERP app (purchases,
sales, finance, stock) for a single shop owner — calm, professional,
data-dense, not playful or game-like. Plain Arial, white cards on a light
grey page, a forest-green + gold brand palette.

## Core tokens (already in `style.css` — reuse, don't redefine)

```css
:root{
  --g9:#1a3a2a;--g8:#1e4d35;--g7:#2d6a4f;--g6:#40916c;--g5:#52b788;
  --g4:#74c69d;--g2:#d8f3dc;--g1:#f0faf3;
  --gold:#e9c46a;--goldd:#c49a3c;--red:#e63946;--orange:#f4a261;
  --w:#fefefe;--gr0:#f8f9fa;--gr1:#f1f3f5;--gr2:#e9ecef;
  --gr4:#adb5bd;--gr6:#6c757d;--gr8:#343a40;
  --sh:0 2px 12px rgba(0,0,0,.08);--shl:0 8px 32px rgba(0,0,0,.12);
  --r:14px;--rs:8px;
}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:Arial,sans-serif;background:var(--gr0);color:var(--gr8);min-height:100vh;font-size:15px}
```

**Color roles** (never repurpose):
- `--g9`/`--g8`/`--g7` — darkest→medium green; `--g8` is the header bar and
  primary heading/value color, `--g7` is hover/darker states.
- `--g6` — primary brand action color (buttons, active nav underline, links).
- `--g5` — focus border, "in stock / OK" progress states.
- `--g4`/`--g2`/`--g1` — pale greens for tints: `--g1` is the standard soft
  card/section background (money box, table header bg), `--g2` is a badge fill.
- `--gold`/`--goldd` — accent gold: the logo's trailing "." and positive/
  highlight numbers.
- `--red` — negative amounts, delete actions, error banners, "out of stock."
- `--orange` — warning accent (loan-remaining tile border, low-stock bar).
- `--w` — card/surface background (white).
- `--gr0`/`--gr1`/`--gr2` — light greys: page background, input/table-stripe
  background, borders.
- `--gr4`/`--gr6`/`--gr8` — text greys: placeholder/disabled, secondary
  copy, primary body text.
- `--sh`/`--shl` — shadow tokens: `--sh` for ordinary cards, `--shl` for
  hero banners/modals.
- `--r`/`--rs` — border-radius tokens: `--r` (14px) for cards/boxes, `--rs`
  (8px) for inputs/buttons/small chips.

No other hex values — if a new state is needed, reuse the closest existing
token rather than adding one, unless the user asks for a new color explicitly.

## Typography

- **Font stack: `Arial, sans-serif` everywhere — no Google Fonts, no
  webfonts.** This was deliberately enforced project-wide (the register page
  used to load Syne/DM Sans from Google Fonts and looked inconsistent with
  the sign-in page; it was removed specifically to match). Never add a
  `<link>` to a font service or a second `font-family` anywhere.
- Body text: 14–16px range; `color:var(--gr6)` for secondary/descriptive
  copy, `var(--gr8)` for primary/emphasized text.
- Card titles (`.ctitle`, `.stitle`) are **not bold** — `font-weight:400`,
  small (`1rem`), `color:var(--gr6)`, laid out `display:flex;align-items:
  center;gap:8px` with a leading emoji (see Icons below).
- Stat values (`.sval`) are bold and larger — `font-weight:700`,
  `font-size:1.35rem`, `color:var(--g8)`.
- Stat labels (`.slabel`) are small caps — `font-size:.74rem;font-weight:
  700;text-transform:uppercase;letter-spacing:.5px;color:var(--g8)`.

## Logo lockup

```html
<div class="auth-logo">mitumba<span>.</span></div>
```
```css
.auth-logo{font-family:Arial,sans-serif;font-weight:800;font-size:1.6rem;color:var(--g8);text-align:center;letter-spacing:-.5px}
.auth-logo span{color:var(--gold)}
```
- "mitumba" in dark green, trailing "." in gold. No stroke/outline effect
  (unlike a game-style logo — this is a clean wordmark).
- Compact header version (`.htitle`, white text since it sits on the dark
  green header bar) at `font-size:1.15rem`, same `span{color:var(--gold)}`
  treatment, plus a small uppercase tagline beside it:
  ```html
  <div class="htitle">mitumba<span>.</span> <span class="hsubtitle">manage your shop better</span></div>
  ```
  ```css
  .hsubtitle{font-size:.75rem;font-weight:400;opacity:.7;letter-spacing:1px;text-transform:uppercase;color:var(--w)}
  ```
- On auth pages (sign-in/register), the same tagline appears sentence-case,
  centered, below the logo: `Manage your shop better` —
  `.auth-subtitle{text-align:center;color:var(--gr6);font-size:.85rem;margin-bottom:28px}`.
- Logo image (`img/logo.png`) is shown above the wordmark on auth pages at
  `height:72px`, centered.

## Layout shell

```html
<div id="app">
  <header>...</header>
  <nav>...</nav>
  <main>...</main>
</div>
```
```css
#app{display:flex;flex-direction:column;min-height:100vh}
header{background:var(--g8);padding:0 16px;display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:100;height:56px;box-shadow:0 2px 8px rgba(0,0,0,.2)}
main{flex:1;padding:16px;max-width:800px;margin:0 auto;width:100%}
```
Single centered column, **max-width 800px** (wider than a mobile-game shell
— this is a data-dense business app with tables), mobile-first but not
constrained to phone widths.

## Nav (tab bar)

```css
nav{background:var(--w);display:flex;overflow-x:auto;border-bottom:2px solid var(--gr1);position:sticky;top:56px;z-index:99}
.ntab{padding:14px 16px;border:none;background:none;font-size:.83rem;font-weight:500;color:var(--gr6);border-bottom:2px solid transparent}
.ntab.on{color:var(--g7);border-bottom-color:var(--g6);font-weight:600}
```
Sticky, horizontally scrollable on narrow screens, active tab gets a green
underline + darker green text (no background fill).

## Cards

```css
.card{background:var(--w);border-radius:var(--r);padding:20px;box-shadow:var(--sh);margin-bottom:16px}
.ctitle{font-family:Arial,sans-serif;font-size:1rem;font-weight:400;color:var(--gr6);margin-bottom:16px;display:flex;align-items:center;gap:8px}
```
Every card title is **emoji + Title Case label** (e.g. `🏆 Top selling
category`, `🏪 Current Stock Levels`, `🏦 Loan Status`) — pick one emoji
that reads clearly at 1rem, don't use multiple or decorative ones.

## Stat tiles

```css
.sgrid{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:16px}
.sbox{background:var(--w);border-radius:var(--r);padding:16px;box-shadow:var(--sh);border-left:4px solid var(--g5)}
.sbox.red{border-left-color:var(--red)}
.sbox.gold{border-left-color:var(--gold)}
.sbox.ora{border-left-color:var(--orange)}
```
2-column grid of white tiles, distinguished only by a 4px colored left
border: green (default/positive), red (cost), gold (stock/value), orange
(warning/loan). A `.dim{opacity:.45}` utility class greys out a tile/section
when its data isn't applicable (e.g. viewing a past month for a live-only
stock figure) — prefer dimming over hiding, so layout doesn't jump.

## Hero / highlight boxes

Three distinct patterns are in active use on the Dashboard — don't blend them:

- **`.pdraw-shop`** — plain white card, `display:flex;justify-content:
  space-between;align-items:center`, bold dark shop name + grey subtitle on
  the left, a small (52px) rounded icon badge on the right. The badge itself
  has **no colored fill** (`background:transparent`) — just a simple
  Feather-style line icon (`fill:none;stroke:currentColor;stroke-width:2`)
  colored `var(--g6)`. Prefer well-known simple icons (house/shop/market)
  over decorative ones.
- **`.pdraw-money`** — pale green (`var(--g1)`) background, centered text,
  small uppercase label, then a large (2.4rem, bold) amount colored
  `var(--g7)` if positive or `var(--red)` if negative.
- **`.pdraw`** (used inside finance-style cards) — dark green gradient
  (`linear-gradient(135deg,var(--g8),var(--g6))`), centered, big **gold**
  number — reserved for the one legacy banner style, don't reuse for new
  boxes; prefer `.pdraw-money` for new "big number" displays.

## Buttons

```css
.btn{padding:12px 20px;border:none;border-radius:var(--rs);font-weight:700;font-size:.88rem;display:inline-flex;align-items:center;gap:6px;transition:all .2s}
.bp{background:var(--g6);color:var(--w)}
.bp:hover{background:var(--g7);transform:translateY(-1px)}
.bs{background:var(--gr1);color:var(--gr8)}
.bd{background:var(--red);color:var(--w)}
.bsm{padding:6px 12px;font-size:.78rem}
.bfull{width:100%;justify-content:center}
```
Hover is a **subtle 1px lift + darken**, not a 3D pressed/bottom-shadow
effect — keep buttons flat and understated, matching the business-app tone.

## Forms

```css
.fg{margin-bottom:14px}
.fl{font-size:.8rem;color:var(--gr6);margin-bottom:6px}
.fi,.fs{padding:11px 14px;border:1.5px solid var(--gr2);border-radius:var(--rs);font-size:.95rem;background:var(--w);color:var(--gr8)}
.fi:focus,.fs:focus{border-color:var(--g5)}
.fi.err,.fs.err{border-color:var(--red) !important;background:#fff5f5}
```

## Tables

```css
th{background:var(--g1);color:var(--g8);font-size:.76rem;text-transform:uppercase;letter-spacing:.5px}
td{padding:10px 12px;border-bottom:1px solid var(--gr1);font-size:.87rem}
tr:hover td{background:var(--g1)}
```

## Badges

```css
.badge{padding:3px 9px;border-radius:20px;font-size:.73rem;font-weight:600}
.bg{background:var(--g2);color:var(--g8)}      /* good / in-stock / B2C */
.bb{background:#dbeafe;color:#1e40af}          /* info / B2B */
.br{background:#fee2e2;color:#991b1b}          /* bad / out of stock */
.bo{background:#fef3c7;color:#92400e}          /* warning / low stock */
```

## Auth pages (sign-in / register)

```css
.auth-wrap{min-height:100vh;display:flex;flex-direction:column;align-items:center;justify-content:center;background:var(--gr0);padding:24px 16px}
.auth-card{background:var(--g1);border-radius:var(--r);box-shadow:var(--shl);padding:36px 32px;width:100%;max-width:380px}
```
Centered single card, `max-width:380px`, pale-green background (not white —
this is the one place `.card`-equivalent surfaces use `--g1` instead of
`--w`). Sign-in and Register must look identical in structure (logo image,
wordmark, subtitle, form, divider, cross-link) — when adding a new auth
page, copy this shell rather than styling from scratch.

## Month selector (chips)

```css
.mbar{display:flex;gap:6px;overflow-x:auto;justify-content:center}
.mchip{padding:6px 14px;border-radius:20px;background:var(--gr1);color:var(--gr6);font-size:.82rem;font-weight:500}
.mchip.on{background:var(--gr6);color:var(--w)}
.mchip.today:not(.on){color:var(--red)}
```
Selected chip uses a neutral grey fill (`--gr6`), not brand green — green is
reserved for primary actions/buttons. The chip matching the real current
month is always red text, *unless* it's also the selected chip (then it
follows the normal selected style).

## Toast

```css
.toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%);background:var(--g8);color:var(--w);padding:11px 22px;border-radius:24px;font-size:.87rem;box-shadow:var(--shl);opacity:0;pointer-events:none;transition:opacity .3s,transform .3s}
.toast.on{opacity:1}
```

## Icons

Emoji-first for card titles and inline labels (🏆 🏪 💰 🏦 📦 🏠 👤 🏛️ 💸
⚙️) — one emoji per label, no combinations. The one exception is the shop
icon badge on `.pdraw-shop` (see above), which uses a simple inline SVG
line-icon instead of an emoji, because it needs to render in a single brand
color rather than emoji's fixed multi-color glyph.

## Rendering pattern

**Server-rendered Django templates**, not a client-side `render()` SPA
pattern — `backend/templates/index.html` is a single static HTML shell for
all authenticated pages (`.pg` divs toggled by `nav()` in `app.js`), and
`app.js` populates specific element IDs via `fetch()` calls to the Django
REST API (`/api/...`). New content goes into a real `<div id="...">` in the
template, populated by a render function that sets `.textContent`/
`.innerHTML` on that ID — not a full-page re-render of a template literal.
There is no client-side language switching or `localStorage`-driven i18n —
this app is English-only, single-tenant-per-user.

## Rules when building a new page or component

1. Start from the `:root` tokens in `backend/static/css/style.css`,
   unchanged — never add a new hex color or a second font-family.
2. Reuse existing classes (`.card`, `.sbox`, `.btn`, `.badge`, `.auth-card`,
   `.pdraw-money`) instead of inventing new ones when the pattern already fits.
3. Card titles: emoji + `font-weight:400` grey label, never bold headings.
4. Buttons: flat with a subtle hover-lift — no 3D press-shadow, no bounce.
5. Keep the tone calm/professional — this is a shop-owner's bookkeeping
   tool, not a game; avoid playful copy, badges-as-rewards, or animation
   beyond simple `.2s`–`.4s` transitions.
6. New CSS goes in `backend/static/css/style.css`; new markup goes in the
   relevant `backend/templates/*.html` file — everything still ships as
   plain CSS/JS with no build step or bundler.
