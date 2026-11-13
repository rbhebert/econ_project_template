---
name: figures
description: House style for research figures in R (ggplot2) and Stata — colors, sizing, labels, event-study and coefficient plots, and export formats. Use whenever creating or editing a graph.
---

# Figure style

Read `r.md` or `stata.md` in this folder for the theme/scheme and export code.

## Principles

- One message per figure.
- **No title inside the image** — the title and notes go in the LaTeX caption.
- Axis labels in sentence case, with units: "Spending per capita (2020 USD)".
- Light horizontal gridlines only; no box around the plot; white background.
- Legend at the bottom with no title, or label lines directly. Order legend entries to match the data.
- Start bar-chart y-axes at zero. Don't truncate axes in misleading ways.

## Color

Colorblind-safe Okabe–Ito palette, in this order (skip yellow):

| # | Hex | RGB |
|---|---|---|
| 1 | `#000000` | 0 0 0 |
| 2 | `#0072B2` | 0 114 178 |
| 3 | `#D55E00` | 213 94 0 |
| 4 | `#009E73` | 0 158 115 |
| 5 | `#E69F00` | 230 159 0 |
| 6 | `#CC79A7` | 204 121 167 |
| 7 | `#56B4E9` | 86 180 233 |

- Use at most ~5 colors. Also vary shape or line type so the figure works in grayscale.
- Treated vs. control: treated = color 3, control = color 2.
- Continuous/maps: viridis.

## Size and export

| Use | Size (in) |
|---|---|
| Full-width paper | 6.5 × 4 |
| Half-width / panel | 3.2 × 2.6 |
| Slides (16:9) | 8 × 4.5 |

- Base font ~11 pt at final size. Don't rescale in LaTeX (`\includegraphics[width=\textwidth]` on a 6.5 in figure is fine).
- Export **PDF** (vector) for papers and **PNG at 300 dpi** for slides and Word.
- File names: `output/figures/fig_<topic>.pdf`, lowercase, underscores.

## Standard plot types

- **Event study:** point estimates with 95% CIs (caps or ribbon); dashed horizontal line at 0; dashed vertical line between −1 and 0; the reference period (−1) is shown at 0; x-axis "Periods relative to treatment".
- **Coefficient plot:** horizontal, sorted by estimate or in logical order, 95% CIs.
- **Binned scatter:** `binsreg` (R and Stata) rather than ad-hoc bins.
