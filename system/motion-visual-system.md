# Motion & composition system

Animation and visual structure only. Colour, type, and content are supplied separately —
nothing here assumes a palette or a typeface.

---

## Easing — four curves, nothing else

| Name | Curve | Use |
|---|---|---|
| reveal | `cubic-bezier(.16, 1, .3, 1)` | Every entrance. Fast start, long settle. |
| curtain | `cubic-bezier(.83, 0, .17, 1)` | Transitions, wipes, masks. Symmetric. |
| ui | `cubic-bezier(.25, 1, .5, 1)` | Hover, press, small state changes. |
| drift | `cubic-bezier(.33, 0, .15, 1)` | Parallax and scroll-scrub mapping. |
| linear | — | Marquees and progress bars only. |

**Banned:** `ease`, `ease-in-out`, `ease-in`, and any spring overshooting past 1.02. Browser
defaults read as unstyled; overshoot reads as a consumer toy.

## Duration ladder — never an arbitrary number

`120ms` small UI colour/opacity · `240ms` hover, underline sweep · `480ms` card and media reveal ·
`900ms` headline lines, mask wipes · `1400ms` page curtain, hero settle

## Stagger

Lines `90ms` · words `60ms` · chars `28ms` (≤3 words only — char stagger across a full sentence
is the loudest slop signal there is) · grid items `70ms`

---

## The scroll patterns

Every section uses one of these, or one of the four in *3D & interactive* below. If a section
fits none of them, question whether it should exist.

**01 · Line mask reveal.** Split the headline by *measured* line. Each line sits in a
`overflow:hidden` wrapper; the inner element goes `translateY(110%) → 0`. 900ms on reveal, 90ms
stagger, fires once at 85% viewport, never re-runs on scroll-back.

**02 · Media settle.** Image starts at `scale(1.06)` and settles to `1` mapped to scroll on drift.
Paired with a `clip-path` wipe from `inset(0 0 100% 0)` to `inset(0)` on curtain, 1400ms. Parent
keeps `overflow:hidden`.

**03 · Pinned scrub.** Pin 300–400vh, map scroll progress 0→1 onto a drawing, sequence, or video
`currentTime`. Overlay captions crossfade at fixed marks — 0.15 / 0.45 / 0.8. Preload the whole
sequence behind a numeric counter before unlocking the pin. Most expensive-looking pattern available.

**04 · Horizontal marquee.** Pin, then translate a wider-than-viewport track on X as a function of
vertical scroll. Travel = `scrollWidth − innerWidth`, so a track never exposes empty space. Stack
layers at different speeds with one running counter-direction.

**05 · Layer parallax.** Background `0.75×`, copy `1×`, accent `1.15×`. Never exceed `1.25×` —
past that it induces motion sickness and reads cheap.

**06 · Sticky swap.** A visual pins while content blocks crossfade and rise 24px at scroll
thresholds. Use this instead of a three-up card grid. The pinned visual responds to section progress.

**07 · Magnetic cursor.** 7px dot at exact pointer position; a ring lerping at `0.14` that scales
38 → 76px over interactive elements, `mix-blend-mode: difference`. Targets pull `0.22×` toward the
pointer within a 60px radius, released on ui. Reserve for deliberate targets — not every link.
Gate behind `(hover:hover) and (pointer:fine)`.

**08 · Page curtain.** A panel wipes up from the bottom, the route swaps behind it, the panel exits
upward. 1400ms curtain. The outgoing page holds still — never animate both at once.

---

## Composition

**Asymmetry by default.** Content occupies columns 1–5 or 7–12 of a 12-column grid. Centre only
for the hero.

**Break the container.** Oversized type and media exceed the container and clip at the viewport
edge. Use `overflow-x: clip` on a wrapper — never `overflow: hidden` on `body`, which kills sticky.

**Live frame edges.** Reserve an inset band around the viewport for a wordmark, nav, a micro-label
triplet, a live scroll percentage, and a rotated side tab. Cheap to build, and the fastest signal
that a human art-directed the page.

**Layering.** Cutout subjects overlap the headline — above the type, below the nav. Nothing else
does as much for perceived quality.

**Section rhythm.** Identical vertical padding on every section. Consistent rhythm is what makes
irregular content placement read as intentional rather than accidental.

---

## 3D & interactive

**09 · Cursor-reactive 3D hero.** A single geometric object in Three.js that tilts toward the
pointer. The whole effect is damped follow — never map the pointer 1:1, which reads as twitchy:

```js
// target from pointer offset, normalised to −0.5…0.5
target.x = (e.clientY / innerHeight - 0.5) * 0.6;
target.y = (e.clientX / innerWidth  - 0.5) * 0.9;

// in the rAF loop — 0.06 is the damping constant
mesh.rotation.x += (target.x - mesh.rotation.x) * 0.06;
mesh.rotation.y += (target.y - mesh.rotation.y) * 0.06;
mesh.rotation.z += 0.0015;              // idle drift, always running
```

Rules: idle drift continues when the pointer leaves, so the object never looks frozen. Pause the
render loop when the canvas is offscreen (`IntersectionObserver`). Cap `devicePixelRatio` at 2.
On touch, drop the pointer input and keep drift only — do not wire it to device orientation
without asking. Under `prefers-reduced-motion`, render one static frame and stop.

Geometry that holds up at this tier: icosahedron, torus knot, or a subdivided plane with a
vertex-displacement shader. Wireframe or a single matte material — no chrome, no rainbow env maps.

> If you use a pre-made model instead (Spline, Sketchfab, a marketplace), check the licence
> covers commercial use, and credit the author. Models are someone's work — the technique is free
> to copy, the asset is not.

**10 · Infinite marquee.** Duplicate the track so two identical copies sit end to end, then
translate `0 → −50%` on `linear`, infinite. The duplicate is what makes the wrap seamless. Pause
on hover. Easing anything here makes the seam visible — linear only.

**11 · Repeating text strip.** Same mechanism at display size: one phrase repeated across a
full-bleed band with a separator glyph between repeats. Keep it under ~40px/s or it becomes
unreadable and starts to feel like a stock ticker.

**12 · Progressive timeline rail.** A vertical rail whose fill maps to section scroll progress.
Each entry resolves — rises 24px and fades in, 70ms apart — as its node is passed. Nodes ahead of
the progress point stay outlined and offset; passed nodes fill solid. Never re-runs on scroll-back.

---

## Budget & fallback

- Animate only `transform`, `opacity`, `clip-path`, `filter`. Never `width`, `height`, `top`,
  `left`, or `margin` — they trigger layout and drop frames.
- Three animated properties per element, maximum. Past that the intent stops reading.
- All scroll-linked work runs through a single `requestAnimationFrame` loop with lerp `0.12`.
  Pin with native `position: sticky`; do not transform-scroll the page.
- Split text only after `document.fonts.ready` resolves, or measured line masks won't match the
  final wrap.
- `prefers-reduced-motion`: every scrubbed sequence resolves to its **finished** state. Never
  left invisible, never jump-cut.
- Lock every media `aspect-ratio` before load. CLS under 0.05.
