# Approximate Leave-One-Out (LOO) Cross-validation

Estimates the leave-one-out (LOO) information criterion for `dynamite`
models using Pareto smoothed importance sampling with the loo package.

## Usage

``` r
# S3 method for class 'dynamitefit'
loo(x, separate_channels = FALSE, thin = 1L, ...)
```

## Arguments

- x:

  \[`dynamitefit`\]  
  The model fit object.

- separate_channels:

  \[`logical(1)`\]  
  If `TRUE`, computes LOO separately for each channel. This can be
  useful in diagnosing where the model fails. Default is `FALSE`, in
  which case the likelihoods of different channels are combined, i.e.,
  all channels of are left out.

- thin:

  \[`integer(1)`\]  
  Use only every `thin` posterior sample when computing LOO. This can be
  beneficial with when the model object contains large number of
  samples. Default is `1` meaning that all samples are used.

- ...:

  Ignored.

## Value

An output from
[`loo::loo()`](https://mc-stan.org/loo/reference/loo.html) or a list of
such outputs (if `separate_channels` was `TRUE`).

## References

Aki Vehtari, Andrew, Gelman, and Johah Gabry (2017). Practical Bayesian
model evaluation using leave-one-out cross-validation and WAIC.
Statistics and Computing. 27(5), 1413–1432.

## See also

Model diagnostics
[`hmc_diagnostics()`](https://docs.ropensci.org/dynamite/reference/hmc_diagnostics.md),
[`lfo()`](https://docs.ropensci.org/dynamite/reference/lfo.md),
[`mcmc_diagnostics()`](https://docs.ropensci.org/dynamite/reference/mcmc_diagnostics.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
# \donttest{
# Please update your rstan and StanHeaders installation before running
# on Windows
if (!identical(.Platform$OS.type, "windows")) {
  # this gives warnings due to the small number of iterations
  suppressWarnings(loo(gaussian_example_fit))
  suppressWarnings(loo(gaussian_example_fit, separate_channels = TRUE))
}
#> $y_loglik
#> 
#> Computed from 200 by 1450 log-likelihood matrix.
#> 
#>          Estimate   SE
#> elpd_loo    241.1 27.0
#> p_loo        91.2  3.4
#> looic      -482.3 53.9
#> ------
#> MCSE of elpd_loo is NA.
#> MCSE and ESS estimates assume MCMC draws (r_eff in [0.4, 2.2]).
#> 
#> Pareto k diagnostic values:
#>                           Count Pct.    Min. ESS
#> (-Inf, 0.57]   (good)     1436  99.0%   60      
#>    (0.57, 1]   (bad)        14   1.0%   <NA>    
#>     (1, Inf)   (very bad)    0   0.0%   <NA>    
#> See help('pareto-k-diagnostic') for details.
#> 
# }
```
