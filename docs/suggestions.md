# System suggestions (roll-up)

> Roll-up of every `<component>/suggestions.md`, highest payoff first. Detail
> lives in the linked file.

| # | Component | Rule (§) | Smell | Proposed change |
| --- | --- | --- | --- | --- |
| 1 | content / plans-wheel / roaming-mascots / device-panel | §4 Ports & strategy | `el`/`showToast` called directly across component boundaries with no declared port | acceptable at this scale (same file, same `Loc`); only worth a real port if these components ever split into separate files |
| 2 | (system-wide) | §5 One source of truth | `compare.html:DAYS` (dog $39/mo, cat $29/mo) and `index.html:PET_PLANS` both encode plan pricing independently | if the two pages are meant to always agree on price, push pricing to one declared seam; if `compare.html`'s pricing is allowed to diverge (e.g. a promo page), record that explicitly instead of leaving it implicit |

## System-wide reductions
**#2 above is the one worth escalating**: two independent sources of the same
real-world fact (plan price) is exactly the §5 smell this framework exists to
catch. Given the no-build-step constraint (`CLAUDE.md`, both projects), the
practical fix isn't a shared module — it's a single comment in both files
pointing at each other ("keep in sync with compare.html:DAYS" /
"keep in sync with index.html:PET_PLANS") so a future price change doesn't
silently drift. Not applied in this scaffold — recorded here for the next
`work` cycle that touches pricing.
