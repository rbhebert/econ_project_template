# Figures — R (ggplot2)

## Theme and palette (put in `code/utils/theme.R` and `source()` it)

```r
library(ggplot2)

okabe_ito <- c("#000000", "#0072B2", "#D55E00", "#009E73",
               "#E69F00", "#CC79A7", "#56B4E9")

theme_paper <- function(base_size = 11) {
  theme_minimal(base_size = base_size) +
    theme(
      panel.grid.minor   = element_blank(),
      panel.grid.major.x = element_blank(),
      axis.ticks         = element_line(colour = "grey30", linewidth = 0.3),
      axis.line.x        = element_line(colour = "grey30", linewidth = 0.3),
      legend.position    = "bottom",
      legend.title       = element_blank(),
      plot.background    = element_rect(fill = "white", colour = NA)
    )
}

theme_set(theme_paper())
options(ggplot2.discrete.colour = okabe_ito,
        ggplot2.discrete.fill   = okabe_ito)

save_fig <- function(p, name, width = 6.5, height = 4) {
  dir <- here::here("output/figures")
  ggsave(file.path(dir, paste0(name, ".pdf")), p, width = width, height = height)
  ggsave(file.path(dir, paste0(name, ".png")), p, width = width, height = height, dpi = 300)
}
```

## Event study

```r
# m: fixest model with i(rel_time, treat, ref = -1) or sunab()
fixest::iplot(m)                     # quick look only

# final figure
ggfixest::ggiplot(m, geom_style = "errorbar") +
  geom_hline(yintercept = 0, linetype = "dashed") +
  labs(x = "Periods relative to treatment", y = "Estimate (95% CI)", title = NULL) +
  theme_paper()
```

## Coefficient plot

```r
modelsummary::modelplot(list("Main" = m1, "Controls" = m2), coef_map = coef_labels) +
  geom_vline(xintercept = 0, linetype = "dashed")
```

## Binned scatter

```r
binsreg::binsreg(y = df$y, x = df$x, w = df[c("z1", "z2")])
```
