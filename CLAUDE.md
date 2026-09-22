# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single self-contained static website for "Nuzzle," a fictional remote pet-sitting company: a live camera plus a proprietary AI companion robot ("NuzzlePal") for cats and dogs, with more species planned. The whole site — markup, CSS, and JavaScript — lives in one file: `index.html`, plus a flat `images/` folder of photos it references by relative path. There is no build step, no package manager, and no dependencies to install. See [README.md](README.md) for the full feature list and image credits.

## Running it

Open `index.html` directly in a browser, or serve the folder (`python3 -m http.server`) to sanity-check relative image paths. There is no dev server, bundler, linter, or test suite configured in this repo.

## Architecture

Everything is inline in `index.html`:

1. **`<style>`** — CSS custom properties on `:root` (`--paper`, `--navy-700`, `--coral-500`, plus per-species accents `--violet-500` for cats, `--amber-500` for dogs, `--sage-500` for future/other pets).
2. **Markup** — header/nav, hero, stats bar, feature grid, services grid (cats/dogs/future pets), the NuzzlePal device section, how-it-works steps, an app-preview phone mockup, a spinnable plans wheel, testimonials, FAQ, closing CTA, footer. Three fixed roaming mascot buttons (`#roamBot`, `#roamDog`, `#roamCat`) sit outside `<main>`, right before the closing `</body>`.
3. **`<script>`** — a single IIFE, no external JS libraries. `SERVICES`, `PET_PLANS` (keyed `cat`/`dog`/`future` — deliberately no `bonus` key), `WHEEL_ORDER` (`['cat','dog','future','bonus']`), `TESTIMONIALS`, `FAQ`, `PET_MODES`, `ROAM_LINES`, and `CHASE_LINES` are the data to edit for copy changes. `renderServices`/`renderPlanPanel`/`renderTestimonials`/`renderFaq` build their sections from that data; `renderBonusPanel` builds the voucher panel separately, and `renderActivePanel(petKey)` dispatches between the two. `selectPet(petKey, extraTurns)` is the shared handler behind the plans wheel, its SPIN button, and its tab buttons — it does the rotation math (see below) and calls `renderActivePanel` after the spin animation. `initRoamBot()`/`initRoamCritter()` drive the roaming mascots (see below). `setPetMode()` drives the Cat/Dog toggle on the NuzzlePal illustration (swaps accessory, ring-light color, caption). `setPhoneTab()` drives the Live/Talk/Insights app mockup. Nothing submits anywhere — "Choose plan," "Join the Waitlist," "Copy code," and the bundle-note link just show a toast (or use the Clipboard API), consistent with this being a demo/portfolio site with no backend.

## The plans wheel

`#wheel` is a plain circular `<div>` colored with a CSS `conic-gradient` (four 90° wedges: cat=violet, dog=amber, future=sage, bonus=coral) — there is no SVG arc math. `selectPet()` keeps a running `wheelRotation` value (in degrees, can exceed 360) and always rotates *forward* to whatever multiple of 360 lands the target wedge's center under the fixed pointer at the top, so repeated selections never spin backwards. `initWheel()`'s click handler converts a click's pixel position into a compass angle with `Math.atan2`, compensates for the wheel's current rotation, and figures out which wedge was actually clicked — the math is generic over `WHEEL_ORDER.length`, so it didn't need to change when the Bonus wedge was added; only the four `conic-gradient` stops and the four `.wheel-label` elements' `--ang` values (45/135/225/315deg, i.e. `index*90 + 45`) needed updating. On page load, `initWheel()` snaps the wheel to the default active pet's position with `transition:none` first, so the wheel doesn't visibly spin on first paint.

The **Bonus** wedge doesn't map to a `PET_PLANS` entry — `selectPet()`'s validity check is `WHEEL_ORDER.indexOf(petKey) !== -1`, not a `PET_PLANS` lookup, specifically so `bonus` is a legal wheel value with no plan behind it. `renderBonusPanel()` lazily creates a `{code, issued, expires}` object on first view (`bonusVoucher`, module-level, generated once and reused — clicking away and back shows the same code rather than re-rolling it) using `generatePromoCode()` (excludes visually ambiguous characters like `0/O`, `1/I`) and `addOneMonth()` (uses `Date#setMonth`, which correctly rolls over year/day-count edge cases). If you change the voucher's validity window, edit `addOneMonth` — don't hardcode "30 days," since the copy explicitly says "1 month."

