# Content — suggestions (category-theory derived)

> Deduced from ARCHITECTURE.md by FRAMEWORK rules. Not applied — a backlog.

| # | Rule (§) | Smell found | Proposed change | Payoff |
| --- | --- | --- | --- | --- |
| 1 | §4 Ports & strategy | `el`/`showToast`, owned here, are called directly by `plans-wheel`, `roaming-mascots`, and `device-panel` with no declared port | acceptable at single-file scale; only worth a real port if this file is ever split into modules | keeps the dependency direction (everything depends on `content`, not vice versa) visible instead of implicit |

## Detail

### 1. Undeclared utility port
`el` (line 1001) and `showToast` (line 1368) are the two functions every other
`index.html` component calls directly. Nothing is broken by this — same file,
same `Loc`, no real boundary to cross — but per §4.5 Law 4 it's worth naming
explicitly so a future split (e.g. extracting `plans-wheel` to its own file)
knows exactly what it would need to import.
