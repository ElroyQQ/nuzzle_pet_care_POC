# Nuzzle — AI-Powered Remote Pet Care

A single self-contained static website for **Nuzzle**, a fictional remote pet-sitting company. Nuzzle's pitch: a live HD camera paired with **NuzzlePal**, a proprietary AI companion robot, so owners can watch, talk to, and play with their cat or dog from anywhere — with the AI keeping pets company in between check-ins.

The whole site — markup, CSS, and JavaScript — lives in one file: `index.html`, plus a flat `images/` folder of photos and a `video/` folder holding one looping clip, both referenced by relative path. There is no build step, no package manager, and no dependencies to install.

## Running it

Open `index.html` directly in a browser, or serve the folder (`python3 -m http.server`) to sanity-check relative image paths the way a real host would serve them. There is no dev server, bundler, linter, or test suite configured in this repo.

## Features

- **Hero** with a looping floor-level video ("LIVE · NUZZLEPAL CAM") showing a dog walking through a home from a pet-height, following-camera POV, plus a floating "now playing" status card.
- **Services by species** — separate Cats and Dogs offerings with species-specific hardware/AI behavior, plus a "More friends, coming soon" card (rabbits, birds) with a waitlist button.
- **Meet NuzzlePal** — an original SVG illustration of the robot device with a live Cat/Dog mode toggle that swaps the visible accessory (laser vs. treat launcher), the status-ring color, and the caption text.
- **How it works** — a 4-step onboarding explainer.
- **App mockup** — a CSS-built phone frame with working Live / Talk / Insights tabs that swap the feed photo, badge, and copy.
- **Plans wheel** — a spinnable, carnival-style wheel (Cats / Dogs / Future Pets / **Bonus**) built with a CSS `conic-gradient`. Spin it, click a wedge directly, or use the plain-text tabs — all three stay in sync and swap a single plan-detail panel below. Includes a Future Pets waitlist plan alongside Cat and Dog pricing, a small link to the Multi-Pet Bundle, and a Bonus wedge that issues a one-time 10%-off voucher (a randomly generated code, valid for exactly one month from the moment it's issued — the code is generated once per page load and stays the same on repeat visits to the tab).
- **Roaming mascots** — a small fixed NuzzlePal-style robot in the corner of the page, plus a dog and a cat mascot elsewhere on screen. Click the robot and it hops to a new random spot with a bounce animation and a speech-bubble line. Click the dog or cat and *it* hops away — the robot then "notices" and hops over near wherever it landed a beat later, like it's chasing them.
- Pet-themed decorative background: a faint site-wide paw-print pattern plus a large dog silhouette watermark in "Why Nuzzle" and a cat silhouette watermark in the testimonials section (see Image & video policy below).
- **Testimonials** and an **FAQ accordion**, including an explicit, honest FAQ entry on how the AI is trained (see below).
- Responsive layout with a mobile hamburger nav; sticky, blurred header.

## Architecture

Everything is inline in `index.html`, in three blocks in order:

1. **`<style>`** — all CSS, using custom properties on `:root` (`--paper`, `--navy-700`, `--coral-500`, `--sage-500`, `--amber-500`, `--violet-500`, etc.) for a warm cream background with navy as the brand/trust color and coral as the CTA color. Cats, dogs, and "future pets" each get their own accent tag color (violet / amber / sage).
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

Every *subject* photo and video on this site (dogs, cats, rabbits, people) is real, freely-licensed footage sourced from Pexels (Pexels License, free for commercial use) — never an illustration or AI-generated image/video. See credits below.

**Two explicit exceptions**, both flat, single-color decorative/iconographic work rather than photorealistic subject illustration:
1. The NuzzlePal device graphic in the "Meet NuzzlePal" section is an original SVG illustration, not a photo. Since NuzzlePal is a fictional proprietary product, no real photo of it can exist — the illustration is drawn flat-style in the brand palette and redraws its accessory (laser vs. treat launcher) and ring-light color based on the selected Cat/Dog mode. The small roaming robot mascot (`#roamBot`, near the end of `<body>`) reuses the same visual language at a smaller scale.
2. The pet-themed background decoration — a repeating paw-print pattern (`body::before`) and the large dog/cat silhouette watermarks in the "Why Nuzzle" and testimonials sections (`.critter-deco`) — uses CC0-licensed flat SVG icons from SVG Repo (see credits below), recolored to the brand navy at low opacity. This is a deliberate, requested exception to the photo-only policy for site-wide decorative theming; it should stay confined to low-opacity background texture, not become new photo-replacement content elsewhere.

## Image credits

All photos via [Pexels](https://www.pexels.com), free to use under the [Pexels License](https://www.pexels.com/license/):

- `images/hero-dog.jpg` — Marcelo Chagas
- `images/cat-portrait.jpg` — Vũ Nguyễn
- `images/dog-portrait.jpg` — Helena Lopes
- `images/rabbit-portrait.jpg` — cottonbro studio
- `images/lifestyle-phone.jpg` — Andrea Piacquadio
- `video/nuzzlepal-cam-loop.mp4` — Erik Mclean ("A white dog walking through a kitchen"), used to simulate NuzzlePal's pet-height following camera on the hero's live-cam loop

Decorative background icons via [SVG Repo](https://www.svgrepo.com), CC0 License (public domain, no attribution required, credited here anyway):
- Paw-print pattern (`body::before`) — "Paw Print Fill," SVG Repo
- Dog silhouette watermark (`#why .critter-deco`) — "Dog Silhouette In A Sitting Position," SVG Repo
- Cat silhouette watermark (`.testimonials .critter-deco`) — "Cat In Black Silhouette," SVG Repo

Source files for these three are also kept at `images/deco/*.svg` for reference, even though the page embeds them inline (recolored) rather than loading the files directly.

## A note on the AI

The FAQ is intentionally direct about how NuzzlePal's AI works: its behavior models are described as trained on publicly available pet-behavior footage, veterinary research, and other public data from across the internet, then fine-tuned per-pet on real interaction data with owner consent. This is stated plainly in the FAQ rather than buried, and the same FAQ entry is upfront that NuzzlePal is a companion, not a replacement for veterinary care or an in-person sitter for anything urgent.
