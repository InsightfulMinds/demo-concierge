# Demo Desk — Design System ("Matte Mint Studio")

Single source of truth for the look & feel of `index.html`, `social.html`, and any future page.
If a value here and the code disagree, **this file wins** — update the code to match.

> Brand: **Demo Desk** · domain `askdemodesk.com` · voice: calm, disciplined, specific.
> Aesthetic: warm cream paper, soft mint + sage, **matte** deep-forest ink, with a terracotta
> clay accent and a muted ochre pop. Premium and organic, not techy-neon.

---

## 1. Color tokens

Declared once in each page's `:root`. Copy verbatim — do not hand-tweak hexes per page.

| Token | Hex | Use |
|---|---|---|
| `--cream` | `#F4EFE3` | page background (warm creamish white) |
| `--cream-hi` | `#FBF7EE` | lighter cream — light text on dark, raised panels |
| `--paper` | `#FFFFFF` | crisp cards needing max contrast |
| `--panel` | `#FCF9F1` | soft cream cards |
| `--mint` | `#BFE3CE` | primary soft mint — pills, soft fills, logo tile |
| `--mint-2` | `#A6D7BC` | deeper mint — dots, status |
| `--mint-soft` | `#DDEEE3` | tint fills (fields, table head) |
| `--sage` | `#8FAE9B` | muted green — secondary labels, borders on hover |
| `--forest` | `#28433A` | **primary brand ink** — headings, primary buttons, logo strokes |
| `--forest-2` | `#1E332C` | darkest — footer, button hover, dark hero |
| `--ink` | `#2C3A34` | body text (soft matte charcoal-green) |
| `--muted` | `#6E7B72` | secondary text |
| `--clay` | `#CC8264` | **accent** (complement to mint) — emphasis, accent CTAs, arrows, italics |
| `--clay-2` | `#B96E50` | clay hover / deeper |
| `--clay-soft` | `#EFD9CC` | clay tint fill |
| `--ochre` | `#D9B36B` | secondary warm pop — sparingly (stars, highlights) |
| `--ochre-soft` | `#F0E3C6` | ochre tint (Rule-0 / moat panel) |
| `--line` | `#E4DCCB` | warm hairline borders |
| `--line-2` | `#D8CFBB` | stronger border |

**Accent rule:** forest is the workhorse dark; **clay** is the one accent that draws the eye
(use it on ~1 thing per view); ochre is a rare second pop. Never introduce blue/purple into chrome.

### Platform-authentic exception (social.html only)
The IG/YouTube/LinkedIn/TikTok mock feeds MUST keep each platform's real colors so they read as
genuine: `--ig` (IG gradient), `--yt:#ff0000`, `--li:#0a66c2`, `--tt:#000000`, `--verify:#3897f0`,
TikTok follow `#fe2c55`. Only the **site chrome** (nav, hero, tabs, footer) and the **brand-generated
imagery** (avatar/banner/post tiles) use the Matte Mint palette.

---

## 2. Typography

- Display / headings: **Fraunces** (serif), weights 500–700, `letter-spacing:0`.
- Body / UI: **Inter**, 400–800.
- Load: `https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,500;9..144,600;9..144,700&family=Inter:wght@400;500;600;700;800&display=swap`
- Scale: h1 `clamp(3rem,6.4vw,5.6rem)` line-height .96 · h2 `clamp(2.1rem,4.2vw,3.5rem)` · body `1.0–1.18rem` line-height 1.6.
- Accent device: occasional **italic Fraunces in `--clay`** for a key word (e.g. hero "*interest.*").
- Section kicker: Inter, `.78rem`, `letter-spacing:.14em`, uppercase, color `--clay`.

---

## 3. Logo — the desk mark

A minimalist **desk**: laptop on a desktop with two legs and a small clay desk-lamp, on a mint
rounded tile. Forest strokes (`#28433A`), clay accent (`#CC8264`), cream screen (`#FBF7EE`).
Canonical inline SVG (also the favicon):

```html
<svg viewBox="0 0 40 40" width="32" height="32" aria-hidden="true">
  <rect width="40" height="40" rx="11" fill="#BFE3CE"/>
  <rect x="14" y="10.5" width="12" height="8.5" rx="1.4" fill="#FBF7EE" stroke="#28433A" stroke-width="2"/>
  <line x1="20" y1="19" x2="20" y2="21.6" stroke="#28433A" stroke-width="2"/>
  <rect x="7.5" y="21.6" width="25" height="3.2" rx="1.6" fill="#28433A"/>
  <path d="M11 24.8l-1.2 7M29 24.8l1.2 7" stroke="#28433A" stroke-width="2.4" stroke-linecap="round"/>
  <path d="M30.6 21.6v-3.2l2.6-1.8" fill="none" stroke="#CC8264" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round"/>
  <circle cx="33.4" cy="16.2" r="1.7" fill="#CC8264"/>
</svg>
```

> NOTE: Sean is providing real logo/imagery. This SVG is the interim mark — when real assets land,
> swap the nav/footer logo + favicon and update this section. Keep the desk concept.

---

## 4. Components

