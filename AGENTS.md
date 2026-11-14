# Agent Instructions

Canonical context for AI coding agents. `CLAUDE.md` imports this file — edit here only.

## Context

- Applied economics research in **R** and **Stata**. Output feeds LaTeX papers and slides.
- Correctness and reproducibility matter more than speed or cleverness.

## Expected project layout

```
code/            numbered scripts: 01_clean.R, 02_build.do, 03_analysis.qmd,
                 04_robustness.do + 04_robustness.md ...
code/logs/       script logs
_quarto.yml      Quarto project settings (renders to output/reports/)
data/raw/        READ-ONLY source data
data/cleaned/    analysis-ready data
data/temp/       scratch; safe to delete
output/figures/  fig_<topic>.pdf / .png
output/tables/   tab_<topic>.tex / .csv
output/reports/  rendered Quarto / Stata reports
drafts/          paper and slides
docs/            budgets, RFPs, correspondence (not tracked)
```

## Ground rules

- Never modify, overwrite, or delete anything in `data/raw/`.
- Never commit data, credentials, or `.Renviron`.
- Paths are relative to the project root: R uses `here::here()`; Stata uses a `$root` global set once in the master do-file. No `setwd()`, no hard-coded user paths.
- Every script must run top to bottom in a fresh session. R: no `rm(list = ls())`. Stata: start with `version`, `clear all`, `set more off`.
- Set seeds for anything random (R `set.seed()`; Stata `set seed` and `set sortseed`).
- Never invent or "fill in" numbers. Report only estimates produced by code that actually ran; if it hasn't been run, say so.
- Ask before: changing the estimation sample or main specification, installing packages, or running jobs likely to exceed ~10 minutes.
- Keep every specification that was run. Don't silently drop results that look bad.
- Prefer editing existing scripts over creating new ones; one task per script.

## Readable, documented analysis

Every analysis should be understandable by someone reading it cold, including the author a year later.

- **R:** write analysis as **Quarto (`.qmd`) documents**, not bare `.R` scripts. Prose explains each step — what is estimated, why, and what the result means — between code chunks. Plain `.R` scripts are fine for data cleaning, building, and helper functions.
- **Stata:** every analysis do-file gets a **companion Markdown file** with the same name (`04_robustness.do` → `04_robustness.md`) explaining its purpose, inputs, outputs, each section of the code, and the main results. Do-file section headers must match the Markdown headings.
- **Code:** every script has a header comment (purpose, steps, inputs, outputs); comments explain why, not syntax. A multi-file pipeline also gets a `README.md` (how to run it) and a thorough `IMPLEMENTATION.md` (how the code works).
- Update the companion document and comments whenever the code changes. Out-of-date documentation is a bug.
- Results quoted in prose must come from code that ran; cite the log or rendered report they came from.

See the `documentation` skill for templates.

## Code style

- **R:** tidyverse style, native pipe `|>`, snake_case, `fixest` for regressions. Dependencies tracked with `renv`.
- **Stata:** 4-space indents, `///` for line continuation, `//` comments, locals for varlists, a log per do-file. List required SSC packages in the master do-file header.

## Skills

Task-specific guidance lives in `.claude/skills/`. Each skill has a `SKILL.md` (language-neutral rules) plus `r.md` and `stata.md` (code templates).

| Skill | Use for |
|---|---|
| `diagnostics` | Assertions, merge checks, sample-flow logging, DEBUG/QUICK flags |
| `figures` | Graph formatting and export |
| `tables` | Regression and summary-statistics tables |
| `causal-inference` | Estimator choice and required checks for DiD, event studies, IV, RD |
| `documentation` | Quarto analysis documents (R) and companion Markdown for do-files (Stata); readable, annotated code; an `IMPLEMENTATION.md` per pipeline; checking that readability rewrites leave results unchanged |

## Project-specific notes

<!-- Fill in per project: data sources, unit of observation, sample period, main specification, clustering level. -->
