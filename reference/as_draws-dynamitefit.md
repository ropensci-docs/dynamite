# Convert `dynamite` Output to `draws_df` Format

Converts the output from a
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md)
call to a `draws_df` format of the posterior package, enabling the use
of diagnostics and plotting methods of posterior and bayesplot packages.
Note that this function returns variables in a wide format, whereas
[`as.data.frame.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.frame.dynamitefit.md)
uses the long format.

## Usage

``` r
# S3 method for class 'dynamitefit'
as_draws_df(
  x,
  parameters = NULL,
  responses = NULL,
  types = NULL,
  times = NULL,
  groups = NULL,
  ...
)

# S3 method for class 'dynamitefit'
as_draws(x, parameters = NULL, responses = NULL, types = NULL, ...)
```

## Arguments

- x:

  \[`dynamitefit`\]  
  The model fit object.

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

- types:

  \[[`character()`](https://rdrr.io/r/base/character.html)\]  
  Type(s) of the parameters for which the samples should be extracted.
  See details of possible values. Default is all values listed in
  details except spline coefficients `omega`. This argument is mutually
  exclusive with `parameters`.

- times:

  \[[`double()`](https://rdrr.io/r/base/double.html)\]  
  Time point(s) to keep. If `NULL` (the default), all time points are
  kept.

- groups:

  \[[`character()`](https://rdrr.io/r/base/character.html)\]  
  Group name(s) to keep. If `NULL` (the default), all groups are kept.

- ...:

  Ignored.

## Value

A `draws_df` object.

A `draws_df` object.

## Details

You can use the arguments `parameters`, `responses` and `types` to
extract only a subset of the model parameters (i.e., only certain types
of parameters related to a certain response variable).

See potential values for the types argument in
[`as.data.frame.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.frame.dynamitefit.md)
and
[`get_parameter_names()`](https://docs.ropensci.org/dynamite/reference/get_parameter_names.md)
for potential values for `parameters` argument.

## See also

Model outputs
[`as.data.frame.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.frame.dynamitefit.md),
[`as.data.table.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.table.dynamitefit.md),
[`coef.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/coef.dynamitefit.md),
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
as_draws(gaussian_example_fit, types = c("sigma", "beta"))
#> # A draws_df: 100 iterations, 2 chains, and 2 variables
#>    beta_y_z sigma_y
#> 1         2    0.19
#> 2         2    0.19
#> 3         2    0.19
#> 4         2    0.19
#> 5         2    0.20
#> 6         2    0.20
#> 7         2    0.20
#> 8         2    0.20
#> 9         2    0.20
#> 10        2    0.20
#> # ... with 190 more draws
#> # ... hidden reserved variables {'.chain', '.iteration', '.draw'}

# Compute MCMC diagnostics using the posterior package
posterior::summarise_draws(as_draws(gaussian_example_fit))
#> # A tibble: 143 × 10
#>    variable      mean median     sd    mad      q5   q95  rhat ess_bulk ess_tail
#>    <chr>        <dbl>  <dbl>  <dbl>  <dbl>   <dbl> <dbl> <dbl>    <dbl>    <dbl>
#>  1 alpha_y[2]  0.0579 0.0594 0.0301 0.0318 0.00740 0.102 1.01      144.     120.
#>  2 alpha_y[3]  0.0973 0.0955 0.0452 0.0478 0.0268  0.170 1.01      225.     191.
#>  3 alpha_y[4]  0.169  0.168  0.0407 0.0416 0.106   0.234 1.02      202.     188.
#>  4 alpha_y[5]  0.264  0.263  0.0410 0.0429 0.202   0.329 0.998     285.     218.
#>  5 alpha_y[6]  0.303  0.300  0.0392 0.0391 0.245   0.374 1.01      278.     154.
#>  6 alpha_y[7]  0.332  0.335  0.0384 0.0397 0.265   0.390 1.01      223.     114.
#>  7 alpha_y[8]  0.422  0.423  0.0348 0.0302 0.365   0.482 1.00      247.     163.
#>  8 alpha_y[9]  0.459  0.456  0.0382 0.0381 0.390   0.520 0.997     207.     220.
#>  9 alpha_y[10] 0.414  0.414  0.0433 0.0456 0.350   0.494 1.02      126.     165.
#> 10 alpha_y[11] 0.405  0.407  0.0412 0.0433 0.340   0.479 1.00      196.     231.
#> # ℹ 133 more rows
```
