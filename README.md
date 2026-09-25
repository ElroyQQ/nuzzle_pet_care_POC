# Nuzzle — AI-Powered Remote Pet Care

A single self-contained static website for **Nuzzle**, a fictional remote pet-sitting company. Nuzzle's pitch: a live HD camera paired with **NuzzlePal**, a proprietary AI companion robot, so owners can watch, talk to, and play with their cat or dog from anywhere — with the AI keeping pets company in between check-ins.

The whole site — markup, CSS, and JavaScript — lives in one file: `index.html`, plus a flat `images/` folder of photos and a `video/` folder holding one looping clip, both referenced by relative path. There is no build step, no package manager, and no dependencies to install.

## Running it

Open `index.html` directly in a browser, or serve the folder (`python3 -m http.server`) to sanity-check relative image paths the way a real host would serve them. There is no dev server, bundler, linter, or test suite configured in this repo.

## Features

- **Hero** with a looping floor-level video ("LIVE · NUZZLEPAL CAM") showing a dog walking through a home from a pet-height, following-camera POV, plus a floating "now playing" status card.
- **Services by species** — separate Cats and Dogs offerings with species-specific hardware/AI behavior, plus a "More friends, coming soon" card (rabbits, birds) with a waitlist button.
- **Meet NuzzlePal** — a real photo of a small home companion robot, with a live Cat/Dog mode toggle that swaps a transparent SVG overlay's visible accessory (laser vs. treat launcher), the status-ring color, and the caption text.
- **How it works** — a 4-step onboarding explainer.
- **App mockup** — a CSS-built phone frame with working Live / Talk / Insights tabs that swap the feed photo, badge, and copy.
- **Plans** — three plan tabs (Cats / Dogs / Future Pets) are the primary, instant-swap way to compare pricing; picking one is a near-instant panel swap, not gated behind an animation. Separately, a demoted "Feeling lucky?" **wheel** (Cats / Dogs / Future Pets / **Bonus**, built with a CSS `conic-gradient`) offers the same spin-a-wedge novelty as a side attraction, explicitly labeled as "not how you pick a plan" — spinning it or clicking a wedge stays in sync with the tabs above and swaps the same plan-detail panel. Includes a Future Pets waitlist plan alongside Cat and Dog pricing, a small link to the Multi-Pet Bundle, and a Bonus wedge (wheel-only, not a tab) that issues a one-time 10%-off voucher (a randomly generated code, valid for exactly one month from the moment it's issued — the code is generated once per page load and stays the same on repeat visits to the tab).
- **Roaming mascots** — a small fixed NuzzlePal-style robot in the corner of the page, plus a dog and a cat mascot elsewhere on screen. Click the robot and it hops to a new random spot with a bounce animation and a speech-bubble line. Click the dog or cat and *it* hops away — the robot then "notices" and hops over near wherever it landed a beat later, like it's chasing them.
- Pet-themed decorative background: a faint site-wide paw-print pattern plus a large dog silhouette watermark in "Why Nuzzle" and a cat silhouette watermark in the testimonials section (see Image & video policy below).
- **Testimonials** and an **FAQ accordion**, including an explicit, honest FAQ entry on how the AI is trained (see below).
- Responsive layout with a mobile hamburger nav (morphs into an X when open); sticky, flat-paper header that gains a hairline shadow on scroll.

## Design

The site now uses an **"editorial spec-sheet" visual language** — warm near-white paper, huge condensed all-caps display type, and real pet/device photography doing the persuasive work instead of glow, gradient, blur, or glassmorphism. It's the fourth visual pass on this project (soft-pastel → neo-brutalist → dark-cosmic "Orbital AI" → this one), and a deliberate reversal of the dark-cosmic pass specifically because that look — and its predecessors — still read as generic "AI-SaaS template" chrome no matter the palette. Direction was pinned to a specific reference (a Dribbble shot, "Humanoid AI Robot Company Website" by Dannniel for Marcato Studio). Headings use **Bebas Neue** (condensed, all-caps, tight tracking, every scale from the hero down to card titles); body copy uses **Manrope**; timestamps, stats, and photo-caption spec labels use **Space Mono** — all three loaded from Google Fonts.

Palette: `--paper` (`#E7E6E5`, warm near-white) and `--card`/`--card-2` (stepped warm-grey panel tones) carry the base; text uses `--ink-900` (near-black). Buttons are flat, small-radius solid `--ink-900` fills with white text — no gradient, no pill shape, no glow shadow. `--cat-500` (muted rose), `--dog-500` (muted denim), `--future-500` (muted sage), and `--bonus-500` (muted ochre) are the four wheel-wedge/tag colors, confined to small flat pill/tag/active-state fills — never a page background or glow. There is no blur, glassmorphism, or starfield anywhere in the build.

This was — like every prior pass — done under a pure CSS + inline-SVG-recolor discipline for the visual system itself: every element ID, class name, and `data-*` attribute JS depends on is unchanged. This pass is the one exception on script logic: a small, explicitly user-approved set of `<script>` changes folds in interaction fixes from a design critique (an instant plan-tab swap instead of a forced wheel-spin delay, roaming mascots kept clear of the mobile hero, a hamburger-to-X morph) alongside the visual rewrite. See [CLAUDE.md](CLAUDE.md#design-system) for the full account and [DESIGN.md](DESIGN.md) for the generated, token-level design system this pass produced.

## Architecture

Everything is inline in `index.html`, in three blocks in order:

1. **`<style>`** — all CSS, using custom properties on `:root` (`--paper`, `--ink-900`, `--cat-500`, `--dog-500`, `--future-500`, `--bonus-500`, etc. — see [Design](#design)) for a warm near-white background with near-black ink for text and flat black as the primary CTA color. Cats, dogs, future pets, and the wheel's bonus wedge each get their own muted accent color, confined to small pill/tag fills.
2. **Markup** — header/nav, hero, stats bar, feature grid, services grid, the NuzzlePal device section, how-it-works steps, an app-preview phone mockup, plans, testimonials, FAQ, a closing CTA band, and the footer.
3. **`<script>`** — a single IIFE containing all interactivity, no external JS libraries:
   - `SERVICES`, `PET_PLANS` (keyed `cat`/`dog`/`future`), `WHEEL_ORDER` (now `['cat','dog','future','bonus']`), `TESTIMONIALS`, `FAQ`, `PET_MODES`, `ROAM_LINES`, and `CHASE_LINES` are static data near the top — edit these to change copy, pricing, wheel order, or FAQ content. `bonus` is deliberately *not* a key in `PET_PLANS`; it's handled separately (see below) since a voucher isn't a pet plan.
   - `renderServices`, `renderPlanPanel`, `renderTestimonials`, `renderFaq` build their DOM section from the data above; there's no framework, so these fully render their target element once on load (`renderPlanPanel` re-renders on every wheel/tab selection instead). `renderBonusPanel` is the Bonus-wedge equivalent: it lazily generates a promo code + issued/expiry dates the first time it's called and caches them in `bonusVoucher` so repeat views show the same code, then renders a distinct dashed-border "voucher card." `renderActivePanel(petKey)` is the dispatcher that picks between the two based on whether `petKey === 'bonus'`.
   - `selectPet(petKey, extraTurns)` is the single source of truth for the plans wheel: it computes the rotation needed to land the wheel on `petKey` (adding `extraTurns` full spins for flourish), updates the tab buttons, and calls `renderActivePanel` after the CSS transition finishes. `initWheel()` wires the wheel's own click (using `Math.atan2` on the click point to figure out which wedge was clicked, compensated for the wheel's current rotation — this math is generic over `WHEEL_ORDER.length`, so it didn't need changes when Bonus was added, only the markup's `--ang` values and the CSS `conic-gradient` stops did), the SPIN button (random pet, 3–4 extra turns), and the tab buttons to all call `selectPet`.
   - `randomViewportSpot(el)` and `hopMascotTo(el, x, y)` are the shared movement primitives behind all three roaming mascots. `initRoamBot()` wires the robot's own click to a random hop plus a `ROAM_LINES` speech bubble. `initRoamCritter(id, kind)` wires the dog/cat buttons: on click they hop to a random spot, then call `chaseWithBot(spot, kind)`, which — after a short delay so it reads as reacting rather than teleporting in sync — hops the robot to a point offset from the critter's new spot and shows a `CHASE_LINES` bubble.
   - `setPetMode('cat' | 'dog')` toggles the NuzzlePal illustration's visible accessory, ring-light color, and caption.
   - `setPhoneTab('live' | 'talk' | 'insights')` swaps the phone mockup's feed image, badge, and copy.
   - The FAQ accordion, mobile nav toggle, sticky-header scroll state, and the toast helper (used by "Choose plan," "Join the Waitlist," "Copy code," and the bundle note) round out the interactivity.
   - Nothing here submits anywhere or persists across reloads — plan/waitlist/voucher buttons just show a toast or use the Clipboard API, matching the site's status as a demo/portfolio project with no backend.

## Image & video policy

Every *subject* photo and video on this site (dogs, cats, rabbits, people, and now the NuzzlePal device itself) is real, freely-licensed footage sourced from Pexels (Pexels License, free for commercial use) — never an illustration or AI-generated image/video. See credits below.

The "Meet NuzzlePal" hero image (`images/nuzzlepal-device.jpg`) is a real photo of an actual small two-wheeled home companion robot — since NuzzlePal is fictional, no photo of it specifically can exist, so (as with the Dodo Burgers site's dodo mascot) a real photo of a close analog stands in for it, cropped and lightly color/contrast-adjusted from the original, not generated. It's meant to read as "pet sized" — roughly cat/small-dog scale, evoking consumer home robots like Roomba-style vacuums or small companion bots, not a human-sized machine.

**One remaining exception**, flat single-color iconographic work rather than photorealistic subject illustration: a thin transparent SVG (`#deviceSvg`) sits directly on top of that photo to carry the *interactive* parts — the ring-light glow around the camera lens and the laser/treat-launcher callout graphics that swap with Cat/Dog mode — since those need to be real DOM elements JS can recolor and toggle, not baked into the photo. The roaming mascots (`#roamBot`, `#roamDog`, `#roamCat`) and the pet-themed background decoration (paw-print pattern + dog/cat silhouette watermarks, from CC0 SVG Repo icons — see credits below) are the site's other iconographic exceptions, both unrelated to this photo.

## Image credits

All photos via [Pexels](https://www.pexels.com), free to use under the [Pexels License](https://www.pexels.com/license/):

- `images/hero-dog.jpg` — Marcelo Chagas
- `images/cat-portrait.jpg` — Vũ Nguyễn
- `images/dog-portrait.jpg` — Helena Lopes
- `images/rabbit-portrait.jpg` — cottonbro studio
- `images/lifestyle-phone.jpg` — Andrea Piacquadio
- `images/nuzzlepal-device.jpg` — Kindel Media ("A White and Black Robot Toy"), cropped and lightly color/contrast-adjusted, used to represent NuzzlePal in the "Meet NuzzlePal" section
- `video/nuzzlepal-cam-loop.mp4` — Erik Mclean ("A white dog walking through a kitchen"), used to simulate NuzzlePal's pet-height following camera on the hero's live-cam loop

Decorative background icons via [SVG Repo](https://www.svgrepo.com), CC0 License (public domain, no attribution required, credited here anyway):
- Paw-print pattern (`body::before`) — "Paw Print Fill," SVG Repo
- Dog silhouette watermark (`#why .critter-deco`) — "Dog Silhouette In A Sitting Position," SVG Repo
- Cat silhouette watermark (`.testimonials .critter-deco`) — "Cat In Black Silhouette," SVG Repo

Source files for these three are also kept at `images/deco/*.svg` for reference, even though the page embeds them inline (recolored) rather than loading the files directly.

## A note on the AI

The FAQ is intentionally direct about how NuzzlePal's AI works: its behavior models are described as trained on publicly available pet-behavior footage, veterinary research, and other public data from across the internet, then fine-tuned per-pet on real interaction data with owner consent. This is stated plainly in the FAQ rather than buried, and the same FAQ entry is upfront that NuzzlePal is a companion, not a replacement for veterinary care or an in-person sitter for anything urgent.
