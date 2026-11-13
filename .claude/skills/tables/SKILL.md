---
name: tables
description: House style for regression and summary-statistics tables in R and Stata, exported as LaTeX booktabs fragments. Use whenever producing or editing a results table.
---

# Table style

Read `r.md` or `stata.md` in this folder for code templates.

## Regression tables

- One column per specification, numbered (1), (2), …; dependent variable in the header.
- Show key coefficients only. Report controls and fixed effects as Yes/No rows.
- Human-readable variable labels from a single dictionary — never raw variable names.
- Coefficients and SEs to **3 decimals** (adjust only if the scale requires it; keep it consistent within a table).
- SEs in parentheses below coefficients.
- Stars: `* p<0.10, ** p<0.05, *** p<0.01`. (AEA journals discourage stars — turn them off for submissions there.)
- Bottom rows, in this order: fixed effects, controls, observations (comma-separated), R² (within R² when FEs are absorbed), mean of dependent variable, number of clusters.
- Notes state: SE type and clustering level, sample, and the star convention.

## Summary statistics

- Columns: mean, SD, min, max, N. For group comparisons: means by group, difference, SE of difference.
- Report variables in their natural units with units in the label.

## Output

- Write a LaTeX **fragment** (the tabular only, no `table` environment) using `booktabs` to `output/tables/tab_<topic>.tex`. The paper controls captions, labels, and notes (`threeparttable`).
- Also write a `.csv` or print to console for quick checking.
- Never hand-type numbers into a table.

## LaTeX preamble the fragments assume

```latex
\usepackage{booktabs, threeparttable, siunitx}
```
