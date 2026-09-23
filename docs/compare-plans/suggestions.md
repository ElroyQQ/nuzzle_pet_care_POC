# Compare plans — suggestions (category-theory derived)

> Deduced from ARCHITECTURE.md by FRAMEWORK rules. Not applied — a backlog.

| # | Rule (§) | Smell found | Proposed change | Payoff |
| --- | --- | --- | --- | --- |
| 1 | §5 One source of truth | `DAYS.dog.price`/`DAYS.cat.price` (`$39`/`$29`) independently restate the same real-world facts as `index.html:PET_PLANS` | if the two pages must always agree, push pricing to one declared seam (or at minimum a paired comment in both files); if intentionally allowed to diverge, record that explicitly | prevents a future price change from silently drifting between the two pages |

## Detail

### 1. Independent pricing sources
`compare.html:DAYS` and `index.html:PET_PLANS` both encode the same two plans'
pricing, written separately by hand. With no build step to share a module
between the two single-file pages, the practical fix is a paired comment in
both files rather than a real shared object — see `docs/suggestions.md`
"System-wide reductions" for the reasoning.
