# Figures — Stata

## Scheme and palette (set in master.do)

```stata
* ssc install blindschemes
set scheme plotplainblind          // or stcolor (Stata 18+)
graph set window fontface "Helvetica"

global c1 "0 0 0"
global c2 "0 114 178"
global c3 "213 94 0"
global c4 "0 158 115"
global c5 "230 159 0"
```

## Standard options

```stata
twoway (line y_treat year, lcolor("$c3") lpattern(solid)) ///
       (line y_ctrl  year, lcolor("$c2") lpattern(dash)), ///
    xtitle("Year") ytitle("Spending per capita (2020 USD)") ///
    legend(order(1 "Treated" 2 "Control") position(6) rows(1) region(lstyle(none))) ///
    ylabel(, angle(horizontal) grid glcolor(gs14)) xlabel(, nogrid) ///
    graphregion(color(white)) xsize(6.5) ysize(4)
```

No `title()` — titles go in the LaTeX caption.

## Export

```stata
graph export "$root/output/figures/fig_trends.pdf", replace
graph export "$root/output/figures/fig_trends.png", width(1950) replace   // 6.5 in × 300 dpi
```

## Event study (coefplot, ssc install coefplot)

```stata
coefplot, keep(rel_*) vertical omitted baselevels ///
    yline(0, lpattern(dash) lcolor(gs8)) ///
    xline(4.5, lpattern(dash) lcolor(gs8)) ///   // adjust to sit between −1 and 0
    ciopts(recast(rcap) lcolor("$c2")) mcolor("$c2") ///
    xtitle("Periods relative to treatment") ytitle("Estimate (95% CI)") ///
    graphregion(color(white)) xsize(6.5) ysize(4)
```

`csdid`, `eventstudyinteract`, and `did_imputation` have their own plot commands (`csdid_plot`, `event_plot`); apply the same options.

## Binned scatter

```stata
* ssc install binsreg
binsreg y x z1 z2
```
