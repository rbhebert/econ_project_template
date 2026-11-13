# Tables — Stata

## Regression tables: `esttab` (ssc install estout)

```stata
eststo clear

eststo: reghdfe outcome treat, absorb(id year) vce(cluster state)
estadd ysumm
estadd local fe_unit "Yes"
estadd local fe_year "Yes"
estadd local controls "No"
estadd scalar n_clust = e(N_clust)

eststo: reghdfe outcome treat $controls, absorb(id year) vce(cluster state)
estadd ysumm
estadd local fe_unit "Yes"
estadd local fe_year "Yes"
estadd local controls "Yes"
estadd scalar n_clust = e(N_clust)

esttab using "$root/output/tables/tab_main.tex", replace ///
    keep(treat) label b(3) se(3) ///
    star(* 0.10 ** 0.05 *** 0.01) ///
    booktabs fragment nomtitles nonotes ///
    mgroups("Outcome", pattern(1 0) prefix(\multicolumn{@span}{c}{) suffix(}) span) ///
    stats(fe_unit fe_year controls N r2_within ymean n_clust, ///
        labels("Unit FE" "Year FE" "Controls" "Observations" ///
               "Within R\$^2\$" "Mean of dep. var." "Clusters") ///
        fmt(0 0 0 %12.0fc 3 3 %9.0fc))

esttab, keep(treat) b(3) se(3) stats(N r2_within ymean)   // console check
```

Use variable labels (`label variable treat "Treated × Post"`) so `label` produces readable names.

Stata 17+ alternative: `etable` / `collect`.

## Summary statistics

```stata
estpost tabstat outcome treat x1 x2, statistics(mean sd min max count) columns(statistics)
esttab using "$root/output/tables/tab_sumstats.tex", replace ///
    cells("mean(fmt(3)) sd(fmt(3)) min(fmt(3)) max(fmt(3)) count(fmt(%12.0fc))") ///
    label booktabs fragment nonumbers nomtitles noobs

* balance table
estpost ttest x1 x2, by(treat)
esttab using "$root/output/tables/tab_balance.tex", replace ///
    cells("mu_1(fmt(3)) mu_2(fmt(3)) b(fmt(3)) se(fmt(3) par)") ///
    label booktabs fragment nonumbers noobs
```