## Roaming mascots

`randomViewportSpot(el)` and `hopMascotTo(el, x, y)` are shared by all three mascots: pick a random point within the viewport (16px margin, sized off the element's own `offsetWidth`) and move the button there via inline `left`/`top` (clearing the CSS `right`/`bottom` defaults first), replaying the `bot-hop` keyframe. `initRoamBot()` wires the robot's own click to that plus a `ROAM_LINES` bubble. `initRoamCritter(id, kind)` wires `#roamDog`/`#roamCat`: on click, the critter hops, then `chaseWithBot(spot, kind)` fires — after a ~450ms delay (so it visibly reads as the robot *reacting*, not moving in lockstep) it hops the robot to a random point offset ~80px from the critter's new spot and shows a `CHASE_LINES[kind]` bubble. If you add a fourth roaming mascot, reuse these two primitives rather than duplicating the hop logic again.

## Image & video policy

Every photo and video on this site must be real, freely-licensed footage — never an illustration, drawing, or AI-generated image/video. Source from Pexels (Pexels License) or another clearly free/CC0 source, and record credits in the footer of `index.html` and in [README.md](README.md#image-credits) when adding new sourced images or video.

The hero's "LIVE · NUZZLEPAL CAM" background (`video/nuzzlepal-cam-loop.mp4`) is real Pexels footage of a dog walking through a home at floor level, chosen specifically to read as a pet-height camera following the animal around — not a generated or staged "robot POV" shot. If this is swapped, keep sourcing from real footage with a similar low, following angle rather than a generic dog video, or the "NuzzlePal is watching" framing stops making sense.

**Two explicit exceptions**, both flat single-color iconographic work, not photorealistic subject illustration:
1. The NuzzlePal device graphic in the "Meet NuzzlePal" section (`#deviceSvg`) and the three roaming mascots (`#roamBot`, `#roamDog`, `#roamCat`) are original, hand-built SVG illustrations, not photos. NuzzlePal is a fictional proprietary product, so no real photo of it can exist — this is the site's original carve-out from the photo-only policy, the same way a fictional-product mockup would be illustrated on any real SaaS marketing site. The dog/cat mascots extend this same carve-out (small, flat, UI-icon-scale mascots, not photo-replacement subject illustrations) — they're stylized to match the robot's proportions and use the same violet/amber accent colors as the cat/dog wheel wedges and service tags.
2. The pet-themed background decoration — the repeating paw-print pattern on `body::before` and the large dog/cat silhouette watermarks (`.critter-deco` in `#why` and `.testimonials`) — was added on explicit request to make the site feel more "pet themed," using CC0-licensed flat SVG icons from SVG Repo (see README credits), recolored to brand navy at low opacity (`fill-opacity` around 0.03–0.08 in the inline SVGs, `opacity:.07` on `.critter-deco`). Source files also live at `images/deco/*.svg` for reference.

Don't treat either exception as license to introduce photorealistic illustrations elsewhere — hero, service cards, testimonials, etc. should stay real photos. If asked to add more decorative background art, keep it in the same family (flat, low-opacity, CC0-sourced, recolored to the brand palette) rather than introducing a different illustration style.

## Content notes

- The FAQ item "How smart is the AI, really?" is intentionally direct: NuzzlePal's models are described as trained on publicly available pet-behavior footage and other public internet data, then fine-tuned per-pet with owner consent — and explicitly *not* a substitute for veterinary care or an in-person sitter. Keep this framing if the FAQ is edited; it was a deliberate choice to be upfront rather than vague marketing-speak about "proprietary AI."
- Pricing, stats (e.g. "11,400+ homes"), and testimonials are illustrative placeholder content for a demo site, not real figures.
- All three roaming mascots (`#roamBot`, `#roamDog`, `#roamCat`) are `position:fixed` and move within the viewport only (not the full scrollable document) — this was a deliberate simplification so none of them can wander somewhere the visitor has to scroll to find, which read as broken rather than playful during testing.
- Pricing/stats/testimonial placeholder-content note above also covers the voucher: the promo code and dates are real, freshly computed values (not hardcoded strings), but the "10% off" discount itself isn't wired to anything — there's no cart or checkout on this site for it to apply to.
