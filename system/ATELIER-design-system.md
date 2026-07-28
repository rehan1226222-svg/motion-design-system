# ATELIER — Premium Editorial Design System

A build system for fully-animated, high-end marketing sites. Derived from two references:

- **Ref A — Cartier *Shaping Movement*.** Warm ivory canvas, letterspaced thin caps, centred axis, scroll-scrubbed cinematic film, single product hero.
- **Ref B — "sofi" product site (Wavely AI redesign).** Cool off-white canvas, oversized tight-tracked lowercase grotesque, product cutouts layered over type, horizontal-scroll marquee, micro-labels at the frame edge.

They look different. They are the same system: **restrained palette, one heroic typeface, extreme scale contrast, generous negative space, and motion driven entirely by scroll position rather than page load.** That's the recipe. Everything below is the specification.

Companion file: `tokens.css` — paste it above your stylesheet and everything here resolves.

---

## 1. Principles

These are decision rules, not vibes. When stuck, apply in order.

1. **One typeface does the heavy lifting.** A display face plus a mono for micro-labels. Never three families. Never a "fun" secondary.
2. **Scale contrast, not colour contrast.** The gap between your smallest text (11px mono) and largest (up to 288px) is what reads as expensive. Ratio should exceed 20:1.
3. **Negative space is the product.** If a section feels sparse in a static mockup, it's probably right — motion fills it.
4. **Colour is nearly absent.** Two neutrals plus one accent used under five times per page. Full stop.
5. **Motion answers to scroll.** Entrance-on-load animations feel like a template. Scroll-linked transforms feel authored.
6. **Nothing bounces.** Overshoot springs read as playful/consumer. Use expo and quint easing. One exception per site, maximum.
7. **Edges of the frame are used.** Micro-labels, scroll progress, rotated side tabs, section counters. This is the single fastest signal that a human art-directed it.
8. **Media is treated, not dropped in.** Cutouts, duotone, grain, letterbox crops, scale-on-scrub. Never a raw stock photo in a rounded card.

---

## 2. Colour

Two skins ship in `tokens.css`. Pick one per project — never mix.

### Skin: `maison` (Ref A lineage)

| Token | Value | Use |
|---|---|---|
| `--bg` | `#F2EDE4` | Page canvas — warm ivory |
| `--bg-alt` | `#E9E2D6` | Alternating band, cards |
| `--fg` | `#1A1713` | Headings, primary text |
| `--fg-muted` | `#6B6459` | Body copy |
| `--fg-faint` | `#A29A8C` | Micro-labels, rules |
| `--accent` | `#C9A87C` | Sand — underlines, active states |

### Skin: `studio` (Ref B lineage)

| Token | Value | Use |
|---|---|---|
| `--bg` | `#F5F4F2` | Page canvas — cool paper |
| `--bg-alt` | `#EAE9E6` | Alternating band |
| `--fg` | `#0D0D0C` | Headings, display type |
| `--fg-muted` | `#5A5A57` | Body copy |
| `--fg-faint` | `#9B9B96` | Micro-labels |
| `--accent` | `#16150F` | Near-black — buttons, fills |

### Rules

- **Never `#FFFFFF` or `#000000`.** Pure values flatten the screen and kill the paper quality. The ramps are warm-biased on purpose.
- **Inverted sections** use `[data-invert]` — one or two per page, always full-bleed, always at a narrative pivot (the "how it works" or the film sequence).
- **Accent budget: 5 uses per page.** Count them. Link underline, one button fill, scroll-progress bar, one rule, one hover state.
- Contrast floor: body copy ≥ 4.5:1, display type ≥ 3:1. `--fg-faint` on `--bg` fails AA for body — it is only ever legal at ≥11px uppercase mono for decorative labels that are duplicated in the DOM elsewhere, or on non-essential text.

---

## 3. Typography

The most important section. Get this right and mediocre layout still reads premium.

### Families

| Role | Primary | Free substitute |
|---|---|---|
| Display serif (`maison`) | Canela Deck, Ogg | Instrument Serif, Newsreader |
| Display grotesque (`studio`) | Neue Haas Grotesk Display, Helvetica Now Display | Switzer, General Sans |
| Micro-label mono | Söhne Mono | Geist Mono, JetBrains Mono |

Load **three weights maximum** — Light 300, Book 400, Medium 500. If you find yourself wanting Bold 700, the type is too small.

### Scale

Fluid, `clamp()`-driven, 390px → 1728px viewport.

