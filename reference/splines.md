# Define the B-splines Used for the Time-varying Coefficients of the Model.

This function can be used as part of
[`dynamiteformula()`](https://docs.ropensci.org/dynamite/reference/dynamiteformula.md)
to define the splines used for the time-varying coefficients \\\delta\\.

## Usage

``` r
splines(
  df = NULL,
  degree = 3L,
  lb_tau = 0,
  noncentered = FALSE,
  override = FALSE
)
```

## Arguments

- df:

  \[`integer(1)`\]  
  Degrees of freedom, i.e., the total number of spline coefficients. See
  [`splines::bs()`](https://rdrr.io/r/splines/bs.html). Note that the
  knots are always defined as an equidistant sequence on the interval
  starting from the first non-fixed time point to the last time point in
  the data. See
  [`dynamiteformula()`](https://docs.ropensci.org/dynamite/reference/dynamiteformula.md)
  for more information on fixed time points. Should be an (unrestricted)
  positive integer.

- degree:

  \[`integer(1)`\]  
  See [`splines::bs()`](https://rdrr.io/r/splines/bs.html). Should be an
  (unrestricted) positive integer.

- lb_tau:

  \[[`numeric()`](https://rdrr.io/r/base/numeric.html)\]  
  Hard constraint(s) on the lower bound of the standard deviation
  parameters \\\tau\\ of the random walk priors. Can be useful in
  avoiding divergences in some cases. See also the `noncentered`
  argument. Can be a single positive value, or vector defining the lower
  bound separately for each channel, even for channels without varying
  effects. The ordering is based on the order of channel definitions in
  the `dynamiteformula` object.

- noncentered:

  \[[`logical()`](https://rdrr.io/r/base/logical.html)\]  
  If `TRUE`, use a noncentered parameterization for the spline
  coefficients. Default is `FALSE`. Try changing this if you encounter
  divergences or other problems in sampling for example when simulating
  from prior predictive distribution. Can be a single logical value, or
  vector of logical values, defining the parameterization separately for
  each channel, even for channels without varying effects.

- override:

  \[`logical(1)`\]  
  If `FALSE` (the default), an existing definition for the splines will
  not be overridden by another call to `splines()`. If `TRUE`, any
  existing definitions will be replaced.

## Value

An object of class `splines`.

## See also

Model formula construction
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md),
[`dynamiteformula()`](https://docs.ropensci.org/dynamite/reference/dynamiteformula.md),
[`lags()`](https://docs.ropensci.org/dynamite/reference/lags.md),
[`lfactor()`](https://docs.ropensci.org/dynamite/reference/lfactor.md),
[`random_spec()`](https://docs.ropensci.org/dynamite/reference/random_spec.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
# Two channel model with varying effects, with explicit lower bounds for the
# random walk prior standard deviations, with noncentered parameterization
# for the first channel and centered for the second channel.
obs(y ~ 1, family = "gaussian") + obs(x ~ 1, family = "gaussian") +
  lags(type = "varying") +
  splines(
    df = 20, degree = 3, lb_tau = c(0, 0.1),
    noncentered = c(TRUE, FALSE)
  )
#>   Family   Formula
#> y gaussian y ~ 1  
#> x gaussian x ~ 1  
#> 
#> Lagged responses added as varying predictors with: k = 1
```
