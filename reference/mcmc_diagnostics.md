# Diagnostic Values of a dynamite Model

Prints HMC diagnostics and lists parameters with smallest effective
sample sizes and largest Rhat values. See
[`hmc_diagnostics()`](https://docs.ropensci.org/dynamite/reference/hmc_diagnostics.md)
and
[`posterior::default_convergence_measures()`](https://mc-stan.org/posterior/reference/draws_summary.html)
for details.

## Usage

``` r
mcmc_diagnostics(x, ...)

# S3 method for class 'dynamitefit'
mcmc_diagnostics(x, n = 3L, ...)
```

## Arguments

- x:

  \[`dynamitefit`\]  
  The model fit object.

- ...:

  Ignored.

- n:

  \[`integer(1)`\]  
  How many rows to print in parameter-specific convergence measures. The
  default is 3. Should be a positive (unrestricted) integer.

## Value

Returns `x` (invisibly).

## See also

Model diagnostics
[`hmc_diagnostics()`](https://docs.ropensci.org/dynamite/reference/hmc_diagnostics.md),
[`lfo()`](https://docs.ropensci.org/dynamite/reference/lfo.md),
[`loo.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/loo.dynamitefit.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
mcmc_diagnostics(gaussian_example_fit)
#> NUTS sampler diagnostics:
#> 
#> No divergences, saturated max treedepths or low E-BFMIs.
#> 
#> Smallest bulk-ESS values: 
#>                 
#> alpha_y[28]   72
#> alpha_y[10]  126
#> delta_y_x[7] 126
#> 
#> Smallest tail-ESS values: 
#>                  
#> nu_y_alpha_id6 83
#> sigma_y        91
#> alpha_y[28]    94
#> 
#> Largest Rhat values: 
#>                        
#> delta_y_y_lag1[28] 1.03
#> alpha_y[29]        1.03
#> alpha_y[28]        1.03
```
