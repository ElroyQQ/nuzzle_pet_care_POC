# Compare plans — status

> Reconciles ARCHITECTURE.md (intent) vs IMPLEMENTATION.md (code).

## Headline
Code-complete and wired (species switch, moment timeline, FAQ, plan buttons
all functional) — but **not yet committed to git** as of this scaffold
(2026-09-23). That's a git-status fact, not a code gap; re-verify before
relying on this STATUS if picked up later, since it may have changed.

## Completeness
| Object / morphism | State | Notes |
| --- | --- | --- |
| `setSpecies` / `renderMomentTabs` / `applyMoment` | ✅ built | |
| `initSpeciesSwitch` / `initPlanButtons` / `initFaq` | ✅ built | |

## Needs work
1. Not committed to git — the working-tree state at scaffold time
   (`git status`: `?? compare.html`) should be resolved (commit, or discard if
   abandoned) before this STATUS can be trusted long-term.
2. Pricing (`DAYS`) duplicates `index.html:PET_PLANS` independently — see
   `docs/suggestions.md` #2.

## Coherence
No §4.5 law FAILing.

## Where to dig
- Model: `ARCHITECTURE.md` · Code map: `IMPLEMENTATION.md`
- In flight: `openspec/changes/` (none as of scaffold) · Reviews: `reviews/` · Notes: `general/`
