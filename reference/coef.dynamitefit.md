# Extract Regression Coefficients of a dynamite Model

Extracts either time-varying or time-invariant parameters of the model.

## Usage

``` r
# S3 method for class 'dynamitefit'
coef(
  object,
  types = c("alpha", "beta", "delta"),
  parameters = NULL,
  responses = NULL,
  times = NULL,
  groups = NULL,
  summary = TRUE,
  probs = c(0.05, 0.95),
  ...
)
```

## Arguments

- object:

  \[`dynamitefit`\]  
  The model fit object.

- types:

  \[[`character()`](https://rdrr.io/r/base/character.html)\]  
  Type(s) of the parameters for which the samples should be extracted.
  See details of possible values. Default is all values listed in
  details except spline coefficients `omega`. This argument is mutually
  exclusive with `parameters`.

- parameters:

  \[[`character()`](https://rdrr.io/r/base/character.html)\]  
  Parameter(s) for which the samples should be extracted. Possible
  options can be found with function
  [`get_parameter_names()`](https://docs.ropensci.org/dynamite/reference/get_parameter_names.md).
  Default is all parameters of specific type for all responses. This
  argument is mutually exclusive with `types`.

- responses:

  \[[`character()`](https://rdrr.io/r/base/character.html)\]  
  Response(s) for which the samples should be extracted. Possible
  options are elements of `unique(x$priors$response)`, and the default
  is this entire vector. Ignored if the argument `parameters` is
  supplied. `omega_alpha`, and `omega_psi`. See also
  [`get_parameter_types()`](https://docs.ropensci.org/dynamite/reference/get_parameter_types.md).

- times:

  \[[`double()`](https://rdrr.io/r/base/double.html)\]  
  Time point(s) to keep. If `NULL` (the default), all time points are
  kept.

- groups:

  \[[`character()`](https://rdrr.io/r/base/character.html)\]  
  Group name(s) to keep. If `NULL` (the default), all groups are kept.

- summary:

  \[`logical(1)`\]  
  If `TRUE` (default), returns posterior mean, standard deviation, and
  posterior quantiles (as defined by the `probs` argument) for all
  parameters. If `FALSE`, returns the posterior samples instead.

- probs:

  \[[`numeric()`](https://rdrr.io/r/base/numeric.html)\]  
  Quantiles of interest. Default is `c(0.05, 0.95)`.

- ...:

  Ignored.

## Value

A `tibble` containing either samples or summary statistics of the model
parameters in a long format.

## See also

Model outputs
[`as.data.frame.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.frame.dynamitefit.md),
[`as.data.table.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.table.dynamitefit.md),
[`as_draws_df.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as_draws-dynamitefit.md),
[`confint.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/confint.dynamitefit.md),
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md),
[`get_code()`](https://docs.ropensci.org/dynamite/reference/get_code.md),
[`get_data()`](https://docs.ropensci.org/dynamite/reference/get_data.md),
[`get_parameter_dims()`](https://docs.ropensci.org/dynamite/reference/get_parameter_dims.md),
[`get_parameter_names()`](https://docs.ropensci.org/dynamite/reference/get_parameter_names.md),
[`get_parameter_types()`](https://docs.ropensci.org/dynamite/reference/get_parameter_types.md),
[`ndraws.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/ndraws.dynamitefit.md),
[`nobs.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/nobs.dynamitefit.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
betas <- coef(gaussian_example_fit, type = "beta")
deltas <- coef(gaussian_example_fit, type = "delta")
```
