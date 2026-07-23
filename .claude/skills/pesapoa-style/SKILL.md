---
name: pesapoa-style
description: Use whenever creating or editing an HTML page in this project (index.html, household.html, savings.html) so it matches the existing Pesa Poa look — same CSS variables, logo, fonts, cards, buttons, and component patterns. Load before writing any new page or component.
---

# Pesa Poa visual style

Single source of truth for the look of every page in this project. Copy the
`:root` block and reused classes verbatim — do not invent new colors, fonts,
radii, or shadow values. This is a mobile-first, offline, bilingual (en/sw)
site for Kenyan secondary students. The visual language is calm and clean —
white cards on a light grey page, flat buttons with a subtle hover-lift,
Arial throughout, a forest-green + gold brand palette — with one bolder
gradient accent reserved for a specific moment (the homepage Support
section), not used everywhere.

**File layout:** all three pages sit flat at the repo root —
`index.html`, `household.html`, `savings.html` — plus `logo/` for the
shared logo image and `vercel.json` (`{"cleanUrls": true}`, so
`household.html`/`savings.html` are reachable as `/household`/`/savings`
with no `.html` and no subfolder). There is **no `apps/` folder anymore** —
the games used to live there but were moved to root so clean URLs work
without needing Vercel `rewrites` (those were tried first and silently
didn't apply for this project — cleanUrls + flat file layout is the
solution that actually works). Because everything is flat, every internal
reference is a same-level path: `logo/sunset_trans_mini_2.png` and
`index.html`, never `../something`.

**Important:** `index.html` (the homepage) and `household.html` /
`savings.html` (the games) intentionally use two different header
patterns — a flat, non-sticky "hero" on the homepage vs. a colored sticky
HUD inside games. Both are documented separately below; don't merge them.

## Core tokens (put this exact block in every page's `<style>`)

```css
:root{
  --paper:#F8F9FA; --card:#FEFEFE; --ink:#343A40; --ink-soft:#6C757D; --line:#E9ECEF;
  --green:#40916C; --green-deep:#1E4D35; --green-tint:#D8F3DC;
  --coral:#E63946; --coral-tint:#FEE2E2; --yellow:#E9C46A; --teal:#C2410C;
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
- `--line` — card borders, dividers, and the homepage logo-badge circle fill
- `--green` / `--green-deep` — primary brand action color; `-deep` is for in-game HUD backgrounds, gradients, and darker headings
- `--green-tint` — soft success/positive/stat-tile background fill, and the default game-icon tint
- `--coral` / `--coral-tint` — negative/spend/warning
- `--teal` — a **dark orange** (`#C2410C`), despite the name. It was blue
  earlier, then Mitumba-derived blue, and is now dark orange — the name
  stuck through both changes purely to avoid a site-wide rename. It's used
  for eyebrows/accents, the homepage "Poa" logo accent, and as the entire
  chrome color of `savings.html` (HUD, logo, buttons) so Savings keeps a
  visually distinct identity from Household's green theme. Keep
  `index.html`'s `--teal` value identical to `savings.html`'s, since
  `index.html`'s `.gicon.c-teal`/`.game.c-teal` preview Savings' color on
  the homepage.
- `--yellow` — a muted gold (`#E9C46A`) — badges, points, and the Support button's hover color
- `--r` — card/box border-radius (14px)
- `--rs` — button/input border-radius (8px)

`savings.html` additionally defines `--teal-deep:#9A3412` (a darker orange)
for its HUD background and biggest numbers/logo, parallel to how
`--green-deep` relates to `--green`.

## Typography

- Font stack: **`Arial, sans-serif`** only — no webfonts, no Google Fonts
  (site must work fully offline). Never add a second `font-family`.
- Body text: 14–16px, `var(--ink-soft)` for descriptive copy, `var(--ink)`
  for emphasized/primary text.
- Headings inside cards (`h2`/`h3`): `font-weight:700`, tight
  `line-height:1.25`, no letter-spacing.
- Numbers/money/scores: add `font-variant-numeric:tabular-nums`.

## Logo asset

`logo/sunset_trans_mini_2.png` — a small circular transparent-background
icon (acacia tree at sunset). Reference it as plain `logo/...` from any
page — everything lives flat at the repo root, so there's never a `../`.

## Homepage hero (`index.html` only)

The homepage does **not** use a sticky colored HUD. Instead it has a flat,
page-colored hero block that scrolls away with the rest of the content:

