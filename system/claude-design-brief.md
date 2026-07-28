# Design system brief — premium editorial (paste into Claude Design)

Build every site against this system. It is not a suggestion set; treat each value as a constraint.

## Voice of the work
Luxury editorial. Warm ivory paper, one heroic typeface, extreme scale contrast, generous
negative space, and motion driven by scroll position rather than page load. Restraint is the
aesthetic — if a section looks sparse in a static frame, it is correct, because motion fills it.

## Colour — warm neutrals, one accent
Never pure #FFFFFF or #000000; pure values flatten the screen and kill the paper quality.

- Canvas          #F2EDE4   warm ivory
- Canvas alt      #E9E2D6   alternating band
- Ink             #1A1713   headings, display type
- Ink muted       #6B6459   body copy
- Ink faint       #A8A093   micro-labels, rules
- Accent          #B98C52   sand — max 5 uses on the whole page, count them
- Deep (inverted) #12100D   one full-bleed dark band per page, at a narrative pivot
- Rule            rgba(26,23,19,.13)

## Typography
- Display: a high-contrast serif (Cormorant Garamond / Canela / Ogg). Weight 300 only.
- UI + body: a neutral grotesque (Inter / Neue Haas Grotesk Display). Weights 300/400/500.
- Micro-labels: a mono (JetBrains Mono / Söhne Mono), 11px, uppercase.
- Three weights maximum across the entire site. If you want Bold 700, the type is too small.

Fluid scale (clamp, 390px → 1728px):
- mega   80 → 240px   overflow type that clips past the viewport edge
- h1     52 → 152px   hero
- h2     36 → 84px    section heads
- h3     24 → 42px    sub-heads
- lead   17 → 22px
- body   15 → 17px
- micro  12px fixed   letterspaced caps
- mono   11px fixed   edge labels, counters

Tracking is the single biggest premium tell — large type tightens, small type opens:
- ≥80px  −0.045em     · 52–80px −0.035em    · 36–52px −0.02em
- body    0em         · uppercase micro +0.2em    · mono +0.09em

Leading: display goes below 1.0 (mega 0.86, h1 0.94). Body 1.6. Nothing in between.
Body copy never exceeds 36ch. Long paragraphs break the aesthetic.

## Layout
- Container 1408px, gutter fluid 20 → 56px, 12 columns.
- Section padding fluid 96 → 224px, identical on every section — consistent rhythm is what
  makes irregular content placement read as intentional.
- Asymmetric by default: content at columns 1–5 or 7–12. Centre only for the hero.
- Break the container: mega type and media exceed it and clip at the viewport edge.
- Use the frame edges. Reserve an inset band for: wordmark top-left, nav top-right,
  micro-label triplet bottom-left, live scroll percentage bottom-right. This is the fastest
  signal that a human art-directed the page.
- Layer product cutouts *over* headline type. Nothing else does as much for perceived quality.

## Motion
Easing — never use `ease`, `ease-in-out`, or any spring that overshoots:
- reveal   cubic-bezier(.16, 1, .3, 1)     every entrance
- curtain  cubic-bezier(.83, 0, .17, 1)    transitions, wipes, masks
- ui       cubic-bezier(.25, 1, .5, 1)     hover, press
- drift    cubic-bezier(.33, 0, .15, 1)    parallax, scrub mapping
- linear   marquees only

Durations, pick from the ladder, never an arbitrary number:
120ms · 240ms · 480ms · 900ms · 1400ms
Stagger: lines 90ms · words 60ms · chars 28ms (≤3 words only — char stagger on a full
sentence is the #1 slop signal) · grid 70ms

Required patterns — every section uses one of these:
1. Line mask reveal — split headlines by measured line, each in overflow:hidden, inner
   translateY(110%) → 0, 900ms reveal, 90ms stagger, fire once at 85% viewport, never re-run.
2. Media settle — image starts scale(1.06) and settles to 1 mapped to scroll, paired with a
   clip-path wipe inset(0 0 100% 0) → inset(0).
3. Pinned scrub — pin a section 300–400vh and map scroll progress onto a drawing/sequence;
   overlay captions crossfade at fixed progress marks. The most expensive-looking pattern there is.
4. Horizontal marquee — pin, translate a wider-than-viewport track on X against vertical
   scroll, layers at different speeds, one running counter-direction.
5. Layer parallax — background 0.75×, copy 1×, accent 1.15×. Never exceed 1.25×.
6. Sticky swap — section holds while content blocks crossfade and translate 24px.
7. Magnetic cursor — 7px dot plus a lerped ring that scales to 76px over interactive
   elements, mix-blend-mode difference. Desktop and fine-pointer only.
8. Page curtain — a panel wipes up, route swaps behind it, panel exits upward.

Budget: max three animated properties per element, and only transform / opacity / clip-path /
filter. Never animate width, height, top, left, or margin. Route all scroll-linked work through
a single rAF loop with lerp smoothing (0.12). Use native position:sticky for pinning — do not
transform-scroll the page, it breaks sticky.

## Components
Nav fixed and transparent with mix-blend-mode:difference so it survives light and dark sections;
auto-hide on scroll-down. Hero holds three elements maximum — eyebrow, headline, one media
object; no paragraph, no two buttons. Numbered section markers `(01) — NAME` in mono. Buttons in
two variants only, filled and ghost, with a bottom-up fill wipe on hover; no tertiary, no icon
buttons. Footer is an oversized wordmark clipped by the viewport bottom plus mono link columns.

## Imagery
Cutouts over rectangles. Aggressive crops. 3–5% grain over everything — it unifies mixed sources
and kills the clean-render look. Colour-grade every image to the palette; mismatched temperature
is the loudest amateur signal on an otherwise good page. Radii near zero (0–4px); curves read cheap.

## Accessibility and performance
Full prefers-reduced-motion path — scrubbed sequences resolve to their finished state, never sit
invisible or jump-cut. Focus-visible rings on everything, 2px accent at 3px offset. Split text
keeps aria-label on the container. Reserve every media aspect-ratio (CLS < 0.05). LCP < 2.0s,
JS < 120KB gzipped. Split lines only after document.fonts.ready, or the masks won't match the
final wrap.

## Never ship
Purple or blue-to-pink gradients · frosted-glass cards over blurred blobs · emoji as icons ·
three feature cards with icon + title + two lines of filler · everything centred · 12px radius on
every surface · identical fade-up on load for every element · char stagger on a sentence · bouncy
springs · more than one accent · ungraded stock photo in a rounded rectangle · text drop shadows ·
headline and body at the same tracking · unused viewport edges.
