# Causal inference — Stata templates

Required SSC packages (list in master.do header):

```stata
* ssc install reghdfe ftools ivreghdfe ivreg2 ranktest
* ssc install csdid drdid eventstudyinteract did_imputation jwdid bacondecomp
* ssc install rdrobust rddensity lpdensity
* ssc install ppmlhdfe boottest weakivtest
```

## TWFE / 2×2 DiD

```stata
reghdfe y treat_post x1, absorb(id year) vce(cluster state)
```

## Event study

```stata
* rel_time = year - first_treat; never-treated have rel_time missing
forvalues k = 5(-1)2 {
    gen byte pre`k' = (rel_time == -`k')
}
forvalues k = 0/5 {
    gen byte post`k' = (rel_time == `k')
}
replace pre5  = (rel_time <= -5) if !missing(rel_time)    // bin endpoints
replace post5 = (rel_time >=  5) if !missing(rel_time)
* period -1 omitted as reference

reghdfe y pre5 pre4 pre3 pre2 post0-post5, absorb(id year) vce(cluster state)
test pre5 pre4 pre3 pre2
```

## Staggered adoption

```stata
* Callaway & Sant'Anna (first_treat = 0 for never treated)
csdid y, ivar(id) time(year) gvar(first_treat) notyet
estat simple
estat event
csdid_plot

* Borusyak, Jaravel & Spiess
did_imputation y id year first_treat, horizons(0/5) pretrends(4) autosample
event_plot

* Bacon decomposition (only when also showing TWFE)
xtset id year
bacondecomp y treat_post, ddetail
```

## IV

```stata
ivreghdfe y x1 (d = z), absorb(id year) cluster(state) first
weakivtest                                  // effective F (Montiel Olea–Pflueger)
reghdfe y z x1, absorb(id year) vce(cluster state)   // reduced form
```

## RD

```stata
rdrobust y running, c(0)
rddensity running, c(0)
rdplot y running, c(0)

* bandwidth sensitivity
rdrobust y running, c(0)
local h = e(h_l)
foreach m in 0.5 0.75 1.25 1.5 2 {
    rdrobust y running, c(0) h(`=`m'*`h'')
}
```

## Poisson

```stata
ppmlhdfe count treat_post, absorb(id year) vce(cluster state)
display exp(_b[treat_post]) - 1
```

## Few clusters

```stata
reghdfe y treat_post, absorb(id year) vce(cluster state)
boottest treat_post, reps(9999) seed(20260101)
```
