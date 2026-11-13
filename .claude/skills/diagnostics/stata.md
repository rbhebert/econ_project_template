# Diagnostics — Stata templates

## Flags (in master.do; individual do-files inherit)

```stata
if "$DEBUG" == "" global DEBUG 0
if "$QUICK" == "" global QUICK 0
set seed 20260101
set sortseed 20260101

global out "$root/output"
if $QUICK global out "$root/output/quick"
```

In a do-file: `if $QUICK sample 5`

## Checks

```stata
* unique key
isid id year

* merge with declared pattern; errors on anything unexpected
local n0 = _N
merge m:1 fips using "$root/data/cleaned/county", ///
    assert(match master) keep(match master) nogenerate
assert _N == `n0'

* ranges and missingness
assert inrange(share, 0, 1) if !missing(share)
assert year >= 2010 & year <= 2025
misstable summarize outcome treat

* before estimation
quietly levelsof state, local(cl)
display "Clusters: `: word count `cl''"
quietly summarize treat
assert r(sd) > 0

if $DEBUG {
    describe
    summarize
}
```

## Sample flow

```stata
capture program drop log_n
program log_n
    args step
    quietly count
    file write flow `"`step',`r(N)'"' _n
    display as text "`step': N = " as result %12.0fc r(N)
end

file open flow using "$out/tables/sample_flow.csv", write replace
file write flow "step,n" _n
log_n "Loaded panel"
drop if missing(outcome)
log_n "Non-missing outcome"
file close flow
```

## Logging and timing

```stata
capture log close
log using "$root/code/logs/03_analysis.log", replace text
timer clear
timer on 1
* ...
timer off 1
timer list
log close
```
