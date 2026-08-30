# Define a Common Latent Factor for the dynamite Model.

This function can be used as part of a
[`dynamiteformula()`](https://docs.ropensci.org/dynamite/reference/dynamiteformula.md)
to define a common latent factor component. The latent factor is modeled
as a spline similarly as a time-varying intercept, but instead of having
equal effect on each group, there is an additional loading variable for
each group so that in the linear predictor we have a term \\\lambda_i
\psi_t\\ for each group \\i\\.

## Usage

``` r
lfactor(
  responses = NULL,
  nonzero_lambda = TRUE,
  correlated = TRUE,
  noncentered_psi = FALSE,
  flip_sign = TRUE
)
```

## Arguments

- responses:

  \[[`character()`](https://rdrr.io/r/base/character.html)\]  
  Names of the responses that the factor should affect. Default is all
  responses defined with `obs` except categorical responses, which do
  not (yet) support the factor component.

- nonzero_lambda:

  \[[`logical()`](https://rdrr.io/r/base/logical.html)\]  
  If `TRUE` (the default), assumes that the mean of factor loadings is
  nonzero or not. Should be a logical vector matching the length of
  `responses` or a single logical value in case `responses` is `NULL`.
  See details.

- correlated:

  \[[`logical()`](https://rdrr.io/r/base/logical.html)\]  
  If `TRUE` (the default), the latent factors are assumed to be
  correlated between channels.

- noncentered_psi:

  \[`logical(1)`\]  
  If `TRUE`, uses a noncentered parametrization for spline coefficients
  of all the factors. The number of knots is based
  [`splines()`](https://docs.ropensci.org/dynamite/reference/splines.md)
  call. Default is `FALSE`.

- flip_sign:

  \[`logical(1)`\]  
  If `TRUE` (default), try to avoid multimodality due to sign-switching
  by defining the sign of \\\lambda\\ and \\\psi\\ based on the mean of
  \\\omega_1,\ldots, \omega_D\\ coefficients. This only affects channels
  with `nonzero_lambda = FALSE`. If the true mean of \\\omega\\s is
  close to zero, this might not help, in which case it is better to set
  `flip_sign = FALSE` and post-process the samples in other ways (or use
  only one chain and/or suitable initial values). This argument is
  common to all factors.

## Value

An object of class `latent_factor`.

## See also

Model formula construction
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md),
[`dynamiteformula()`](https://docs.ropensci.org/dynamite/reference/dynamiteformula.md),
[`lags()`](https://docs.ropensci.org/dynamite/reference/lags.md),
[`random_spec()`](https://docs.ropensci.org/dynamite/reference/random_spec.md),
[`splines()`](https://docs.ropensci.org/dynamite/reference/splines.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
# three channel model with common factor affecting for responses x and y
obs(y ~ 1, family = "gaussian") +
  obs(x ~ 1, family = "poisson") +
  obs(z ~ 1, family = "gaussian") +
  lfactor(
    responses = c("y", "x"), nonzero_lambda = c(TRUE, FALSE),
    correlated = TRUE, noncentered_psi = FALSE
  )
#>   Family   Formula
#> y gaussian y ~ 1  
#> x poisson  x ~ 1  
#> z gaussian z ~ 1  
```
