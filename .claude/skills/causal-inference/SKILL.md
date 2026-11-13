---
name: causal-inference
description: Estimator choice, required diagnostics, and reporting rules for regression-based causal inference in R and Stata — fixed effects, difference-in-differences (including staggered adoption), event studies, IV, regression discontinuity, and Poisson. Use when writing or reviewing any estimation code.
---

# Causal inference and regression

Read `r.md` or `stata.md` in this folder for code templates.

## Before writing estimation code

Write a comment block at the top of the script stating:

- Estimand (ATT, LATE, …) and the unit of observation
- Treatment definition and timing
- Identifying assumption, in one sentence
- Comparison group
- Clustering level and why

If any of these is unclear, ask rather than guess.

## General rules

- Cluster SEs at the level treatment is assigned. With fewer than ~40 clusters, also report wild cluster bootstrap p-values.
- Absorb fixed effects (`fixest` / `reghdfe`); don't add dummies by hand.
- Keep a spec log: every specification run, including ones not reported.
- For counts or non-negative outcomes with zeros, use Poisson (PPML) rather than log(y + 1).
- Never interpret a coefficient as causal in comments or output unless the design supports it.

## Methods and required checks

| Design | R | Stata | Required checks |
|---|---|---|---|
| FE / 2×2 DiD | `fixest::feols` | `reghdfe` | Event-study pre-trends; covariate balance |
| Staggered DiD | `did` (Callaway–Sant'Anna), `fixest::sunab`, `didimputation`, `did2s` | `csdid`, `eventstudyinteract`, `did_imputation`, `jwdid` | Don't rely on TWFE alone. If TWFE is shown, add a Bacon decomposition (`bacondecomp`). State the control group (never- vs not-yet-treated) |
| Event study | `fixest::feols` with `i()` or `sunab()` | `reghdfe` with relative-time dummies, or the staggered estimators above | Reference period −1; bin endpoints; joint test of pre-period coefficients; `HonestDiD` sensitivity for the main result |
| IV | `fixest::feols(y ~ x | fe | d ~ z)` | `ivreghdfe`, `ivreg2` | Report first stage, reduced form, and effective F (Montiel Olea–Pflueger); weak-IV-robust (Anderson–Rubin) CI if F is low |
| RD | `rdrobust`, `rddensity`, `rdplot` | same command names | Density test; covariate balance at cutoff; bandwidth sensitivity (0.5×–2×); donut; placebo cutoffs; report robust bias-corrected CIs |
| Poisson | `fixest::fepois` | `ppmlhdfe` | Report as semi-elasticities: exp(b) − 1 |
| Synthetic control | `synthdid` | `sdid` | Pre-period fit; placebo/permutation inference |

## Reporting

- Main table, then robustness table(s), then heterogeneity — each pre-specified where possible.
- Report point estimates with CIs, not only stars.
- Translate the main estimate into a meaningful magnitude (percent of the mean, per-SD effect) in a comment next to the code that produces it.
