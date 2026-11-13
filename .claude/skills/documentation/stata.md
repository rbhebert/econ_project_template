# Documentation — Stata (companion Markdown)

Each analysis do-file gets a same-name `.md` in `code/`. Section headers in the do-file match the `##` headings in the Markdown, so a reader can jump between them.

## Do-file header and sections: `code/04_robustness.do`

```stata
/*==============================================================================
  04_robustness.do
  Purpose: Robustness of the main DiD estimate to alternative controls/samples
  Inputs:  $root/data/cleaned/panel.dta
  Outputs: $root/output/tables/tab_robustness.tex
  Docs:    code/04_robustness.md
==============================================================================*/

version 18
clear all
set more off
capture log close
log using "$root/code/logs/04_robustness.log", replace text

* ---- 1. Sample ----------------------------------------------------------------
...

* ---- 2. Alternative control groups -------------------------------------------
...

* ---- 3. Dropping early adopters ----------------------------------------------
...

log close
```

## Companion Markdown: `code/04_robustness.md`

```markdown
# 04_robustness.do

**Purpose:** Test whether the main DiD estimate holds under alternative
control groups and samples.

**Inputs:** `data/cleaned/panel.dta`
**Outputs:** `output/tables/tab_robustness.tex`
**Log:** `code/logs/04_robustness.log` (last run: 2026-11-13)

## Design

- Estimand, treatment, comparison group, identifying assumption, clustering.

## 1. Sample

What restrictions are applied and why. Final N = 12,345 (from log).

## 2. Alternative control groups

Why never-treated vs. not-yet-treated matters here; which estimator.

## 3. Dropping early adopters

Why early adopters might bias results.

## Results

| Specification | Estimate | SE |
|---|---|---|
| Main | 0.042 | 0.011 |
| Never-treated controls | 0.039 | 0.013 |

Estimates are stable across specifications; ...
(Numbers copied from the log above — update after each run.)

## Open issues

- ...
```

## Optional: generated reports

For a self-updating report like Quarto, Stata's built-in `dyndoc` turns a Markdown file with embedded Stata code into HTML:

```stata
dyndoc "$root/code/04_robustness_report.txt", saving("$root/output/reports/04_robustness.html") replace
```

Use this only when the user asks for it; the plain companion `.md` is the default.
