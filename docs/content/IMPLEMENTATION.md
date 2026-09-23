# Content — implementation map

> The functor ARCHITECTURE.md → code. Each object/morphism → the file:symbol
> that realises it. Keep in sync WITH the code (§6.3).

## Objects (Dat) → code
| Object | Form / shape | Realised at | State |
| --- | --- | --- | --- |
| `Service[]` | array of service cards | `index.html:SERVICES` (line 875) | built |
| `Testimonial[]` | array | `index.html:TESTIMONIALS` (line 974) | built |
| `FaqEntry[]` | `{q,a}[]` | `index.html:FAQ` (line 980) | built |

## Morphisms (Trn / relations) → code
| Morphism | Signature | Realising code | State |
| --- | --- | --- | --- |
| `renderServices` | `Service[] → DOM` | `index.html:renderServices` (line 1007) | built |
| `renderTestimonials` | `Testimonial[] → DOM` | `index.html:renderTestimonials` (line 1280) | built |
| `renderFaq` | `FaqEntry[] → DOM` | `index.html:renderFaq` (line 1296) | built |
| `setPhoneTab` | `tab → DOM` | `index.html:setPhoneTab` (line 1343) | built |
| `el` | `html → Element` | `index.html:el` (line 1001) | built |
| `showToast` | `msg → DOM` | `index.html:showToast` (line 1368) | built |
| `initHeader` | `() → DOM` | `index.html:initHeader` (line 1376) | built |
| `initHeroVideo` | `() → DOM` | `index.html:initHeroVideo` (line 1389) | built |
| `init` | `() → ()` | `index.html:init` (line 1401) | built |

## Composition rules → where enforced
None specific to this component.

## Notes / divergences
None found at scaffold time (2026-09-23).