```css
.hero{position:relative;background:var(--paper);padding:44px 20px 32px;text-align:center}
.langbtn{position:absolute;top:16px;right:16px;z-index:2;background:var(--card);border:1px solid var(--green);color:var(--green-deep);border-radius:999px;font:inherit;font-weight:700;font-size:11px;letter-spacing:.04em;padding:6px 12px;cursor:pointer;transition:background .15s ease}
.langbtn:hover{background:var(--green-tint)}
.hero-logo-ring{width:84px;height:84px;margin:0 auto 14px;background:var(--line);border-radius:50%;display:flex;align-items:center;justify-content:center;box-shadow:var(--shadow)}
.hero-logo{width:62px;height:62px;display:block}
.logo{font-size:32px;font-weight:800;letter-spacing:-.02em;color:var(--green-deep);margin:0}
.logo span{color:var(--teal)}
.tagline{color:var(--ink-soft);font-size:16px;margin:10px auto 0;max-width:320px}
```
```html
<div class="hero">
  <button id="lang" class="langbtn">${t("langBtn")}</button>
  <div class="hero-logo-ring"><img class="hero-logo" src="logo/sunset_trans_mini_2.png" alt=""></div>
  <p class="logo">${t("brand")}</p>
  <p class="tagline">${t("tagline")}</p>
</div>
```
Notes:
- The language button is absolutely positioned top-right of the hero, not a
  separate top bar.
- The logo sits inside a plain light-grey circle (`var(--line)` fill) with a
  soft drop shadow — **not** a colored gradient banner, and **not** white
  (a white circle disappears against the near-white page). A full green
  gradient banner with decorative blur shapes and small "Free / Offline /
  EN·SW" feature pills was tried and explicitly removed in favor of this
  flatter look — don't reintroduce them without being asked.
- Homepage brand text is **"Pesa Poa Games"** (en) / **"Pesa Poa Michezo"**
  (sw) — note the suffix. This differs from the in-game HUD, which just
  says "Pesa Poa". The "Poa" span is dark orange (`var(--teal)`), not gold.
- Current tagline (key `tagline` in `L`): "Learn to manage your finance
  better." / "Jifunze kudhibiti pesa yako vizuri zaidi." — grey
  (`var(--ink-soft)`), not colored. Earlier drafts said "Free games to
  manage..." and tried orange text; both were reverted.
- There is no "Choose a game" eyebrow above the game list anymore — the
  hero is immediately followed by `.games`.

## Homepage game cards

Active games are entire clickable cards (bigger tap target, not just an
inner "Play" link); disabled/"coming soon" games stay as plain non-link
`div`s:

```css
.games{display:flex;flex-direction:column;gap:14px}
.game{background:var(--card);border:1px solid var(--line);border-left:4px solid var(--green);border-radius:var(--r);padding:16px;box-shadow:var(--shadow);display:flex;gap:14px;align-items:center;text-decoration:none;color:inherit;transition:transform .15s ease,box-shadow .15s ease}
.game:not(.disabled):hover{transform:translateY(-3px);box-shadow:0 12px 26px rgba(30,77,53,.14)}
.game:not(.disabled):focus-visible{outline:2px solid var(--green);outline-offset:2px}
.game.c-coral{border-left-color:var(--coral)}
.game.c-teal{border-left-color:var(--teal)}
.game.c-yellow{border-left-color:var(--yellow)}
.game.disabled{border-left-color:var(--line);opacity:.65}
.gicon{width:46px;height:46px;border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:22px;flex:none;background:var(--green-tint)}
.gicon.c-coral{background:var(--coral-tint)}
.gicon.c-teal{background:#FFEDD5}
.gicon.c-yellow{background:#FEF3C7}
```
```html
<a class="game" href="/household">
  <span class="gicon">💰</span>
  <div class="game-body">
    <h3>${t("g1t")}</h3>
    <p>${t("g1d")}</p>
    <div class="game-cta"><span class="play">${t("play")} →</span></div>
  </div>
</a>
```
- Links use the **clean paths** `/household` and `/savings` (not
  `household.html` or an `apps/` prefix) so the URL bar stays tidy.
- Each game gets one color role (default green, or `.c-teal`/`.c-coral`/
  `.c-yellow`), applied to **both** the card (`border-left`) and its
  `.gicon` (tint background) — keep the pair in sync. `.c-teal`'s tint is
  now a light orange (`#FFEDD5`), matching `--teal`'s dark-orange value,
  not blue.
- One emoji per game icon, no combinations: 💰 Money Basics, 🏦 Savings, 🤝
  Loans & Borrowing, 🏪 Starting a Business.
- `.play` inside an active card is a plain styled `<span>`, not a nested
  `<a>` — the whole card is already the link.
- Disabled cards keep `<span class="tag-soon">` instead of `.play`, and add
  `.disabled` (dims via opacity, neutralizes the border-left color) on top
  of their color-role class.

## Cards (general)

```css
.card-like{background:var(--card);border:1px solid var(--line);border-radius:var(--r);padding:18px;box-shadow:var(--shadow)}
```
Used as `.lede`, `.scene`, `.recap`, `.stat`, `.verdict`, `.brief` (the
game-start bullet box, see below) etc. — always `border-radius:var(--r)`
(14px), a thin 1px border, and the soft ordinary `var(--shadow)` drop
shadow (no offset "pressed" shadow anywhere).

