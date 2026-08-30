# HMC Diagnostics for a dynamite Model

Prints the divergences, saturated treedepths, and low E-BFMI warnings.

## Usage

``` r
hmc_diagnostics(x, ...)

# S3 method for class 'dynamitefit'
hmc_diagnostics(x, ...)
```

## Arguments

- x:

  \[`dynamitefit`\]  
  The model fit object.

- ...:

  Ignored.

## Value

Returns `x` (invisibly). data.table::setDTthreads(1) \# For CRAN
hmc_diagnostics(gaussian_example_fit)

## See also

Model diagnostics
[`lfo()`](https://docs.ropensci.org/dynamite/reference/lfo.md),
[`loo.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/loo.dynamitefit.md),
[`mcmc_diagnostics()`](https://docs.ropensci.org/dynamite/reference/mcmc_diagnostics.md)
