# Extract Samples From a `dynamitefit` Object as a Data Frame

Provides a `data.frame` representation of the posterior samples of the
model parameters.

## Usage

``` r
# S3 method for class 'dynamitefit'
as.data.frame(
  x,
  row.names = NULL,
  optional = FALSE,
  types = NULL,
  parameters = NULL,
  responses = NULL,
  times = NULL,
  groups = NULL,
  summary = FALSE,
  probs = c(0.05, 0.95),
  include_fixed = TRUE,
  ...
)
```

## Arguments

- x:

  \[`dynamitefit`\]  
  The model fit object.

- row.names:

  Ignored.

- optional:

  Ignored.

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
  If `TRUE`, returns posterior mean, standard deviation, and posterior
  quantiles (as defined by the `probs` argument) for all parameters. If
  `FALSE` (default), returns the posterior samples instead.

- probs:

  \[[`numeric()`](https://rdrr.io/r/base/numeric.html)\]  
  Quantiles of interest. Default is `c(0.05, 0.95)`.

- include_fixed:

  \[`logical(1)`\]  
  If `TRUE` (default), time-varying parameters for `1:fixed` time points
  are included in the output as `NA` values. If `FALSE`, fixed time
  points are omitted completely from the output.

- ...:

  Ignored.

## Value

A `tibble` containing either samples or summary statistics of the model
parameters in a long format. For a wide format, see
[`as_draws.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as_draws-dynamitefit.md).

## Details

The arguments `responses` and `types` can be used to extract only a
subset of the model parameters (i.e., only certain types of parameters
related to a certain response variable).

Potential values for the `types` argument are:

- `alpha`  
  Intercept terms (time-invariant or time-varying).

- `beta`  
  Time-invariant regression coefficients.

- `cutpoint`  
  Cutpoints for ordinal regression.

- `delta`  
  Time-varying regression coefficients.

- `nu`  
  Group-level random effects.

- `lambda`  
  Factor loadings.

- `kappa`  
  Contribution of the latent factor loadings in the total variation.

- `psi`  
  Latent factors.

- `tau`  
  Standard deviations of the spline coefficients of `delta`.

- `tau_alpha`  
  Standard deviations of the spline coefficients of time-varying
  `alpha`.

- `sigma_nu`  
  Standard deviations of the random effects `nu`.

- `corr_nu`  
  Pairwise within-group correlations of random effects `nu`. Samples of
  the full correlation matrix can be extracted manually as
  `rstan::extract(fit$stanfit, pars = "corr_matrix_nu")` if necessary.

- `sigma_lambda`  
  Standard deviations of the latent factor loadings `lambda`.

- `corr_psi`  
  Pairwise correlations of the noise terms of the latent factors.
  Samples of the full correlation matrix can be extracted manually as
  `rstan::extract(fit$stanfit, pars = "corr_matrix_psi")` if necessary.

- `sigma`  
  Standard deviations of Gaussian responses.

- `corr`  
  Pairwise correlations of multivariate Gaussian responses.

- `phi`  
  Describes various distributional parameters, such as:

  - Dispersion parameter of the Negative Binomial distribution.

  - Shape parameter of the Gamma distribution.

  - Precision parameter of the Beta distribution.

  - Degrees of freedom of the Student t-distribution.

- `omega`  
  Spline coefficients of the regression coefficients `delta`.

- `omega_alpha`  
  Spline coefficients of time-varying `alpha`.

- `omega_psi`  
  Spline coefficients of the latent factors `psi`. Note that in case of
  `nonzero_lambda = FALSE`, mean of these are used to flip the sign of
  `psi` to avoid multimodality due to sign-switching, but `omega_psi`
  variables are not modified.

- `zeta`  
  Total variation of latent factors, i.e., the sum of `sigma_lambda` and
  `tau_psi`.

## See also

Model outputs
[`as.data.table.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.table.dynamitefit.md),
[`as_draws_df.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as_draws-dynamitefit.md),
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
as.data.frame(
  gaussian_example_fit,
  responses = "y",
  types = "beta"
)
#> # A tibble: 200 × 10
#>    parameter value  time category group response type  .draw .iteration .chain
#>    <chr>     <dbl> <int> <chr>    <int> <chr>    <chr> <int>      <int>  <int>
#>  1 beta_y_z   1.97    NA NA          NA y        beta      1          1      1
#>  2 beta_y_z   1.96    NA NA          NA y        beta      2          2      1
#>  3 beta_y_z   1.95    NA NA          NA y        beta      3          3      1
#>  4 beta_y_z   1.96    NA NA          NA y        beta      4          4      1
#>  5 beta_y_z   1.97    NA NA          NA y        beta      5          5      1
#>  6 beta_y_z   1.96    NA NA          NA y        beta      6          6      1
#>  7 beta_y_z   1.97    NA NA          NA y        beta      7          7      1
#>  8 beta_y_z   1.97    NA NA          NA y        beta      8          8      1
#>  9 beta_y_z   1.95    NA NA          NA y        beta      9          9      1
#> 10 beta_y_z   1.97    NA NA          NA y        beta     10         10      1
#> # ℹ 190 more rows

# Basic summaries can be obtained automatically with summary = TRUE
as.data.frame(
  gaussian_example_fit,
  responses = "y",
  types = "beta",
  summary = TRUE
)
#> # A tibble: 1 × 10
#>   parameter  mean     sd    q5   q95  time group category response type 
#>   <chr>     <dbl>  <dbl> <dbl> <dbl> <int> <int> <chr>    <chr>    <chr>
#> 1 beta_y_z   1.97 0.0122  1.95  1.99    NA    NA NA       y        beta 

# Time-varying coefficients "delta"
as.data.frame(
  gaussian_example_fit,
  responses = "y",
  types = "delta",
  summary = TRUE
)
#> # A tibble: 60 × 10
#>    parameter   mean      sd     q5    q95  time group category response type 
#>    <chr>      <dbl>   <dbl>  <dbl>  <dbl> <int> <int> <chr>    <chr>    <chr>
#>  1 delta_y_x NA     NA      NA     NA         1    NA NA       y        delta
#>  2 delta_y_x -0.161  0.0310 -0.206 -0.109     2    NA NA       y        delta
#>  3 delta_y_x -0.906  0.0308 -0.960 -0.852     3    NA NA       y        delta
#>  4 delta_y_x -0.883  0.0250 -0.921 -0.841     4    NA NA       y        delta
#>  5 delta_y_x -0.915  0.0218 -0.951 -0.881     5    NA NA       y        delta
#>  6 delta_y_x -1.06   0.0214 -1.10  -1.03      6    NA NA       y        delta
#>  7 delta_y_x -1.14   0.0224 -1.18  -1.10      7    NA NA       y        delta
#>  8 delta_y_x -0.998  0.0208 -1.03  -0.966     8    NA NA       y        delta
#>  9 delta_y_x -0.905  0.0244 -0.945 -0.867     9    NA NA       y        delta
#> 10 delta_y_x -1.01   0.0221 -1.05  -0.975    10    NA NA       y        delta
#> # ℹ 50 more rows

# Obtain summaries for a specific parameters
as.data.frame(
  gaussian_example_fit,
  parameters = c("tau_y_x", "sigma_y"),
  summary = TRUE
)
#> # A tibble: 2 × 10
#>   parameter  mean      sd    q5   q95  time group category response type 
#>   <chr>     <dbl>   <dbl> <dbl> <dbl> <int> <int> <chr>    <chr>    <chr>
#> 1 tau_y_x   0.368 0.0746  0.257 0.508    NA    NA NA       y        tau  
#> 2 sigma_y   0.198 0.00397 0.192 0.205    NA    NA NA       y        sigma
```