On the homepage, the `.lede` info box ("Pesa Poa... is a set of free
games...") sits **below** the games list, not above it, and has no
box/border of its own — it's plain text directly on the page background
(the boxed-card version was tried and reverted).

## Support / donation section

The one place on the homepage that still uses a bold gradient — intentional
contrast against the flat hero above it:

```css
.support{margin-top:26px;background:linear-gradient(135deg,var(--green-deep),var(--green));border:none;border-radius:var(--r);padding:24px;box-shadow:var(--shadow)}
.support h3{margin:0 0 8px;font-size:19px;color:#fff;font-weight:700}
.support p{margin:0;font-size:15px;color:rgba(255,255,255,.92)}
.btn{display:block;width:100%;margin-top:16px;cursor:pointer;background:#fff;color:var(--green-deep);text-decoration:none;text-align:center;border:none;border-radius:var(--rs);padding:15px;font:inherit;font-weight:700;font-size:16px;transition:background .15s ease,transform .15s ease}
.btn:hover{background:var(--yellow);transform:translateY(-1px)}
```

## Buttons (general)

Flat with a subtle hover-lift — no 3D press-shadow, no bounce:
```css
.btn:hover{background:var(--green-deep);transform:translateY(-1px)}
.btn:focus-visible{outline:2px solid var(--teal);outline-offset:2px}
```
Ghost/secondary variant: white background, colored border, no shadow.

## Tags/badges

Pill-shaped, pastel background + darker matching text:
```css
.tag.spend{background:#FEE2E2;color:#991B1B}
.tag.earn{background:#D8F3DC;color:#1E4D35}
.tag.save{background:#FFEDD5;color:#C2410C}
.tag.pts{background:#FEF3C7;color:#92400E}
```
These four pastel pairs are the only badge colors in the system. `.tag.save`
is a light-orange pair (matching `--teal`'s dark orange), not blue.

## In-game HUD (`household.html`, `savings.html`)

Unlike the homepage, each game keeps a **sticky, colored, two-row HUD**:

```css
.hud{position:sticky;top:0;z-index:10;background:var(--green-deep);color:#fff;padding:8px 12px 10px;box-shadow:0 2px 8px rgba(0,0,0,.2)}
.hud-top{display:flex;justify-content:space-between;align-items:center;padding-bottom:8px;margin-bottom:8px;border-bottom:1px solid rgba(255,255,255,.2)}
.hud-brand{display:flex;align-items:center;gap:6px;font-weight:900;letter-spacing:.01em;font-size:15px;color:#fff;text-decoration:none}
.hud-brand b{color:var(--yellow)}
.hud-logo{width:20px;height:20px;display:block}
.hud-stats{display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px}
```
```html
<div class="hud">
  <div class="hud-top">
    <a class="hud-brand" href="index.html"><img class="hud-logo" src="logo/sunset_trans_mini_2.png" alt="">Pesa <b>Poa</b></a>
    <button id="lang" class="langbtn">${langLabel()}</button>
  </div>
  <div class="hud-stats">...</div>
  <div class="progress">...</div>
</div>
```
- `.hud-brand` here says plain **"Pesa Poa"** (no "Games"/"Michezo" suffix —
  that's homepage-only branding).
- Clicking the brand (logo + wordmark) **always navigates back to
  `index.html`**, from any game screen — this is a real `<a>`, not a JS
  handler, so it works from every screen the HUD appears on.
- `.hud-top` (brand + language button) has a visible thin divider
  (`border-bottom`) separating it from the `.hud-stats` row below —
  don't remove this divider, it was added deliberately to separate identity
  chrome from live game data.
- Household's HUD is green (`--green-deep`); Savings' HUD is dark orange
  (`--teal-deep`) for its distinct module identity — see the `--teal`
  color-role note above.

## Game start screen

Both games open on a start screen with this exact stack (all inside
`.stage.center`), before the language toggle/HUD ever appears:

```css
.back-btn{display:inline-block;color:var(--ink-soft);font:inherit;font-weight:700;font-size:13px;text-decoration:none;margin:0 0 18px}
.back-btn:hover{color:var(--green-deep)} /* or var(--teal-deep) on savings.html */
.start-logo{width:48px;height:48px;display:block;margin:0 auto 12px}
.game-title{font-size:28px;font-weight:800;letter-spacing:-.01em;color:var(--green-deep);margin:0 0 16px} /* var(--teal-deep) on savings.html */
.brief{text-align:left;width:100%;margin:0 0 22px;padding:20px;list-style:none;background:var(--card);border:1px solid var(--line);border-radius:var(--r);box-shadow:var(--shadow)}
.brief li{position:relative;padding-left:20px;margin-bottom:10px;color:var(--ink-soft);font-size:15px}
.brief li:last-child{margin-bottom:0}
.brief li::before{content:"";position:absolute;left:0;top:7px;width:7px;height:7px;border-radius:50%;background:var(--green)} /* var(--teal) on savings.html */
.brief li b{color:var(--ink)}
```
```html
<div class="stage center">
  <a class="back-btn" href="index.html">← Back</a>
  <img class="start-logo" src="logo/sunset_trans_mini_2.png" alt="">
  <h1 class="game-title">${ui("gameTitle")}</h1>
  <ul class="brief">
    <li>${ui("briefDesc")}</li>
    <li>${fmt(ui("startMoney"),{m:KSh(START_MONEY)})}</li>
    <li>${ui("briefGoal")}</li>
    ${best?`<li>${ui("best")} <b>${best}</b></li>`:""}
  </ul>
  <button class="btn" id="go">${ui("start")}</button>
</div>
```
Key points — this layout went through several iterations, so don't
reintroduce earlier versions:
- **No lang-toggle button, no tagline, and no big text-only "Pesa Poa"
  logo** on the start screen — all were explicitly deleted. The only
  branding here is the small `.start-logo` image plus the game title.
- **"← Back" is plain text**, not a button/pill — no background, no
  border. It always links to `index.html` (the homepage), from either
  game.
- **`gameTitle`** is a per-game, per-language string that always ends with
  the word "Game" in English ("Money Basics Game", "Savings Game") / a
  "Mchezo wa ..." construction in Swahili ("Mchezo wa Misingi ya Pesa",
  "Mchezo wa Akiba").
- The bullet list (`.brief`) always has exactly this content, in this
  order: (1) a one-line description, (2) the starting-money line reusing
  the existing `startMoney` string, (3) the goal, (4) the best score on
  this device — only shown if `best > 0`. It's a real card (bordered,
  shadowed, full width matching the button below it), not plain text.
- The old "type your name" input (`.namebox`, `state.name`,
  `scoreNamed`/`scoreUnnamed`) was removed entirely earlier and must not
  come back — the end screen shows one unconditional `score` string.
- The Start button always reads "Start the game →" / "Anza mchezo →"
  (key `start`), not a per-game verb like "Start the month" or "Start
  saving".

## Motion

Only `background`, `transform` (hover-lift, or the homepage card's larger
`translateY(-3px)` lift), and simple `width`/opacity transitions, always
fast (`.15s`–`.4s ease`). No bounce, no press-flatten, no scale animations.
Entrance animations must respect
`@media (prefers-reduced-motion: reduce)`.

## Bilingual content pattern (en/sw)

```js
const L = { en:{ ... }, sw:{ ... } };
let LANG = "en";
function loadLang(){ try{ const v=localStorage.getItem("pesa_lang"); if(v==="en"||v==="sw") LANG=v; }catch(e){} }
function saveLang(){ try{ localStorage.setItem("pesa_lang",LANG); }catch(e){} }
function t(k){ return L[LANG][k]; }
```
The language toggle swaps `LANG`, saves it, and re-renders. New pages must
follow this exact pattern (same localStorage key `pesa_lang`).

## Rendering pattern

Pages render into a single `#app`/`.wrap` via a template-literal `render()`
function called on load, keeping all copy centralized in `L`.

## Rules when building a new page

1. Start from the `:root` token block above, unchanged.
2. Decide up front: is this the homepage (flat hero pattern) or a game
   (sticky colored HUD + start-screen pattern)? Don't mix the two.
3. Reuse existing class names/patterns instead of inventing new ones when
   one already fits.
4. Keep everything inline in one `<style>` + one `<script>` per `.html`
   file, flat at the repo root — no external CSS/JS/font files, no `apps/`
   subfolder, no `../` paths (must work fully offline; clean URLs depend on
   the flat layout).
5. New copy must be added to both `en` and `sw` in `L` — never hardcode
   English strings in the template.
6. Max width stays 560px; mobile-first single column.
7. Buttons/cards stay flat (thin border, soft ordinary shadow, hover-lift).
8. Font stays Arial only; radii come from `--r`/`--rs`; tags use the four
   pastel pairs; game-card color roles stay in sync between the card's
   `border-left` and its `.gicon` tint.
9. Don't add a name-entry step, a lang toggle or tagline on the game start
   screen, or a colored gradient banner/feature-pill row to the homepage
   hero — all were tried and explicitly removed.
10. If a new top-level page needs a clean URL (no `.html`), just add the
    file at the repo root — `vercel.json`'s `cleanUrls: true` handles the
    rest automatically. Don't add custom `rewrites`; they didn't work
    reliably for this project.
