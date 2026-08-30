# Additional Specifications for the Group-level Random Effects of the DMPM

This function can be used as part of
[`dynamiteformula()`](https://docs.ropensci.org/dynamite/reference/dynamiteformula.md)
to define whether the group-level random effects should be modeled as
correlated or not.

## Usage

``` r
random_spec(correlated = TRUE, noncentered = TRUE)
```

## Arguments

- correlated:

  \[`logical(1)`\]  
  If `TRUE` (the default), correlations of random effects are modeled as
  multivariate normal.

- noncentered:

  \[`logical(1)`\]  
  If `TRUE` (the default), use a noncentered parameterization for random
  effects. Try changing this if you encounter divergences or other
  problems in sampling.

## Value

An object of class `random_spec`.

## Details

With a large number of time points random intercepts can become
challenging sample with default priors. This is because with large group
sizes the group-level intercepts tend to be behave similarly to fixed
group-factor variable so the model becomes overparameterized given these
and the common intercept term. Another potential cause for sampling
problems is relatively large variation in the intercepts (large
sigma_nu) compared to the sampling variation (sigma) in the Gaussian
case.

## See also

Model formula construction
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md),
[`dynamiteformula()`](https://docs.ropensci.org/dynamite/reference/dynamiteformula.md),
[`lags()`](https://docs.ropensci.org/dynamite/reference/lags.md),
[`lfactor()`](https://docs.ropensci.org/dynamite/reference/lfactor.md),
[`splines()`](https://docs.ropensci.org/dynamite/reference/splines.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
# two channel model with correlated random effects for responses x and y
obs(y ~ 1 + random(~1), family = "gaussian") +
  obs(x ~ 1 + random(~1 + z), family = "poisson") +
  random_spec(correlated = TRUE)
#>   Family   Formula               
#> y gaussian y ~ 1 + random(~1)    
#> x poisson  x ~ 1 + random(~1 + z)
#> 
#> Correlated random effects added for response(s): y, x
```
