# Extract Samples From a `dynamitefit` Object as a Data Table

Provides a `data.table` representation of the posterior samples of the
model parameters. See
[`as.data.frame.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.frame.dynamitefit.md)
for details.

## Usage

``` r
# S3 method for class 'dynamitefit'
as.data.table(
  x,
  keep.rownames = FALSE,
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

- keep.rownames:

  \[`logical(1)`\]  
  Not used.

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

A `data.table` containing either samples or summary statistics of the
model parameters.

## See also

Model outputs
[`as.data.frame.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.frame.dynamitefit.md),
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
as.data.table(
  gaussian_example_fit,
  responses = "y",
  types = "beta",
  summary = FALSE
)
#>      parameter    value  time category group response   type .draw .iteration
#>         <char>    <num> <int>   <char> <int>   <char> <char> <int>      <int>
#>   1:  beta_y_z 1.971788    NA     <NA>    NA        y   beta     1          1
#>   2:  beta_y_z 1.956853    NA     <NA>    NA        y   beta     2          2
#>   3:  beta_y_z 1.953325    NA     <NA>    NA        y   beta     3          3
#>   4:  beta_y_z 1.962095    NA     <NA>    NA        y   beta     4          4
#>   5:  beta_y_z 1.967552    NA     <NA>    NA        y   beta     5          5
#>  ---                                                                         
#> 196:  beta_y_z 1.960357    NA     <NA>    NA        y   beta   196         96
#> 197:  beta_y_z 1.967019    NA     <NA>    NA        y   beta   197         97
#> 198:  beta_y_z 1.968887    NA     <NA>    NA        y   beta   198         98
#> 199:  beta_y_z 1.960676    NA     <NA>    NA        y   beta   199         99
#> 200:  beta_y_z 1.961960    NA     <NA>    NA        y   beta   200        100
#>      .chain
#>       <int>
#>   1:      1
#>   2:      1
#>   3:      1
#>   4:      1
#>   5:      1
#>  ---       
#> 196:      2
#> 197:      2
#> 198:      2
#> 199:      2
#> 200:      2
```
