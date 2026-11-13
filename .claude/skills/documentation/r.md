# Documentation — R (Quarto)

## Project settings: `_quarto.yml` (repo root)

```yaml
project:
  type: default
  output-dir: output/reports
  execute-dir: project

format:
  html:
    toc: true
    code-fold: show
    code-tools: true
    embed-resources: true
    df-print: kable

execute:
  echo: true
  warning: true
  message: false
  freeze: auto
```

Render one file: `quarto render code/03_analysis.qmd` (or the Render button in RStudio).

## Analysis template: `code/03_analysis.qmd`

````markdown
---
title: "Main analysis: effect of X on Y"
author: "Reginald Hebert"
date: today
---

## Purpose

What question this document answers, in 1–2 sentences.

**Inputs:** `data/cleaned/panel.rds`
**Outputs:** `output/tables/tab_main.tex`, `output/figures/fig_event_study.pdf`

## Design

- **Estimand:** ATT of ... on ...
- **Unit / period:** county-year, 2010–2024
- **Treatment:** ...; **comparison group:** not-yet-treated counties
- **Identifying assumption:** parallel trends in ... absent treatment
- **Inference:** SEs clustered by state (level of treatment assignment)

```{r}
#| label: setup
library(fixest)
library(ggplot2)
source(here::here("code/utils/checks.R"))
source(here::here("code/utils/theme.R"))
set.seed(20260101)

df <- readRDS(here::here("data/cleaned/panel.rds"))
```

## Sample

Why the sample is restricted the way it is.

```{r}
#| label: sample
df <- df |> filter(!is.na(outcome)) |> log_n("Non-missing outcome")
```

The final sample has `r format(nrow(df), big.mark = ",")` county-years.

## Main estimates

What is estimated and why this specification is the main one.

```{r}
#| label: main
m1 <- feols(outcome ~ treat_post | id + year, data = df, cluster = ~ state)
etable(m1)
```

Treatment changes the outcome by `r round(coef(m1)["treat_post"], 3)`
(`r round(100 * coef(m1)["treat_post"] / mean(df$outcome), 1)`% of the mean).

## Event study

...

## Caveats and open issues

- ...
````

## Rules

- Chunk labels on every chunk (`#| label:`), lowercase with hyphens.
- Long-running chunks: `#| cache: true`, or save results to `data/temp/` and load them.
- Figures and tables are still saved to `output/` with `save_fig()` / `etable(file = ...)`; the report shows them too.
- Inline `r` code for every number in prose.
