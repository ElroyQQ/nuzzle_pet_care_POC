# Roaming mascots — implementation map

> The functor ARCHITECTURE.md → code. Each object/morphism → the file:symbol
> that realises it.

## Objects (Dat) → code
| Object | Form / shape | Realised at | State |
| --- | --- | --- | --- |
| `ROAM_LINES` | `string[]` | `index.html:ROAM_LINES` (line 1186) | built |
| `CHASE_LINES` | `{dog,cat}` | `index.html:CHASE_LINES` (line 1191) | built |

## Morphisms (Trn / relations) → code
| Morphism | Signature | Realising code | State |
| --- | --- | --- | --- |
| `randomViewportSpot` | `Element → {x,y}` | `index.html:randomViewportSpot` (line 1193) | built |
| `hopMascotTo` | `(Element,x,y) → DOM` | `index.html:hopMascotTo` (line 1205) | built |
| `showBubble` | `(bubble,text) → DOM` | `index.html:showBubble` (line 1215) | built |
| `chaseWithBot` | `(spot,kind) → DOM` | `index.html:chaseWithBot` (line 1223) | built |
| `initRoamBot` | `() → DOM listener` | `index.html:initRoamBot` (line 1240) | built |
| `initRoamCritter` | `(id,kind) → DOM listener` | `index.html:initRoamCritter` (line 1251) | built |
| `initMascotVisibility` | `() → DOM` | `index.html:initMascotVisibility` (line 1264) | built |

## Composition rules → where enforced
| Rule (ARCHITECTURE §6) | Enforced at | Tested at |
| --- | --- | --- |
| viewport-only movement | `index.html:randomViewportSpot` | no automated tests — repo has no test suite |

## Notes / divergences
None found at scaffold time (2026-09-23).
