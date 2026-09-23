# 2026-09-23 — supercharge docs-tree scaffold (init)

## Decisions made
- Decomposed `index.html` into **four components** by feature seam (not by
  `Loc`, since everything is one browser tab): `content` (services,
  testimonials, FAQ, phone-preview tabs, plus shared utilities `el`/
  `showToast`/`initHeader`/`initHeroVideo`/`init`), `plans-wheel` (the
  spinnable plan picker + bonus voucher), `roaming-mascots` (the three hopping
  UI easter eggs), `device-panel` (the NuzzlePal Cat/Dog mode toggle). This
  matches `CLAUDE.md`'s own section boundaries ("The plans wheel", "Roaming
  mascots"), which is the code's own vocabulary, not an invented taxonomy.
- Added a **fifth component**, `compare-plans`, for `compare.html` — a
  separate document (real code-root boundary), currently **uncommitted** in
  git (`?? compare.html` in `git status`) but confirmed code-complete by
  reading it in full. Modeled as built, with the uncommitted-git-status fact
  flagged in its `STATUS.md` rather than treated as an architecture gap.
- `graphify`'s code-only extraction produced an **empty graph** here too (same
  reason as dodo-burgers — the JS lives inline in `.html` files, no
  `.js`/`.py`/etc. for its grammars to parse). Decomposition was done by
  reading `CLAUDE.md`, then grepping `index.html`/`compare.html` for exact
  `function`/`var` declarations and line numbers to ground every
  `file:symbol` reference.

## Kept / discarded
- Kept `CLAUDE.md`, `README.md`, `PRODUCT.md`, and the in-progress uncommitted
  changes (`index.html`, `compare.html`, `.impeccable/`) completely untouched
  — this docs-tree is additive only.
- Considered folding `device-panel` into `content` (it's tiny — one function,
  one data object) but kept it separate per §3: it's genuinely a different
  `Dat`/`Trn` pair with its own composition rule (the photo-pixel-coupling
  constraint on `#ringLight`), not "the same objects, different morphisms" as
  anything in `content`.

## Open ends
- **`compare.html` is uncommitted.** Resolve that (commit or discard) before
  trusting `compare-plans/STATUS.md` long-term — it was accurate as of this
  scaffold but git state can change independently of this doc.
- Pricing duplication between `compare.html:DAYS` and `index.html:PET_PLANS`
  recorded in `docs/suggestions.md` #2 — not fixed, since there's no shared
  module to fix it into without introducing a build step this repo
  deliberately doesn't have.
- Cross-component utility calls (`el`/`showToast` called directly, no
  declared port) recorded in `docs/suggestions.md` #1 — advisory only, not a
  real problem at single-file scale.

## Live execution state
None — this was a docs-only scaffold. No servers, jobs, or generated
artifacts outside `docs/`.

## Resume commands
```bash
cd nuzzle-pet-care
git status                     # re-check compare.html's commit state first
openspec list --json           # confirm no in-flight changes exist
cat docs/STATUS.md             # system status roll-up
cat docs/compare-plans/STATUS.md  # the one component whose git state matters
```

Next `start` should read this file plus `docs/STATUS.md`, then re-run
`git status` before trusting `compare-plans`'s STATUS, since that's the one
component whose real-world state (committed or not) can drift independently
of this docs-tree.
