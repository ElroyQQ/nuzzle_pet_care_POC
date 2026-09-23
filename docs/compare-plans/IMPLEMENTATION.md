# Compare plans — implementation map

> The functor ARCHITECTURE.md → code. Source: `compare.html` (uncommitted at
> scaffold time — see ARCHITECTURE.md header note).

## Objects (Dat) → code
| Object | Form / shape | Realised at | State |
| --- | --- | --- | --- |
| `DAYS` | `{dog,cat}` each `{price,period,planName,ctaText,moments[]}` | `compare.html:DAYS` (line 643) | built |
| `currentSpecies` | `'dog'\|'cat'` | `compare.html:currentSpecies` (line 712) | built |
| `currentMoment` | `number` | `compare.html:currentMoment` (line 713) | built |

## Morphisms (Trn / relations) → code
| Morphism | Signature | Realising code | State |
| --- | --- | --- | --- |
| `renderMomentTabs` | `() → DOM` | `compare.html:renderMomentTabs` (line 729) | built |
| `applyMoment` | `index → DOM` | `compare.html:applyMoment` (line 743) | built |
| `setSpecies` | `species → DOM` | `compare.html:setSpecies` (line 788) | built |
| `initSpeciesSwitch` | `() → DOM listener` | `compare.html:initSpeciesSwitch` (line 807) | built |
| `initPlanButtons` | `() → DOM listener` | `compare.html:initPlanButtons` (line 813) | built |
| `initFaq` | `() → DOM listener` | `compare.html:initFaq` (line 823) | built |
| `el`, `showToast`, `initHeader` | duplicated from `index.html`, not shared | `compare.html:el`/`showToast`/`initHeader` (lines 715, 721, 841) | built |

## Composition rules → where enforced
| Rule (ARCHITECTURE §6) | Enforced at | Tested at |
| --- | --- | --- |
| `currentMoment` resets on species switch | `compare.html:setSpecies` | no automated tests |
| `moment.kind ∈ {'img','video'}` branch | `compare.html:applyMoment` | no automated tests |

## Notes / divergences
Duplicates `el`/`showToast`/`initHeader` from `index.html` rather than
sharing — consequence of the no-build-step, single-file-per-page convention
(see `docs/IMPLEMENTATION.md` "Divergences"), not a bug.
