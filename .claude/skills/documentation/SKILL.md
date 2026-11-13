---
name: documentation
description: How analysis code is documented — Quarto (.qmd) documents for R analyses and a companion Markdown file for every Stata analysis do-file. Use when creating or editing any analysis script, or when asked to explain or summarize an analysis.
---

# Documenting analyses

Read `r.md` or `stata.md` in this folder for templates.

## Which files need it

| Script type | R | Stata |
|---|---|---|
| Analysis (estimation, figures, tables) | `.qmd` document | `.do` + same-name `.md` |
| Cleaning / building data | `.R` script with header comment | `.do` with header comment; `.md` optional |
| Helper functions | `.R` with roxygen-style comments | `.ado` / program with header comment |

## Every analysis document covers

1. **Purpose:** the question this file answers, in 1–2 sentences.
2. **Inputs and outputs:** data files read; figures and tables written (with paths).
3. **Design:** estimand, treatment, comparison group, identifying assumption, clustering level (see the `causal-inference` skill).
4. **Steps:** one section per logical step, each with a short explanation of *what* the code does and *why*.
5. **Results:** the main estimates in words with magnitudes (e.g., "a 4.2% increase relative to the mean"), plus caveats.
6. **Open issues:** known problems, robustness checks still to run.

## Writing rules

- Plain language; define acronyms once. Aim for a reader who knows econometrics but not this project.
- Explain choices, not syntax: "Cluster by state because treatment varies at the state level", not "This line clusters SEs".
- Never type numbers into prose by hand in Quarto — use inline code (`` `r ` ``). In Stata Markdown, copy numbers from the log and note the log file and run date.
- Keep it short: a reader should get the gist in 2 minutes.
- When code changes, update the document in the same edit.

## Rendering

- Quarto renders to `output/reports/` (configured in `_quarto.yml`); rendered HTML is not tracked.
- Stata `.md` files live in `code/` next to their do-files and are tracked.
