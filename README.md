# Motion & Composition System

Animation and visual structure for high-end marketing sites. **No colour, no typography, no
content opinions** — those are supplied separately. This describes how things move and how the
frame is composed, nothing else.

---

## What's here

```
system/
  motion-visual-system.md      The spec. Easing, durations, stagger, twelve patterns, composition.
  motion-system-board.svg      2560x3738 spec board. Drag into Figma; imports as editable layers.
site/
  meridian.html                Reference build. Single file, zero dependencies, all patterns running.
```

Figma source of the spec board:
<https://www.figma.com/design/e36vx2K5PlnUiQY8PbQrxY>

To brief a design tool, point it at `system/motion-visual-system.md`.

---

## Easing — four curves, nothing else

| Name | Curve | Use |
|---|---|---|
| reveal | `cubic-bezier(.16, 1, .3, 1)` | Every entrance |
| curtain | `cubic-bezier(.83, 0, .17, 1)` | Transitions, wipes, masks |
| ui | `cubic-bezier(.25, 1, .5, 1)` | Hover, press |
| drift | `cubic-bezier(.33, 0, .15, 1)` | Parallax, scroll-scrub |
| linear | — | Marquees only |

Banned: `ease`, `ease-in-out`, `ease-in`, and any spring overshooting past 1.02.

## Duration ladder

`120ms` · `240ms` · `480ms` · `900ms` · `1400ms` — never an arbitrary number.

## Stagger

Lines `90ms` · words `60ms` · chars `28ms` (≤3 words only) · grid `70ms`.

---

## Twelve patterns

Every section uses one:

**01** line mask reveal · **02** media settle · **03** pinned scrub · **04** horizontal marquee ·
**05** layer parallax · **06** sticky swap · **07** magnetic cursor · **08** page curtain ·
**09** cursor-reactive 3D · **10** infinite marquee · **11** repeating text strip ·
**12** timeline rail

Full mechanics for each in `system/motion-visual-system.md`.

---

## Composition

Asymmetry by default — content at columns 1–5 or 7–12 of a 12-column grid, centre only for the
hero. Oversized elements break the container and clip at the viewport edge. The frame edges stay
live: wordmark, nav, micro-label triplet, scroll percentage, rotated side tab. Cutout subjects
overlap headlines — above the type, below the nav. Identical vertical padding on every section.

---

## Budget

Animate only `transform`, `opacity`, `clip-path`, `filter` — never `width`, `height`, `top`,
`left`, or `margin`. Three animated properties per element, maximum. All scroll-linked work through
a single `requestAnimationFrame` loop with lerp `0.12`. Pin with native `position: sticky`; never
transform-scroll the page, it breaks sticky. Split text only after `document.fonts.ready`, or
measured line masks won't match the final wrap. Under `prefers-reduced-motion`, every scrubbed
sequence resolves to its **finished** state — never left invisible, never jump-cut.

---

## Reference build

`site/meridian.html` runs the patterns end to end with no dependencies and no build step —
preloader curtain, generative canvas hero, a 400vh pinned scrub, layered marquee, sticky swap,
magnetic cursor, and a full reduced-motion path. Its palette and typeface are placeholders; replace
them.

---

## Licence

MIT. Any 3D model, photograph, or typeface you bring needs its own licence.
