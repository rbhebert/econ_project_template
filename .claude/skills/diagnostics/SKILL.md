---
name: diagnostics
description: Data checks, assertions, merge diagnostics, sample-flow logging, and DEBUG/QUICK run flags for R and Stata research scripts. Use when writing or editing data cleaning, merging, reshaping, or sample-construction code.
---

# Diagnostics and testing

Read `r.md` or `stata.md` in this folder for code templates.

## Run flags

Set once in the master script (or environment); individual scripts read them. Both default to off.

| Flag | Effect when on |
|---|---|
| `DEBUG` | Extra printing: heads/describes, tabulations, timers |
| `QUICK` | Seeded 5% random subsample to test the pipeline fast. Writes to `data/temp/` and `output/quick/`, never to final outputs |

## Always-on checks (not behind DEBUG)

Failures must **stop the script** (error), not warn.

1. **On load:** print row count; assert the unique key (e.g., `id × year`).
2. **Every merge/join:** declare the expected relationship (1:1, m:1); report matched / unmatched counts; assert row count doesn't change for m:1 left joins.
3. **After reshape or collapse:** assert key uniqueness at the new level.
4. **Key variables:** report missingness; assert plausible ranges (shares in [0, 1], years in the study window, non-negative counts).
5. **Before estimation:** report N, number of clusters, number of treated units; stop if the treatment has no variation.

## Sample flow

Record N after every sample restriction and save it to `output/tables/sample_flow.csv`. Never drop observations without logging how many.

## Logging

Each script writes a log to `code/logs/`.

## Don't

- Suppress warnings globally.
- Use `drop if missing(...)` / `filter(!is.na(...))` without counting what was dropped.
- Leave `QUICK` on in committed master scripts.