| Token | Min → Max | Applies to |
|---|---|---|
| `--t-mega` | 80 → 288px | Overflow marquee type, section titles that break the frame |
| `--t-h1` | 56 → 144px | Hero |
| `--t-h2` | 40 → 80px | Section heads |
| `--t-h3` | 28 → 48px | Sub-heads |
| `--t-h4` | 20 → 28px | Card titles |
| `--t-body-l` | 17 → 22px | Lead paragraph |
| `--t-body` | 15 → 17px | Body |
| `--t-body-s` | 13 → 14px | Captions |
| `--t-micro` | 12px fixed | Letterspaced caps |
| `--t-mono` | 11px fixed | Edge labels, counters, progress |

### Tracking — the tell

Optical sizing is what separates real typography from defaults. **Large type tightens, small type opens.**

| Size band | Tracking | Token |
|---|---|---|
| ≥ 80px | `-0.045em` | `--track-mega` |
| 56–80px | `-0.035em` | `--track-h1` |
| 40–56px | `-0.025em` | `--track-h2` |
| 28–40px | `-0.015em` | `--track-h3` |
| Body | `0em` | `--track-body` |
| Uppercase micro | `+0.18em` | `--track-caps` |
| Mono labels | `+0.08em` | `--track-mono` |

Ref A's `SHAPING MOVEMENT` is exactly this: ~24px, Light, uppercase, `+0.18em`. Ref B's `it all starts with a` is `--t-mega` at `-0.045em`, lowercase, Medium.

### Leading

Display leading goes **below 1.0**. `--lh-mega: 0.86`, `--lh-h1: 0.92`. Body sits at 1.55. Nothing in between.

### Case

- `maison` — sentence case display, uppercase letterspaced for eyebrows and labels.
- `studio` — **lowercase display**. This is deliberate and it is half the character of Ref B. Never sentence-case the mega type in this skin.

### Measure

Body copy never exceeds `--measure` (34ch). In these layouts body copy is a small, dense block set against enormous type — usually 2–4 lines in a corner or a single column at 4/12 grid width. Long paragraphs break the aesthetic.

---

## 4. Layout & grid

- **Container:** `--container` 1440px, `--gutter` fluid 20 → 56px.
- **Grid:** 12 columns, `--col-gap` fluid 16 → 32px.
- **Section rhythm:** `--section-y` fluid 80 → 192px vertical padding. Do not vary this per section — consistency in rhythm is what makes the irregular content placement read as intentional.

### Composition rules

