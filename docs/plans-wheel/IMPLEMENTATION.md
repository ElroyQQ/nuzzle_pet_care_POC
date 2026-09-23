# Plans wheel — implementation map

> The functor ARCHITECTURE.md → code. Each object/morphism → the file:symbol
> that realises it. Keep in sync WITH the code (§6.3).

## Objects (Dat) → code
| Object | Form / shape | Realised at | State |
| --- | --- | --- | --- |
| `WHEEL_ORDER` | `['cat','dog','future','bonus']` | `index.html:WHEEL_ORDER` (line 924) | built |
| `PET_PLANS` | `{cat,dog,future}` (no `bonus` key) | `index.html:PET_PLANS` (line 926) | built |
| `bonusVoucher` | `{code,issued,expires}` or `null` | `index.html:bonusVoucher` (line 1057) | built |
| `wheelRotation` | `number`, monotonically increasing | `index.html:wheelRotation` (line 1115) | built |

## Morphisms (Trn / relations) → code
| Morphism | Signature | Realising code | State |
| --- | --- | --- | --- |
| `renderPlanPanel` | `petKey → DOM` | `index.html:renderPlanPanel` (line 1033) | built |
| `generatePromoCode` | `() → string` | `index.html:generatePromoCode` (line 1059) | built |
| `addOneMonth` | `date → date` | `index.html:addOneMonth` (line 1066) | built |
| `formatDate` | `date → string` | `index.html:formatDate` (line 1076) | built |
| `renderBonusPanel` | `() → DOM` | `index.html:renderBonusPanel` (line ~1076) | built |
| `renderActivePanel` | `petKey → DOM` | `index.html:renderActivePanel` (line 1110) | built |
| `updateTabsUI` | `petKey → DOM` | `index.html:updateTabsUI` (line 1118) | built |
| `selectPet` | `(petKey, extraTurns?) → ()` | `index.html:selectPet` (line 1124) | built |
| `initWheel` | `() → DOM listener` | `index.html:initWheel` (line 1151) | built |

## Composition rules → where enforced
| Rule (ARCHITECTURE §6) | Enforced at | Tested at |
| --- | --- | --- |
| `wheelRotation` always increases | `index.html:selectPet` | no automated tests — repo has no test suite; verified manually per CLAUDE.md session history point 12 |
| `bonusVoucher.expires = addOneMonth(issued)` | `index.html:renderBonusPanel` | no automated tests |
| `bonus ∈ WHEEL_ORDER, ∉ PET_PLANS` | `index.html:selectPet`'s `WHEEL_ORDER.indexOf` check | no automated tests |

## Notes / divergences
None found at scaffold time (2026-09-23). No test suite exists in this repo.
