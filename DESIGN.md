---
name: Nuzzle
description: Editorial spec-sheet — real pet photography and condensed poster type, zero AI-SaaS glow chrome.
colors:
  paper: "#E7E6E5"
  ink-900: "#0B0B0A"
  ink-700: "#4A4844"
  ink-500: "#615F58"
  card: "#DCDAD8"
  card-2: "#CFCDC9"
  navy-700: "#121110"
  navy-600: "#000000"
  cat-500: "#B23E6B"
  dog-500: "#2E5AA8"
  future-500: "#3D7A5C"
  bonus-500: "#A87C2E"
  line: "rgba(11,11,10,.14)"
  line-strong: "rgba(11,11,10,.32)"
typography:
  display:
    fontFamily: "'Bebas Neue', sans-serif"
    fontSize: "clamp(2.7rem, 6vw, 6rem)"
    fontWeight: 400
    lineHeight: 0.96
    letterSpacing: "0.005em"
  body:
    fontFamily: "'Manrope', sans-serif"
    fontSize: "1rem"
    fontWeight: 400
    lineHeight: 1.6
  label:
    fontFamily: "'Space Mono', monospace"
    fontSize: "0.7rem"
    fontWeight: 700
    letterSpacing: "0.05em"
rounded:
  sm: "6px"
  md: "10px"
  lg: "18px"
  pill: "999px"
spacing:
  sm: "12px"
  md: "24px"
  lg: "48px"
  xl: "100px"
components:
  button-primary:
    backgroundColor: "{colors.ink-900}"
    textColor: "{colors.paper}"
    rounded: "{rounded.sm}"
    padding: "14px 26px"
  button-primary-hover:
    backgroundColor: "#000000"
  button-outline:
    backgroundColor: "transparent"
    textColor: "{colors.ink-900}"
    rounded: "{rounded.sm}"
  button-ghost:
    backgroundColor: "{colors.card}"
    textColor: "{colors.ink-900}"
    rounded: "{rounded.sm}"
  pill-cat:
    backgroundColor: "rgba(178,62,107,.14)"
    textColor: "{colors.cat-500}"
    rounded: "{rounded.pill}"
  pill-dog:
    backgroundColor: "rgba(46,90,168,.14)"
    textColor: "{colors.dog-500}"
    rounded: "{rounded.pill}"
  pill-future:
    backgroundColor: "rgba(61,122,92,.14)"
    textColor: "{colors.future-500}"
    rounded: "{rounded.pill}"
---

# Design System: Nuzzle

## Overview

**Creative North Star: "The Editorial Spec-Sheet"**

Nuzzle's fourth visual pass replaces three prior costumes of the same "generic AI-SaaS" genre (soft pastel, neo-brutalist, dark-cosmic "Orbital AI") with a print-editorial register: warm near-white paper, huge condensed all-caps poster type, and real pet/device photography doing the persuasive work instead of glow, gradient, blur, or glassmorphism. Surfaces are flat and stepped (paper → card → card-2), never lit from behind. Depth reads through tonal contrast and hairline dividers, not shadow or blur — confirmed zero `blur()`/`backdrop-filter` anywhere in the shipped stylesheet.

Species identity (cat/dog/future/bonus) survives only as small, muted, flat tag fills — pills, tab active-states, wheel wedges — never as page backgrounds, gradients, or glow. Buttons are small-radius solid near-black fills with white text; there is no pill-button silhouette, no gradient CTA, no colored glow shadow anywhere in the build.

**Key Characteristics:**
- Warm paper/ink neutral base with stepped card surfaces, no dark mode
- Bebas Neue condensed display type at every heading scale, tight `line-height:.96`
- Space Mono for timestamps, chips, and spec-sheet micro-labels
- Zero blur, zero glass, zero starfield, zero colored glow shadow
- Species colors confined to small flat tag/pill/active-state fills

## Colors

A warm near-white paper ground with near-black ink, stepped by two flat mid-grey card tones — no gradients, no dark theme.

