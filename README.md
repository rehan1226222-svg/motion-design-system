# Premium Editorial — Motion & Design System

A design system for building fully-animated, high-end marketing sites. Restrained palette, one
heroic typeface, extreme scale contrast, and motion driven by scroll position rather than page load.

Derived from three references: the Cartier *Shaping Movement* site, a product-editorial redesign
(oversized lowercase grotesque, cutouts layered over type), and a portfolio site using a
cursor-reactive 3D hero and infinite marquees.

---

## What's here

```
system/
  motion-visual-system.md      Motion + composition spec. Colour-agnostic — the canonical doc.
  motion-system-board.svg      2560×3738 spec board. Drag into Figma; imports as editable layers.
  ATELIER-design-system.md     Full system including colour and typography, two skins.
  claude-design-brief.md       Condensed, paste-ready brief for an AI design tool.
tokens/
  tokens.css                   Custom-property token sheet. Two skins: maison (warm) / studio (cool).
site/
  meridian.html                Working reference build. Single file, zero dependencies.
```

Figma source of the spec board:
<https://www.figma.com/design/e36vx2K5PlnUiQY8PbQrxY>

---

## Quick start

**Building a site:** open `site/meridian.html` in a browser to see the system running, then paste
`tokens/tokens.css` above your stylesheet and set `data-skin="maison"` or `"studio"` on `<html>`.

**Briefing a design tool:** paste `system/claude-design-brief.md`. It's written as constraints
rather than options, so the tool has no room to drift into defaults.

**Working in Figma:** open the file linked above, or drag `system/motion-system-board.svg` onto a
canvas.

---

## The system in one screen

**Easing — four curves, nothing else.**

| Name | Curve | Use |
|---|---|---|
| reveal | `cubic-bezier(.16, 1, .3, 1)` | Every entrance |
| curtain | `cubic-bezier(.83, 0, .17, 1)` | Transitions, wipes, masks |
| ui | `cubic-bezier(.25, 1, .5, 1)` | Hover, press |
| drift | `cubic-bezier(.33, 0, .15, 1)` | Parallax, scroll-scrub |

Banned: `ease`, `ease-in-out`, and any spring overshooting past 1.02.

**Durations:** 120 · 240 · 480 · 900 · 1400ms. Never an arbitrary number.

**Stagger:** lines 90ms · words 60ms · chars 28ms (≤3 words only) · grid 70ms.

**Tracking** is the single biggest premium tell — large type tightens (`−0.045em` at ≥80px),
uppercase micro-labels open (`+0.2em`). Uniform tracking is the loudest amateur signal.

**Twelve patterns.** Every section uses one: line mask reveal · media settle · pinned scrub ·
horizontal marquee · layer parallax · sticky swap · magnetic cursor · page curtain ·
cursor-reactive 3D · infinite marquee · repeating text strip · timeline rail.

**Budget.** Animate only `transform`, `opacity`, `clip-path`, `filter` — never `width`, `height`,
`top`, `left`, or `margin`. Three animated properties per element, maximum. All scroll-linked work
through a single `requestAnimationFrame` loop with lerp 0.12. Pin with native `position: sticky`;
never transform-scroll the page, it breaks sticky.

---

## Reference build

`site/meridian.html` implements the system end to end with no dependencies and no build step:

- Preloader with a counting curtain that wipes upward
- Generative flow-field canvas hero (value-noise particle field, not a video or image)
- 400vh pinned scrub that draws a watch dial as you scroll — rings stroke on, indices light
  sequentially, balance wheel spins, hands sweep
- Horizontal marquee with three layers at different speeds, one counter-direction
- Sticky feature swap against a generative orbit canvas
- Custom cursor with lerped ring and magnetic targets
- Full `prefers-reduced-motion` path — scrubbed sequences resolve to their finished state

Line splitting waits on `document.fonts.ready`, since measured line masks won't match the final
wrap otherwise. Re-splits on significant resize.

---

## Notes on assets

The system describes techniques, not artwork. Any 3D model, photograph, or typeface you use needs
its own licence — check commercial use and credit the author. The Three.js approach documented in
pattern 09 needs no external asset or account.

---

## Licence

MIT for the code and documentation in this repository. Third-party fonts and assets referenced by
name are not included and carry their own terms.
