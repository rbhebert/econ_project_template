# Causal inference — R templates

```r
library(fixest)
setFixest_estimation(fixef.rm = "singleton")
```

## TWFE / 2×2 DiD

```r
m <- feols(y ~ treat_post + x1 | id + year, data = df, cluster = ~ state)
```

## Event study

```r
# rel_time = year - first_treat_year; never-treated coded to a value outside the window
m_es <- feols(y ~ i(rel_time, treated, ref = -1) | id + year,
              data = df, cluster = ~ state)

# joint test of pre-period coefficients
wald(m_es, keep = "rel_time::-[2-9]")
```

## Staggered adoption

```r
# Sun & Abraham
m_sa <- feols(y ~ sunab(first_treat, year) | id + year, data = df, cluster = ~ state)
summary(m_sa, agg = "ATT")

# Callaway & Sant'Anna
library(did)
cs <- att_gt(yname = "y", tname = "year", idname = "id", gname = "first_treat",
             control_group = "notyettreated", clustervars = "state", data = df)
aggte(cs, type = "dynamic")
aggte(cs, type = "simple")

# Bacon decomposition (only when also showing TWFE)
bacondecomp::bacon(y ~ treat_post, data = df, id_var = "id", time_var = "year")
```

## Sensitivity to pre-trends

```r
# HonestDiD on the event-study coefficients and vcov
library(HonestDiD)
# see package vignette for extracting betahat / sigma from fixest or did objects
```

## IV

```r
m_iv <- feols(y ~ x1 | id + year | d ~ z, data = df, cluster = ~ state)
summary(m_iv, stage = 1)
fitstat(m_iv, ~ ivf + ivwald)
m_rf <- feols(y ~ z + x1 | id + year, data = df, cluster = ~ state)   # reduced form
```

## RD

```r
library(rdrobust); library(rddensity)
summary(rdrobust(df$y, df$running, c = 0))
summary(rddensity(df$running, c = 0))
rdplot(df$y, df$running, c = 0)

# bandwidth sensitivity
h <- rdrobust(df$y, df$running, c = 0)$bws[1, 1]
lapply(c(0.5, 0.75, 1.25, 1.5, 2) * h,
       \(b) rdrobust(df$y, df$running, c = 0, h = b)$coef)
```

## Poisson

```r
m_p <- fepois(count ~ treat_post | id + year, data = df, cluster = ~ state)
exp(coef(m_p)["treat_post"]) - 1
```

## Few clusters

Wild cluster bootstrap: `fwildclusterboot::boottest()` (if it isn't on CRAN, install from GitHub `s3alfisc/fwildclusterboot`).
