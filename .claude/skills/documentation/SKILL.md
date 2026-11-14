---
name: documentation
description: How analysis code (R and Stata) is documented and kept readable — Quarto (.qmd) documents for R analyses, a companion Markdown file for every Stata analysis do-file, header comments and annotated code, a separate implementation guide for each pipeline, and verified-equivalent refactors. Use when creating, editing, rewriting or annotating any analysis or pipeline code, or when asked to explain or summarize an analysis.
---

# Documenting analyses

Read `r.md` or `stata.md` in this folder for templates.

## Which files need it

| Script type | R | Stata |
|---|---|---|
| Analysis (estimation, figures, tables) | `.qmd` document | `.do` + same-name `.md` (or a section in the pipeline `README.md`) |
| Cleaning / building data | `.R` script with header comment | `.do` with header comment; `.md` optional |
| Helper functions / programs | `.R` with roxygen-style comments | `.ado` / program with header comment |
| Settings | comment beside each entry in the config script | comment beside each global in the config do-file |
| A whole pipeline | `README.md` (how to run, flags, outputs, tests) **and** `IMPLEMENTATION.md` (how the code works; see below) | same |

Helper headers state the arguments, what it does, what it returns, and any side effects.

## Every analysis document covers

1. **Purpose:** the question this file answers, in 1–2 sentences.
2. **Inputs and outputs:** data files read; figures and tables written (with paths).
3. **Design:** estimand, treatment, comparison group, identifying assumption, clustering level (see the `causal-inference` skill).
4. **Steps:** one section per logical step, each with a short explanation of *what* the code does and *why*.
5. **Results:** the main estimates in words with magnitudes (e.g., "a 4.2% increase relative to the mean"), plus caveats.
6. **Open issues:** known problems, robustness checks still to run.

## Readable, annotated code

Code must be understandable by someone reading it cold, including the author a year later.

- **File header:** purpose, the steps in order, inputs, outputs, the file it ports or replaces (if any), and the run flag that controls it.
- **Section headers** that match the steps in the header and the Markdown headings.
- **Function / program headers:** arguments, what it does, what it returns, and any side effects (e.g. "adds columns to `d` by reference").
- **Comment the why, not the syntax:** coding decisions, survey-question or variable codings (what each value means, by year if it changed), patches to upstream code, and language behaviour being relied on or reproduced (e.g. Stata missing values compare as larger than any number).
- **Write the math next to the estimator:** for non-trivial estimators (FWL, bootstrap closed forms, normalizations), state the formula in a comment so the code can be checked against it.
- **Split long functions** into named helpers, one per logical step, so the main function reads as a list of steps.
- **Names:** descriptive for anything long-lived; short loop variables are fine if the convention is documented once (e.g. in the config header). In R `data.table`, a loop variable must never share a name with a column.
- No stale comments: a comment that no longer matches the code is a bug.

## Implementation guide (per pipeline)

Each pipeline gets an `IMPLEMENTATION.md`, separate from the code comments and from the README. Unlike analysis documents, it is meant to be thorough. It explains how the code works for someone who has to check, change or extend it:

1. Architecture: files, how they are loaded and called, data flow, global state.
2. Conventions: codes, suffixes, naming, by-reference changes, result-table columns.
3. Each data step: what it builds and how.
4. The model and the estimator, with the algebra (e.g. FWL, bootstrap derivation, CI search).
5. Each analysis step: what one estimation unit is, what is tested, what is written.
6. Caching or other run-time machinery, including what it does not detect.
7. Output formats, tests, how to extend the pipeline, pitfalls, and a Stata ↔ R map where both versions exist.

Link it from the pipeline `README.md`, and update it when the code changes.

## Rewrites and refactors for readability

A readability change must not change results.

- Keep function names, arguments, cache keys and the order of arithmetic unless the change is the point.
- Before editing, keep a copy of the original code; run original and rewritten code on the same inputs with caching off, and compare every output file (data, `.rds` / `.dta`, csv, xlsx cell by cell, figures). Run the test suite.
- Report the comparison (number of files, identical or not) and the test result. If outputs differ, find out why before going further; never accept a difference silently.

## Writing rules

- Plain language; define acronyms once. Aim for a reader who knows econometrics but not this project.
- Explain choices, not syntax: "Cluster by state because treatment varies at the state level", not "This line clusters SEs".
- Never type numbers into prose by hand in Quarto — use inline code (`` `r ` ``). In Stata Markdown, copy numbers from the log and note the log file and run date.
- Check facts about codings and data against the source (the script, the codebook) before writing them in a comment.
- If code has not been run since a change, say so in the document.
- Analysis documents are short: a reader should get the gist in 2 minutes. The implementation guide is the exception.
- When code changes, update the comments and documents in the same edit.

## Rendering

- Quarto renders to `output/reports/` (configured in `_quarto.yml`); rendered HTML is not tracked.
- Stata `.md` files live in `code/` next to their do-files and are tracked.
