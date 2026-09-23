# Whole-system categorical map (Dat/Trn/Loc/Trm)

> Top-level architecture doc (§4). Names the four atoms, lists components (each
> linking to its ARCHITECTURE.md), reifies placement where it is a relation, and
> runs the §4.5 coherence checklist against the code. Detail lives in the linked
> component docs. Source of record: `index.html`, `compare.html`.

## 1. Why
Everything here runs in one browser tab with no backend, same as dodo-burgers —
but unlike that site, this one has several genuinely distinct feature seams
(the plans wheel, roaming mascots, the NuzzlePal device panel) each with their
own data and behavior, plus a second page (`compare.html`) still mid-build.
Modeling it as several components, rather than one, is what makes "what talks
to what" checkable instead of a 900-line wall of one file's logic.

## 2. The four atoms (at a glance)
**Dat** — `SERVICES`, `PET_PLANS`, `WHEEL_ORDER`, `TESTIMONIALS`, `FAQ`,
`PET_MODES`, `ROAM_LINES`, `CHASE_LINES` (all static, const); `bonusVoucher`,
`wheelRotation` (mutable, module-level) — all in `index.html:<script>`. A
parallel, currently-uncommitted `DAYS` dataset lives in `compare.html:<script>`.

**Trn** — render functions per component (below); shared utilities `el`,
`showToast`, `initHeader`, `initHeroVideo`, `init` (all `index.html`, owned by
the `content` component as the most general one).

**Loc** — one: the browser tab. Collapses to a single process (§7.1) for both
pages — there is no server-side `Loc`. `index.html` and `compare.html` are two
separate documents but the same kind of `Loc` (a static page load), not two
physical sites.

**Trm** — none. No network request exists in either file's JS; "Choose plan,"
"Join the Waitlist," and "Copy code" only show a toast or use the Clipboard API.

## 3. Components
| Component | Owned `Trn` | Built/active when | Doc |
| --- | --- | --- | --- |
| `content` | `renderServices`, `renderTestimonials`, `renderFaq`, `setPhoneTab`, `el`, `showToast`, `initHeader`, `initHeroVideo`, `init` | always, `index.html` | [content/ARCHITECTURE.md](content/ARCHITECTURE.md) |
| `plans-wheel` | `selectPet`, `initWheel`, `renderPlanPanel`, `renderBonusPanel`, `renderActivePanel`, `updateTabsUI`, `generatePromoCode`, `addOneMonth`, `formatDate` | always, `index.html` | [plans-wheel/ARCHITECTURE.md](plans-wheel/ARCHITECTURE.md) |
| `roaming-mascots` | `initRoamBot`, `initRoamCritter`, `chaseWithBot`, `randomViewportSpot`, `hopMascotTo`, `showBubble`, `initMascotVisibility` | always, `index.html` | [roaming-mascots/ARCHITECTURE.md](roaming-mascots/ARCHITECTURE.md) |
| `device-panel` | `setPetMode` | always, `index.html` | [device-panel/ARCHITECTURE.md](device-panel/ARCHITECTURE.md) |
| `compare-plans` | `renderMomentTabs`, `applyMoment`, `setSpecies`, `initSpeciesSwitch`, `initPlanButtons`, `initFaq` | `compare.html`, **uncommitted / in progress** | [compare-plans/ARCHITECTURE.md](compare-plans/ARCHITECTURE.md) |

## 4. Placement (only where runsAt is a relation, §4.2)
None — a single `Loc` means nothing here is placed more than once, even across
the two HTML documents (each load is an independent instance of the same kind
of `Loc`, not a placement relation).

## 5. Coherence checklist (§4.5 / §8) against the implementation
- [x] 1. Placement honesty — no code path claims a real transmission occurs;
      "Choose plan"/"Join the Waitlist" are explicit no-op demo affordances.
- [x] 2. Transmission well-typing — vacuous; no `Trm` exists to type-check.
- [x] 3. Placement totality — every `Trn` runs in the single browser `Loc`.
- [~] 4. Dependency mediation — `content` owns shared utilities (`el`,
      `showToast`) that `plans-wheel`, `roaming-mascots`, and `device-panel`
      all call directly rather than through a declared port — acceptable at
      this scale (all same-file, same-`Loc`), but see
      [suggestions.md](suggestions.md) #1.
- [x] 5. Composition soundness — verified per-component in each
      IMPLEMENTATION.md.
- [x] 6. runsAt is a relation — vacuous; one `Loc` only.

## 6. Modeling smells swept (§3)
No parallel objects across `index.html`'s four components — `SERVICES`,
`PET_PLANS`, `PET_MODES`, and the roaming-mascot line arrays are genuinely
different `Dat`, not the same object duplicated. `compare.html`'s `DAYS`
dataset was **not** consolidated with `index.html`'s `PET_PLANS` even though
both describe pricing/plan content — see [suggestions.md](suggestions.md) #2
for why that's flagged rather than silently merged.