1. **Asymmetry by default.** Content sits at columns 1–5 or 7–12. Centre only for hero moments and full-bleed film (Ref A's hero is centred; everything after is not).
2. **Break the container.** Mega type and media should exceed the container and clip at the viewport edge — the horizontal marquee in Ref B does exactly this. Set `overflow-x: clip` on a wrapper, not `hidden` on `body` (that kills position: sticky).
3. **Frame edges are live.** Reserve a 24–40px inset band around the viewport for: wordmark (TL), menu/CTA (TR), micro-label triplet (BL), scroll progress + percentage (BR). Ref B carries `purity of plants · power of people · preservation of planet` bottom-left and a live `13% / 22% / 36%` bottom-right through the whole scroll.
4. **Layering.** Product cutouts overlap type with a z-index above the headline but below the nav — Ref B's bottle crosses the `arts with a` letterforms. This single move does more for perceived quality than any effect.

---

## 5. Motion system

The core of the deliverable. **All values below are in `tokens.css`.**

### Easing

| Token | Curve | Use |
|---|---|---|
| `--ease-reveal` | `cubic-bezier(0.16, 1, 0.3, 1)` | Every entrance. Expo-out — fast start, long settle. |
| `--ease-curtain` | `cubic-bezier(0.83, 0, 0.17, 1)` | Page transitions, wipes, masks. Symmetric quint. |
| `--ease-ui` | `cubic-bezier(0.25, 1, 0.5, 1)` | Hover, press, small state changes. |
| `--ease-drift` | `cubic-bezier(0.33, 0, 0.15, 1)` | Long parallax and scrub mapping. |
| `linear` | — | Marquees only. Anything else linear looks broken. |

**Banned:** `ease`, `ease-in-out` (browser defaults, instantly recognisable as unstyled), and any spring with overshoot > 1.02.

### Durations

`--d-instant` 120ms · `--d-fast` 240ms · `--d-base` 480ms · `--d-slow` 900ms · `--d-epic` 1400ms

Pick from the ladder. Never type an arbitrary duration.

### Stagger

Lines 90ms · words 60ms · chars 28ms (≤3 words only — char stagger on a full sentence is the #1 slop signal) · grid items 70ms.

### Smooth scroll

Lenis, `lerp: 0.085`, `duration: 1.1`, `smoothTouch: false`. Never smooth-scroll on touch devices — it fights the OS and feels laggy. Disable entirely under `prefers-reduced-motion`.

### The eight patterns

Every section on the page uses one of these. If a section doesn't fit one, it probably shouldn't exist.

**1 — Line mask reveal (all headlines)**
Split by line. Each line wrapped in `overflow: hidden`, inner span translated `translateY(var(--rise))` → `0`. Duration `--d-slow`, ease `--ease-reveal`, stagger `--stagger-line`. Trigger at 85% viewport, `once: true`. Never re-animate on scroll-back.

**2 — Media scale settle**
Image/video starts at `scale(var(--zoom-rest))` (1.06) inside an `overflow: hidden` parent and settles to `1` over `--d-epic` on `--ease-drift`, mapped to scroll progress rather than a fixed trigger. Pairs with a clip-path wipe from `inset(0 0 100% 0)` → `inset(0 0 0 0)` on `--ease-curtain`.

**3 — Scroll-scrubbed sequence (the hero move in Ref A)**
Pin a section for 200–400vh. Map scroll progress 0→1 onto either a video's `currentTime` or a pre-rendered image sequence (120–180 frames, WebP, ~1600px wide). Overlay copy cross-fades at fixed progress marks (0.15, 0.45, 0.8). This is the single most expensive-looking pattern available and it is mostly a `<canvas>` and a `requestAnimationFrame` loop. Preload every frame before unlocking the pin; show a numeric counter while loading.

**4 — Horizontal marquee scroll (Ref B's mega type)**
Pin a section, translate a wider-than-viewport track on X as a function of vertical scroll. Ease `--ease-drift`, no snapping. Give the track a slight lead/lag on child layers (product cutout moves at 0.85× the type's speed) for parallax depth.

**5 — Pinned layer parallax**
Background media translates Y at 0.7–0.85× scroll speed; foreground copy at 1×; a small accent element at 1.15×. Never exceed 1.25× — it induces motion sickness and looks cheap.

**6 — Sticky section swap**
Section holds; content blocks cross-fade and translate 24px on `--ease-reveal` at scroll thresholds. Used for feature lists so you never build a boring 3-up card grid.

**7 — Magnetic hover + custom cursor**
Cursor is a 12px filled dot that scales to 64px and inverts (`mix-blend-mode: difference`) over interactive elements. Buttons translate toward the pointer at 0.25× the delta within a 60px radius, released with `--ease-ui`. **Desktop and fine-pointer only** — gate behind `@media (hover: hover) and (pointer: fine)`.

**8 — Page curtain transition**
On navigate: a full-bleed panel wipes up from the bottom (`--d-epic`, `--ease-curtain`), route swaps behind it, panel exits upward. Old page holds still during the wipe — do not animate both simultaneously.

### Micro-interactions

| Element | Behaviour | Duration / ease |
|---|---|---|
| Text link | Underline sweeps in from left, `transform-origin` flips to right on exit | `--d-fast` / `--ease-ui` |
| Button | Fill wipes up from bottom, label colour crossfades | `--d-fast` / `--ease-ui` |
| Image card | `scale(1.03)` on inner image only, container fixed | `--d-base` / `--ease-ui` |
| Nav item | Label translates up `--rise`, duplicate label follows | `--d-fast` / `--ease-reveal` |
| Scroll progress | Continuous, no easing — it reports position | `linear` |

### Motion budget

**Maximum three animated properties per element**, and only `transform` / `opacity` / `clip-path` / `filter`. Animating `width`, `height`, `top`, `left`, or `margin` will drop frames and is never necessary. Everything scroll-linked runs through a single rAF loop.

---

## 6. Components

Minimal set. Anything not here should be questioned.

### Nav
Fixed, transparent, `mix-blend-mode: difference` so it survives light and dark sections without JS. Wordmark left, one link cluster or a menu trigger right. Height 64–80px. Hides on scroll-down past 15% and returns on scroll-up (`--d-fast`, `--ease-ui`).

### Hero
Full viewport. Three elements maximum: eyebrow (caps micro), headline (`--t-h1` / `--t-mega`), and one media object. No paragraph. No two buttons. Ref A's hero is literally a watch, a letterspaced label, and a scroll cue.

### Scroll indicator
Bottom-right: a live percentage in `--t-mono` plus a 1px rule that fills. Bottom-left: a three-part micro-label triplet separated by `·`. Both persist for the whole page.

### Side tab CTA
Rotated 90°, fixed to the right edge, vertically centred. `--t-micro`, caps, `--track-caps`. Present in Ref B. Desktop only.

### Section marker
Top-left of each section: `(01) — SECTION NAME` in mono. Cheap to build, enormous authorial signal.

### Buttons
Two variants only.
- **Primary** — `--accent` fill, `--r-pill` or `--r-0` (pick one per site, never both), `--t-micro` caps, `--track-caps`, 14px × 28px padding, bottom-up fill wipe on hover.
- **Ghost** — `--hairline` border, transparent, same metrics.

No tertiary. No icon buttons with emoji. No gradient fills.

### Media block
`aspect-ratio` locked (16:9, 4:5, or 1:1 — pick two for the whole site). `--shadow-media` only when floating on `--bg`; never on `--bg-alt`. Grain overlay at `--grain-opacity` via a tiled noise PNG with `mix-blend-mode: overlay`.

### Footer
Oversized wordmark at `--t-mega`, clipped by the viewport bottom. Three link columns in mono caps. One line of legal at `--t-mono`. Nothing else.

---

## 7. Imagery & art direction

- **Cutouts over rectangles.** Products masked from their background and layered across type. This is the defining move of Ref B.
- **Crops are aggressive.** Letterbox to 21:9 for film moments; crop portraits so the subject exits the frame.
- **Grain always.** 3–5% noise overlay unifies mixed-source imagery and is the fastest way to kill the "clean render" look.
- **Colour-grade to the palette.** Everything gets a subtle warm or cool bias to match the skin. Mismatched image temperature is the loudest amateur signal on an otherwise good page.
- **Film > stills** for hero moments. A 6–10 second silent loop, muted, `playsinline`, poster frame set, ≤2.5MB.

---

## 8. Accessibility & performance

**Non-negotiable:**

- Full `prefers-reduced-motion` path — already wired in `tokens.css`. Scroll-scrubbed sequences fall back to a static poster frame at their midpoint, not a jump-cut.
- Focus-visible rings on every interactive element: 2px `--accent` at 3px offset. Do not remove them because they're "ugly" — restyle them.
- Custom cursor never replaces the native one on touch or for keyboard users.
- Split-text reveals must keep the full sentence readable to screen readers — set `aria-label` on the container and `aria-hidden` on the split spans.
- Pinned sections need a keyboard escape path; don't trap tab order inside a 400vh pin.

**Budget:**

| Metric | Target |
|---|---|
| LCP | < 2.0s |
| CLS | < 0.05 (reserve every media aspect-ratio) |
| Total JS | < 120KB gzipped |
| Hero media | < 2.5MB |
| Sequence frames | WebP, ≤180 frames, ≤80KB each, preloaded behind a counter |
| Fonts | 3 weights, `woff2`, subset, `font-display: swap` with a metric-matched fallback |

---

## 9. Anti-slop checklist

Ship-blockers. If any of these are true, it looks AI-generated.

- [ ] Purple / blue-to-pink gradient anywhere
- [ ] Frosted-glass cards over a blurred blob background
- [ ] Emoji used as iconography
- [ ] Three feature cards in a row with icon + title + two lines of filler
- [ ] Everything centred, top to bottom
- [ ] `border-radius: 12px` on every surface
- [ ] Fade-up on load for every element with identical timing
- [ ] Char-by-char stagger on a full sentence
- [ ] Bouncy spring easing on more than one element
- [ ] Body copy wider than 40ch
- [ ] More than one accent colour
- [ ] Stock photo dropped into a rounded rectangle, ungraded
- [ ] Drop shadow on text
- [ ] Headline and body at the same tracking
- [ ] No use of the viewport edges

**Conversely, the presence of these reads as authored:**
numbered section markers · a live scroll percentage · type that clips past the viewport edge · a product cutout overlapping a headline · negative tracking on display type · a section that pins and scrubs · asymmetric column placement · one inverted full-bleed band · grain.

---

## 10. Build order

1. Paste `tokens.css`. Set `data-skin` on `<html>`.
2. Type scale and tracking — build a specimen page first and stare at it before laying out anything.
3. Static layout, all sections, zero motion. It should already look good.
4. Lenis + a single rAF loop.
5. Pattern 1 (line reveals) everywhere. Ship-quality already.
6. Patterns 2, 5 (media settle, parallax).
7. One hero pattern — 3 or 4, not both.
8. Micro-interactions and cursor.
9. Page curtain.
10. Reduced-motion pass, keyboard pass, performance pass.

Steps 1–5 produce something that reads premium on their own. Steps 6–9 are where sites get overbuilt — add them one at a time and remove any that don't survive a second look.
