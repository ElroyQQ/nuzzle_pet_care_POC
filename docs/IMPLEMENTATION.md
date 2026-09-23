# System implementation map

> Whole-system functor architecture-map.md → code, deduced from the component
> IMPLEMENTATION.md files. System-level rows only.

## Components → code root
| Component | Code root | Model | Code map |
| --- | --- | --- | --- |
| content | `index.html` | [content/ARCHITECTURE.md](content/ARCHITECTURE.md) | [content/IMPLEMENTATION.md](content/IMPLEMENTATION.md) |
| plans-wheel | `index.html` | [plans-wheel/ARCHITECTURE.md](plans-wheel/ARCHITECTURE.md) | [plans-wheel/IMPLEMENTATION.md](plans-wheel/IMPLEMENTATION.md) |
| roaming-mascots | `index.html` | [roaming-mascots/ARCHITECTURE.md](roaming-mascots/ARCHITECTURE.md) | [roaming-mascots/IMPLEMENTATION.md](roaming-mascots/IMPLEMENTATION.md) |
| device-panel | `index.html` | [device-panel/ARCHITECTURE.md](device-panel/ARCHITECTURE.md) | [device-panel/IMPLEMENTATION.md](device-panel/IMPLEMENTATION.md) |
| compare-plans | `compare.html` | [compare-plans/ARCHITECTURE.md](compare-plans/ARCHITECTURE.md) | [compare-plans/IMPLEMENTATION.md](compare-plans/IMPLEMENTATION.md) |

## Shared objects (one Dat, DataLocs in ≥2 components)
| Object | Authoritative at | Also read by | Realised at |
| --- | --- | --- | --- |
| none | — | — | `content`, `plans-wheel`, `roaming-mascots`, `device-panel` share no `Dat`, only utility `Trn` (see below) |

`compare.html:DAYS` (plan pricing/content for the comparison page) duplicates
some of the same real-world facts as `index.html:PET_PLANS` (plan pricing) but
is a **separate object in a separate document**, not a shared `DataLoc` — see
[suggestions.md](suggestions.md) #2 for why this is flagged, not merged.

## Inter-component transmissions / ports (Trm)
None — all four `index.html` components and `compare-plans` are same-`Loc`;
nothing here is a `Trm`.

**Utility `Trn` shared without a declared port** (§4.5 Law 4, advisory): `el`
and `showToast`, both owned by `content`, are called directly by
`plans-wheel`, `roaming-mascots`, and `device-panel` — see
[suggestions.md](suggestions.md) #1.

## System entry points
| Entry | Trn triggered | Code |
| --- | --- | --- |
| `index.html` page load | `content.init()` → `renderServices`, `renderTestimonials`, `renderFaq`, `initHeader`, `initHeroVideo`, plus each component's own `init*` | `index.html:init` (line 1401) |
| `compare.html` page load | `DOMContentLoaded` → `initHeader`, `initSpeciesSwitch`, `initPlanButtons`, `initFaq`, `setSpecies('dog')` | `compare.html` (line 854) |

## Divergences (system-level)
`compare.html` duplicates `el`, `showToast`, and `initHeader` from
`index.html` rather than sharing them (there is no shared script file to
share them through — both are single self-contained documents by design, see
each project's CLAUDE.md "no build step" constraint) — not a bug, a
consequence of the single-file-per-page convention. Noted, not flagged as a
suggestion, since fixing it would require introducing a build step this repo
deliberately doesn't have.
