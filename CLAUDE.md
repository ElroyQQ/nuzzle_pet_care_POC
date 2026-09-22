# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single self-contained static website for "Nuzzle," a fictional remote pet-sitting company: a live camera plus a proprietary AI companion robot ("NuzzlePal") for cats and dogs, with more species planned. The whole site — markup, CSS, and JavaScript — lives in one file: `index.html`, plus a flat `images/` folder of photos it references by relative path. There is no build step, no package manager, and no dependencies to install. See [README.md](README.md) for the full feature list and image credits.

## Running it

Open `index.html` directly in a browser, or serve the folder (`python3 -m http.server`) to sanity-check relative image paths. There is no dev server, bundler, linter, or test suite configured in this repo.

## Architecture

Everything is inline in `index.html`:

1. **`<style>`** — CSS custom properties on `:root` (`--paper`, `--navy-700`, `--coral-500`, plus per-species accents `--violet-500` for cats, `--amber-500` for dogs, `--sage-500` for future/other pets).
2. **Markup** — header/nav, hero, stats bar, feature grid, services grid (cats/dogs/future pets), the NuzzlePal device section, how-it-works steps, an app-preview phone mockup, plans, testimonials, FAQ, closing CTA, footer.
3. **`<script>`** — a single IIFE, no external JS libraries. `SERVICES`, `PLANS`, `TESTIMONIALS`, `FAQ`, and `PET_MODES` are the data arrays/objects to edit for copy changes. `renderServices`/`renderPlans`/`renderTestimonials`/`renderFaq` build their sections from that data on load. `setPetMode()` drives the Cat/Dog toggle on the NuzzlePal illustration (swaps accessory, ring-light color, caption). `setPhoneTab()` drives the Live/Talk/Insights app mockup. Nothing submits anywhere — "Choose plan" and "Join the waitlist" just show a toast, consistent with this being a demo/portfolio site with no backend.

## Image & video policy

Every photo and video on this site must be real, freely-licensed footage — never an illustration, drawing, or AI-generated image/video. Source from Pexels (Pexels License) or another clearly free/CC0 source, and record credits in the footer of `index.html` and in [README.md](README.md#image-credits) when adding new sourced images or video.

The hero's "LIVE · NUZZLEPAL CAM" background (`video/nuzzlepal-cam-loop.mp4`) is real Pexels footage of a dog walking through a home at floor level, chosen specifically to read as a pet-height camera following the animal around — not a generated or staged "robot POV" shot. If this is swapped, keep sourcing from real footage with a similar low, following angle rather than a generic dog video, or the "NuzzlePal is watching" framing stops making sense.

**One explicit exception**: the NuzzlePal device graphic in the "Meet NuzzlePal" section (`#deviceSvg`) is an original, hand-built SVG illustration, not a photo. NuzzlePal is a fictional proprietary product, so no real photo of it can exist — this is the site's one intentional carve-out from the photo-only policy, the same way a fictional-product mockup would be illustrated on any real SaaS marketing site. Don't treat this as license to introduce illustrations elsewhere (hero, service cards, testimonials, etc. should stay real photos).

## Content notes

- The FAQ item "How smart is the AI, really?" is intentionally direct: NuzzlePal's models are described as trained on publicly available pet-behavior footage and other public internet data, then fine-tuned per-pet with owner consent — and explicitly *not* a substitute for veterinary care or an in-person sitter. Keep this framing if the FAQ is edited; it was a deliberate choice to be upfront rather than vague marketing-speak about "proprietary AI."
- Pricing, stats (e.g. "11,400+ homes"), and testimonials are illustrative placeholder content for a demo site, not real figures.
