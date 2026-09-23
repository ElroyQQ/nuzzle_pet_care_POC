# Roaming mascots — status

> Reconciles ARCHITECTURE.md (intent) vs IMPLEMENTATION.md (code).

## Headline
Built and complete — all three mascots hop and chase correctly; species
legibility (dog/cat icon redesign) went through two failed revisions before
landing on the current face-only design (see CLAUDE.md).

## Completeness
| Object / morphism | State | Notes |
| --- | --- | --- |
| `initRoamBot` / `initRoamCritter` / `chaseWithBot` | ✅ built | |
| `randomViewportSpot` / `hopMascotTo` | ✅ built | viewport-only, by design |

## Needs work
None.

## Coherence
No §4.5 law FAILing.

## Where to dig
- Model: `ARCHITECTURE.md` · Code map: `IMPLEMENTATION.md`
- In flight: `openspec/changes/` (none as of scaffold) · Reviews: `reviews/` · Notes: `general/`
