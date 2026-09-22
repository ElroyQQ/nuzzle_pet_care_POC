# Nuzzle — AI-Powered Remote Pet Care

A single self-contained static website for **Nuzzle**, a fictional remote pet-sitting company. Nuzzle's pitch: a live HD camera paired with **NuzzlePal**, a proprietary AI companion robot, so owners can watch, talk to, and play with their cat or dog from anywhere — with the AI keeping pets company in between check-ins.

The whole site — markup, CSS, and JavaScript — lives in one file: `index.html`, plus a flat `images/` folder of photos it references by relative path. There is no build step, no package manager, and no dependencies to install.

## Running it

Open `index.html` directly in a browser, or serve the folder (`python3 -m http.server`) to sanity-check relative image paths the way a real host would serve them. There is no dev server, bundler, linter, or test suite configured in this repo.

## Features

- **Hero** with a live-camera-style photo treatment and a floating "now playing" status card.
- **Services by species** — separate Cats and Dogs offerings with species-specific hardware/AI behavior, plus a "More friends, coming soon" card (rabbits, birds) with a waitlist button.
- **Meet NuzzlePal** — an original SVG illustration of the robot device with a live Cat/Dog mode toggle that swaps the visible accessory (laser vs. treat launcher), the status-ring color, and the caption text.
- **How it works** — a 4-step onboarding explainer.
- **App mockup** — a CSS-built phone frame with working Live / Talk / Insights tabs that swap the feed photo, badge, and copy.
- **Plans** — Cat, Dog, and Multi-Pet Bundle pricing cards; "Choose plan" triggers a toast (no real checkout, this is a demo).
- **Testimonials** and an **FAQ accordion**, including an explicit, honest FAQ entry on how the AI is trained (see below).
- Responsive layout with a mobile hamburger nav; sticky, blurred header.

## Architecture

Everything is inline in `index.html`, in three blocks in order:

1. **`<style>`** — all CSS, using custom properties on `:root` (`--paper`, `--navy-700`, `--coral-500`, `--sage-500`, `--amber-500`, `--violet-500`, etc.) for a warm cream background with navy as the brand/trust color and coral as the CTA color. Cats, dogs, and "future pets" each get their own accent tag color (violet / amber / sage).
2. **Markup** — header/nav, hero, stats bar, feature grid, services grid, the NuzzlePal device section, how-it-works steps, an app-preview phone mockup, plans, testimonials, FAQ, a closing CTA band, and the footer.
3. **`<script>`** — a single IIFE containing all interactivity, no external JS libraries:
   - `SERVICES`, `PLANS`, `TESTIMONIALS`, `FAQ`, and `PET_MODES` are static data arrays/objects near the top — edit these to change copy, pricing, or FAQ content.
   - `renderServices`, `renderPlans`, `renderTestimonials`, `renderFaq` build their DOM section from the data above; there's no framework, so these fully render their target element once on load.
   - `setPetMode('cat' | 'dog')` toggles the NuzzlePal illustration's visible accessory, ring-light color, and caption.
   - `setPhoneTab('live' | 'talk' | 'insights')` swaps the phone mockup's feed image, badge, and copy.
   - The FAQ accordion, mobile nav toggle, sticky-header scroll state, and the toast helper (used by "Choose plan" and "Join the waitlist" buttons) round out the interactivity.
   - Nothing here submits anywhere or persists — plan/waitlist buttons just show a toast, matching the site's status as a demo/portfolio project.

## Image policy

Every photo on this site is a real, freely-licensed photograph sourced from Pexels (Pexels License, free for commercial use) — never an illustration or AI-generated image. See credits below.

**One exception**: the NuzzlePal device graphic in the "Meet NuzzlePal" section is an original SVG illustration, not a photo. Since NuzzlePal is a fictional proprietary product, no real photo of it can exist — the illustration is drawn flat-style in the brand palette and redraws its accessory (laser vs. treat launcher) and ring-light color based on the selected Cat/Dog mode.

## Image credits

All photos via [Pexels](https://www.pexels.com), free to use under the [Pexels License](https://www.pexels.com/license/):

- `images/hero-dog.jpg` — Marcelo Chagas
- `images/cat-portrait.jpg` — Vũ Nguyễn
- `images/dog-portrait.jpg` — Helena Lopes
- `images/rabbit-portrait.jpg` — cottonbro studio
- `images/lifestyle-phone.jpg` — Andrea Piacquadio

## A note on the AI

The FAQ is intentionally direct about how NuzzlePal's AI works: its behavior models are described as trained on publicly available pet-behavior footage, veterinary research, and other public data from across the internet, then fine-tuned per-pet on real interaction data with owner consent. This is stated plainly in the FAQ rather than buried, and the same FAQ entry is upfront that NuzzlePal is a companion, not a replacement for veterinary care or an in-person sitter for anything urgent.
