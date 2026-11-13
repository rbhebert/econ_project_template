# Diagnostics — R templates

## Flags

```r
DEBUG <- as.logical(Sys.getenv("DEBUG", "FALSE"))
QUICK <- as.logical(Sys.getenv("QUICK", "FALSE"))
set.seed(20260101)

out_dir <- if (QUICK) here::here("output/quick") else here::here("output")
if (QUICK) df <- dplyr::slice_sample(df, prop = 0.05)
```

Run with flags from the terminal: `DEBUG=TRUE QUICK=TRUE Rscript code/03_analysis.R`

## Helpers (put in `code/utils/checks.R` and `source()` it)

```r
assert_unique <- function(df, keys) {
  dup <- anyDuplicated(df[keys])
  if (dup) stop("Duplicate keys on ", paste(keys, collapse = " x "), " (first at row ", dup, ")")
  invisible(df)
}

checked_left_join <- function(x, y, by, relationship = "many-to-one") {
  n0 <- nrow(x)
  unmatched <- nrow(dplyr::anti_join(x, y, by = by))
  out <- dplyr::left_join(x, y, by = by, relationship = relationship)
  message(sprintf("join: %d rows, %d matched, %d unmatched", n0, n0 - unmatched, unmatched))
  stopifnot(nrow(out) == n0)
  out
}

assert_range <- function(x, lo, hi, name = deparse(substitute(x))) {
  bad <- sum(!is.na(x) & (x < lo | x > hi))
  if (bad) stop(name, ": ", bad, " values outside [", lo, ", ", hi, "]")
  invisible(x)
}

report_missing <- function(df, vars) {
  print(colSums(is.na(df[vars])))
}

# sample flow
flow <- data.frame(step = character(), n = integer())
log_n <- function(df, step) {
  flow <<- rbind(flow, data.frame(step = step, n = nrow(df)))
  message(step, ": N = ", format(nrow(df), big.mark = ","))
  df
}
```

## Usage

```r
df <- readRDS(here::here("data/cleaned/panel.rds")) |>
  assert_unique(c("id", "year")) |>
  log_n("Loaded panel")

df <- df |> filter(!is.na(outcome)) |> log_n("Non-missing outcome")
df <- checked_left_join(df, county, by = "fips")
assert_range(df$share, 0, 1)

if (DEBUG) { dplyr::glimpse(df); report_missing(df, c("outcome", "treat")) }

write.csv(flow, file.path(out_dir, "tables/sample_flow.csv"), row.names = FALSE)
```

Optional packages for heavier validation: `assertr`, `pointblank`.
