# Device panel — status

> Reconciles ARCHITECTURE.md (intent) vs IMPLEMENTATION.md (code).

## Headline
Built and complete.

## Completeness
| Object / morphism | State | Notes |
| --- | --- | --- |
| `setPetMode` | ✅ built | |

## Needs work
1. `#ringLight`'s measured `cx`/`cy` constants have no code-level assertion
   tying them to `images/nuzzlepal-device.jpg`'s dimensions — purely
   documentation-enforced (ARCHITECTURE.md §6). Not currently planned to
   change; flagged for visibility if the photo is ever swapped.

## Coherence
No §4.5 law FAILing.

## Where to dig
- Model: `ARCHITECTURE.md` · Code map: `IMPLEMENTATION.md`
- In flight: `openspec/changes/` (none as of scaffold) · Reviews: `reviews/` · Notes: `general/`
