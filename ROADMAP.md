# PHPInsights Fork — Maintenance Roadmap

This fork continues maintenance of [nunomaduro/phpinsights](https://github.com/nunomaduro/phpinsights),
which has been mostly inactive since early 2024 (dependency bumps only). This document
tracks what we fix, in what order, and why.

Status legend: ☐ todo · ◐ in progress · ✅ done

## Phase 0 — Baseline & identity *(week 1)*

Goal: establish the fork as a real installable package before touching code.

- ☐ Rename composer package (`nunomaduro/phpinsights` → our vendor) so installs don't collide with upstream; internal namespaces stay for now
- ☐ CI green on this fork; cut baseline tag `v2.14.3-fork.1`
- ☐ Land floor-score fix (upstream PR #722 → fixes #721 "scores rounded up")
- ☐ Land Pint-formatted Laravel config stub (upstream PR #729)

## Phase 1 — Trust-restoring bug fixes *(weeks 2–4)*

Goal: fix what makes users distrust the tool today.

| Priority | Issue | Notes |
|---|---|---|
| 🔴 | [#696](https://github.com/nunomaduro/phpinsights/issues/696) | `wmic` removed on Windows 11 24H2 — hard install failure |
| 🔴 | [#719](https://github.com/nunomaduro/phpinsights/issues/719) | Reports wrong version — trivial but embarrassing |
| 🟠 | [#651](https://github.com/nunomaduro/phpinsights/issues/651) · [#690](https://github.com/nunomaduro/phpinsights/issues/690) · [#689](https://github.com/nunomaduro/phpinsights/issues/689) | Config-resolution trio: excludes ignored w/ explicit paths, `remove` ignored, Laravel preset hides Observers — likely one root cause |
| 🟠 | [#514](https://github.com/nunomaduro/phpinsights/issues/514) | LineLength vs import-grouping collision (false positive) |
| 🟡 | [#629](https://github.com/nunomaduro/phpinsights/issues/629) · [#650](https://github.com/nunomaduro/phpinsights/issues/650) · #709 | Docs gaps + website offline → make README self-sufficient |

## Phase 2 — Dependency currency *(month 2)* ⚠️ most existentially important

Goal: survive the ecosystem moving under us.

1. **PHPCS 4 support** ([#723](https://github.com/nunomaduro/phpinsights/issues/723)) — rework conflicting upstream PR #724; squizlabs 3.x receives no more fixes
2. Bump `slevomat/coding-standard`, `friendsofphp/php-cs-fixer`, Symfony constraints; verify PHP 8.5 readiness
3. Fix modern-PHP false positives while there: enums (#621), readonly/inline classes (#616), unparsable docblocks (#589, #668), nullable-type complexity (#605)

## Phase 3 — High-demand QoL features *(months 2–4)*

Ordered by demand vs effort:

1. `--show=summary,...` filter (#400)
2. Output to file / format configurable (#279) — unblocks Docker/CI users
3. `include` alongside `exclude` (#419)
4. **Custom presets from config** (#370) — biggest community ask; natural seam for a future plugin system

## Phase 4 — Strategic bets *(quarter 2+)*

From wishlist [#576](https://github.com/nunomaduro/phpinsights/issues/576):

1. **Plugin architecture**: third-party presets/sniffs as separate packages → kills dependency-conflict pain (#687 larastan conflict)
2. **PHPStan wrapper** (#165): replaces deprecated `UnusedPrivateElements` sniff (#363)
3. HTML/shareable report + badges (#429, #232, #323)

## Standing practices

- **Triage cadence**: label everything (`bug`/`enhancement`/`question`); close stale questions after ~30 days
- **Backport policy**: track upstream master weekly (`git fetch upstream`); cherry-pick security/dep fixes
- **Tests**: every Phase-1 fix ships with a regression test — these bugs are silent config-resolution regressions
