# Tables — R

## Regression tables: `fixest::etable`

```r
dict <- c(treat = "Treated × Post", log_inc = "Log income",
          id = "Unit", year = "Year")

models <- list(m1, m2, m3)

fixest::etable(
  models,
  dict       = dict,
  keep       = "%treat",
  digits     = 3,
  digits.stats = 3,
  fitstat    = ~ n + wr2 + my,          # obs, within R², mean of dep. var.
  signif.code = c("***" = 0.01, "**" = 0.05, "*" = 0.10),
  extralines = list("Controls" = c("No", "Yes", "Yes")),
  tex        = TRUE,
  style.tex  = fixest::style.tex("aer"),
  float      = FALSE,
  file       = here::here("output/tables/tab_main.tex"),
  replace    = TRUE
)

fixest::etable(models, dict = dict)   # console check
```

Set a project-wide dictionary once with `fixest::setFixest_dict(dict)`.

## Summary statistics: `modelsummary::datasummary`

```r
library(modelsummary)

datasummary(
  (`Outcome (USD)` = outcome) + (`Treated` = treat) ~ Mean + SD + Min + Max + N,
  data   = df,
  fmt    = 3,
  output = here::here("output/tables/tab_sumstats.tex")
)

datasummary_balance(~ treat, data = df[c("treat", "x1", "x2")],
                    output = here::here("output/tables/tab_balance.tex"))
```

Check that the generated `.tex` uses `booktabs` rules; if it includes a `table` environment, strip it or wrap the fragment yourself.