### Primary
- **Ink Black** (`#0B0B0A`): heading/body text, primary button fill, active-state fills (mode toggle, plan tabs' `is-active` when not species-colored, wheel hub, badges, CTA band background).

### Secondary
- **Warm Paper** (`#E7E6E5`): page background, default surface; also used as text color reversed on ink-900 fills.

### Tertiary (species accents)
- **Muted Rose** (`#B23E6B`, cat): cat pill/plan-tab-active/wheel wedge.
- **Muted Denim** (`#2E5AA8`, dog): dog pill/plan-tab-active/wheel wedge.
- **Muted Sage** (`#3D7A5C`, future): future-pets pill/plan-tab-active/wheel wedge.
- **Muted Ochre** (`#A87C2E`, bonus): wheel-only bonus wedge (Bonus has no plan-tab entry — see Do's and Don'ts).

### Neutral
- **Ink 900** (`#0B0B0A`): primary text/icon color.
- **Ink 700** (`#4A4844`): secondary text (body copy, nav links, labels).
- **Ink 500** (`#615F58`): muted/tertiary text (captions, stat labels, spec-list descriptions). Deliberately darkened from an earlier `#7A776F` after the shipped build failed WCAG AA (3.59:1 against paper); the current value clears 4.5:1+ against both `--paper` and `--card`.
- **Card** (`#DCDAD8`) / **Card 2** (`#CFCDC9`): stepped surface tiers for alternating section backgrounds, card fills, and hover states.
- **Navy 700** (`#121110`): solid near-black block used for photo caption chips and the hero card overlay, distinct from `--ink-900` only by intent (same visual weight, kept as a separate token for chip/overlay contexts).
- **Line / Line-Strong** (`rgba(11,11,10,.14)` / `rgba(11,11,10,.32)`): hairline dividers and borders throughout — the system's only "structure" device besides type scale and surface stepping.

### Named Rules
**The Muted-Tag-Only Rule.** Species colors (cat/dog/future/bonus) never fill a background, gradient, or large surface — only small pills, active-tab states, and wheel wedges. This is a hard reversal of the prior dark-cosmic pass, which used the same hues as full neon glows.

## Typography

**Display Font:** Bebas Neue (with sans-serif fallback)
**Body Font:** Manrope (with sans-serif fallback)
**Label/Mono Font:** Space Mono (with monospace fallback)

**Character:** A poster-condensed display face carries every heading at every scale against a workhorse humanist sans for body copy; Space Mono marks anything reading as data (timestamps, stats, photo chips, spec labels) — the "spec-sheet" half of the north star.

### Hierarchy
- **Display** (400, `6rem` hero / down to `2.7rem` on mobile, line-height .96): hero `h1`, all-caps, tight tracking (`.005em`).
- **Headline** (400, `2.9rem`, line-height .96): `h2` section heads, all-caps Bebas Neue.
- **Title** (400, `1.3–1.7rem`, line-height .96): `h3` card/feature/plan titles, all-caps Bebas Neue.
- **Body** (400–600, `.85–1.05rem`, line-height 1.6): Manrope, `--ink-700`, max ~42ch on hero lead.
- **Label** (700, `.66–.8rem`, letter-spacing `.03–.08em`, uppercase): Space Mono for chips/stats/badges; Manrope uppercase for nav links and buttons.

### Named Rules
**The One-Display-Face Rule.** Bebas Neue is the only heading face at every scale from hero `h1` to card `h3` — no secondary display face, no system font substitution for headings.

## Layout

A centered `1180px` max-width `.wrap` container with `24px` gutters. Section vertical rhythm is `100px` padding (`64px` on mobile ≤720px). Grids are simple CSS Grid splits (2-col hero, 3-col services/testimonials, 4-col stats/steps) collapsing to 1–2 columns at 980px and 720px breakpoints. Card/section rows use `1px` hairline gaps filled with `--line-strong` to produce a seamed, spec-sheet-grid look (`.service-grid`, `.testimonial-grid`) rather than gapped cards with individual shadows.

## Elevation & Depth

Flat by design: depth is conveyed through tonal surface stepping (paper → card → card-2 → navy-700) and hairline borders, not shadow. The two shadow tokens that exist are near-imperceptible resting shadows, not glow or lift effects.

### Shadow Vocabulary
- **shadow-md** (`box-shadow: 0 1px 2px rgba(11,11,10,.05)`): sticky header on scroll only.
- **shadow-lg** (`box-shadow: 0 2px 4px rgba(11,11,10,.07)`): reserved, minimal use.

### Named Rules
**The No-Glow Rule.** No blurred, colored, or multi-layer shadow appears anywhere in the build — a deliberate reversal of the prior dark-cosmic pass's violet glow-arc motif. Depth comes from flat tonal contrast only.

## Shapes

Small, consistent corner radii throughout (`--radius-sm:6px`, `--radius-md:10px`, `--radius-lg:18px`) — never `0` (no neo-brutalist hard edges) and never large/pill on rectangular elements (pills are reserved for genuine pill-shaped tag chips). Borders are `1px` hairlines (`--border-w`) in `--line`/`--line-strong`, not the thick 3px black borders of the prior brutalist pass. Photo captions use a small folded-corner triangle (`::after` CSS triangle) as the system's one recurring signature mark.

## Components

### Buttons
- **Shape:** small radius (`6px`), `1px` border on outline/ghost variants.
- **Primary:** solid `--ink-900` fill, `--paper` text, `14px 26px` padding; hover darkens to `#000`. Flat — no gradient, no pill shape, no glow shadow.
- **Hover/Focus:** `translateY(-1px)` lift only, background/color transitions — no shadow change.
- **Outline/Ghost:** outline is transparent with a `--line-strong` border filling to `--card` on hover; ghost is `--card`-filled from rest, darkening to `--card-2` on hover.

### Pills / Tags
- **Style:** pill-radius (`999px`), small Space Mono uppercase label, muted 14% species-color background fill with the full-strength species color as text and a 30% border.
- **State:** no selected/unselected toggle variant — pills are static labels, not interactive filters.

### Cards / Containers (spec-sheet data row)
- **Corner Style:** `18px` (`--radius-lg`) for standalone cards (plan cards, device stage); services/testimonials sit in a seamed `1px`-gap grid instead of individually radiused cards.
- **Background:** `--card` for standalone cards, `--paper` for grid-seam cells.
- **Shadow Strategy:** none (see Elevation & Depth).
- **Border:** `1px` `--line-strong` hairline.
- **Signature: the stats data row** (`.stats`) — a quiet Space Mono spec-sheet strip directly under the hero, four inline number+label pairs (`11,400+ homes`, `2 species`, `24/7 monitoring`, `4.9/5 rating`) separated by vertical hairline dividers (`border-left:1px solid var(--line)`), not bordered big-number cards. This is the canonical spec-sheet-data-row component for the system.

### Inputs / Fields
- Not present as a distinct pattern in the shipped build (no form inputs); omitted rather than invented.

### Navigation
- Sticky header, `--paper` background, `1px` bottom hairline that darkens and gains `shadow-md` on scroll. Nav links are Manrope uppercase small-caps-weight labels (`.82rem`, `.08em` tracking) in `--ink-700`, darkening to `--ink-900` on hover/active. Mobile collapses to a hamburger toggle revealing a full-width dropdown panel.

### Plan Tabs / Checklist (signature)
- Plan tabs (`#planTabs`) list exactly **Cats, Dogs, Future Pets** — three species buttons with inline `stroke-width:1.7` line-icon + label, small-radius outline pills that fill solid with the matching muted species color and white text when active. Bonus is not a plan-tab entry; it exists only as a wheel-only novelty wedge (see Do's and Don'ts).
- Plan-card checklist items use a CSS-mask-drawn SVG checkmark (`stroke-width:2.4`, matching the site's line-icon system), not a Unicode glyph — every icon on the page (nav, feature grid, plan tabs, wheel labels, plan-card checklist) is drawn the same way: either an inline `stroke-width:1.7` line-icon SVG from the script's `ICONS` lookup, or a CSS `mask`/`-webkit-mask` applied to a pseudo-element for standalone marks like the checklist tick.

## Do's and Don'ts

### Do:
- **Do** keep species color confined to small pill/tag/active-state fills (14% muted background, full-strength text) — never a page or card background.
- **Do** draw every icon as a `stroke-width:1.7` line icon (inline SVG or CSS-mask), matching the plan-card checklist tick, nav, and wheel-label icons.
- **Do** use Bebas Neue for every heading at every scale, all-caps, `line-height:.96`.
- **Do** keep `--ink-500` at `#615F58` or darker — the lighter `#7A776F` predecessor failed WCAG AA against `--paper`.

### Don't:
- **Don't** add blur, backdrop-filter, glassmorphism, colored glow shadows, or a starfield — all confirmed absent from the shipped build and explicitly reversed from the prior dark-cosmic "Orbital AI" pass.
- **Don't** use a hard offset (non-blurred, high-contrast) drop shadow as a neo-brutalist device — this is not a neo-brutalist world; shadows in this build are near-imperceptible resting shadows only (see Elevation & Depth), not a structural or decorative device.
- **Don't** add a Bonus entry to `#planTabs` — Bonus is a wheel-only novelty (`WHEEL_ORDER` includes it, `PET_PLANS` deliberately does not); the plan-picker tabs list exactly Cats, Dogs, Future Pets.
- **Don't** use a Unicode glyph for icons anywhere — the shipped build replaced its one remaining "✓" glyph with a CSS-mask SVG specifically to match the rest of the line-icon system; don't reintroduce a glyph shortcut for a future icon need.