- **Radii:** `--r:18px` (cards), `--r-sm:12px` (controls).
- **Shadows:** `--shadow-sm` hairline · `--shadow` cards · `--shadow-lg` raised console. All tinted `rgba(40,67,58,*)` (forest), never neutral gray.
- **Buttons:** `.primary` = forest fill / cream text; `.secondary` = translucent cream / forest text / line border; on dark bands the primary becomes **clay**. Min-height 50px, radius 12px, `translateY(-2px)` on hover.
- **Pills/tags:** mint-soft fill, forest text, 999px radius.
- **Cards:** paper/panel bg, `--line` border, soft shadow, `translateY(-3px)` hover lift.
- **Grain:** subtle `feTurbulence` SVG overlay at ~3.5% on `body::before` for matte paper texture.
- **Layout:** `.wrap` = `min(1140px, 100% - 40px)`; sections `~96px` vertical; generous whitespace.

---

## 5. Imagery (social.html generated tiles)

Post/video tiles are inline-SVG generated. Tile gradient palette (`PAL`) is brand-tinted — keep keys,
these values:
```
blue:['#28433A','#6FA98C']  dark:['#1E332C','#28433A']  amber:['#B96E50','#D9B36B']  green:['#2F6B4F','#8FAE9B']
pink:['#B96E50','#CC8264']  teal:['#3F7B5F','#A6D7BC']  violet:['#4F6F62','#8FAE9B']  slate:['#4F6F62','#6E7B72']
```
Avatar = the desk mark on a forest→mint gradient. Banner = forest gradient + mint rings + clay tagline.

---

## 6. Accessibility

- Body text `--ink` on `--cream` ≈ 9:1 — good. `--muted` on cream ≈ 4.6:1 — body-size only.
- Light text on forest/clay must be `--cream-hi`/white. Avoid white text on `--mint` (too light).
- All interactive controls keep a visible focus/hover state.

---

## 7. Files & collaboration

- `index.html` — landing (hero + live triage console). **Owner this round: Codex.**
- `social.html` — mock IG/YT/LinkedIn/TikTok social proof. **Owner this round: Claude.**
- `DESIGN.md` — this contract (Claude). Both pages must conform.
- Coordination: edit your owned file; if you must touch the other, announce it. Tokens above are
  canonical — never fork the palette per page. Pull `--rebase` before pushing; commit only your files.

## 8. Deployment

GitHub `InsightfulMinds/demo-concierge` → Cloudflare Pages → `https://demo-concierge.pages.dev`
(+ GitHub Pages mirror). A push to `main` redeploys. Ship `index.html` + `social.html` together so the
two pages never go live on mismatched themes.

## 9. Video embed (performance-clean)

**Canonical pattern: a lazy "facade."** Render a branded poster + play button; load the real player
ONLY on click/Enter. Nothing (iframe JS, player, video bytes) loads on initial page load.

- Lives in `index.html` at `#watch` (owner: Claude). **Do not add a second player** — reuse this one.
- Source is config-only via two data-attrs on `#videoFrame`:
  - `data-yt="VIDEO_ID"` → loads `https://www.youtube-nocookie.com/embed/ID?autoplay=1&rel=0` (recommended if the video is on YouTube).
  - `data-mp4="path.mp4"` → loads `<video controls autoplay playsinline preload="none">` (zero third-party).
  - Neither set → shows a brief "video coming soon" note (no error).
- Rules: never autoplay on load; the injected `<iframe>` carries `loading="lazy"`; poster is inline
  CSS/SVG (no network request); use `youtube-nocookie` (no cookies until play).
- Recommendation: **YouTube facade** for reliability + free hosting; self-hosted MP4 for max privacy/control.

### Canonical preview copy (marketing voice — never implementation talk)
The preview that shows before play must read as brand copy. Do NOT expose load mechanics, "lite embed",
"swap in the YouTube ID", or "MVP path" to visitors. Use:
- Pill / eyebrow: `90-SECOND WALKTHROUGH`
- Headline: `One signal in. The right move out.`  (alt: `The minutes after interest, handled.`)
- Subtext: `Watch Demo Desk take a real demo event — read the intent, pick the route, write the follow-up, and hand it off clean. No spray-and-pray, no guessing.`
- Play button `aria-label`: `Play the walkthrough`
- Before the recording exists, the pill may read `PREMIERES SOON` — that's the only "not-ready" signal a visitor should see.

## 10. Proof page (`proof.html`)

Standalone, branded, scrollable render of the 10 `PROOF_LOG.md` events (owner: Claude). It does **not**
rewrite proof copy — values come straight from the proof log. Has a sticky **clickable event index**
(jump to any event) + per-event cards (fields, decision chips, PASS, Rule 0 + batch styled distinctly).

Wire it as a **clickable** (navigation, not a copy change):
- Nav on `index.html` + `social.html`: add `<a class="nav-link" href="proof.html">Proof</a>` by Overview/Social.
- `index.html` proof section CTA: `<a class="button secondary" href="proof.html">Open the proof log →</a>`.
- Demo-script beat "Open PROOF_LOG.md and scroll" → opens `proof.html` and scrolls the events.
