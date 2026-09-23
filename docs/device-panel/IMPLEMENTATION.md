# Device panel — implementation map

> The functor ARCHITECTURE.md → code.

## Objects (Dat) → code
| Object | Form / shape | Realised at | State |
| --- | --- | --- | --- |
| `PET_MODES` | `{cat,dog}` (accessory, ring color, caption) | `index.html:PET_MODES` (line 990) | built |

## Morphisms (Trn / relations) → code
| Morphism | Signature | Realising code | State |
| --- | --- | --- | --- |
| `setPetMode` | `mode → DOM` | `index.html:setPetMode` (line 1324) | built |

## Composition rules → where enforced
| Rule (ARCHITECTURE §6) | Enforced at | Tested at |
| --- | --- | --- |
| `#ringLight` cx/cy tied to `images/nuzzlepal-device.jpg`'s exact dimensions | not enforced in code — documentation only (`CLAUDE.md`, `ARCHITECTURE.md` §6) | no automated tests |

## Notes / divergences
None found at scaffold time (2026-09-23).
