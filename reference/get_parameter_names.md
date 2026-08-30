# Get Parameter Names of the dynamite Model

Extracts all parameter names of used in the `dynamitefit` object.

## Usage

``` r
get_parameter_names(x, types = NULL, ...)

# S3 method for class 'dynamitefit'
get_parameter_names(x, types = NULL, ...)
```

## Arguments

- x:

  \[`dynamitefit`\]  
  The model fit object.

- types:

  \[[`character()`](https://rdrr.io/r/base/character.html)\]  
  Extract only names of parameter of a certain type. See
  [`get_parameter_types()`](https://docs.ropensci.org/dynamite/reference/get_parameter_types.md).

- ...:

  Ignored.

## Value

A `character` vector with parameter names of the input model.

## Details

The naming of parameters generally follows style where the name starts
with the parameter type (e.g. beta for time-invariant regression
coefficient), followed by underscore and the name of the response
variable, and in case of time-invariant, time-varying or random effect,
the name of the predictor. An exception to this is spline coefficients
omega, which also contain the number denoting the knot number.

## See also

Model outputs
[`as.data.frame.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.frame.dynamitefit.md),
[`as.data.table.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.table.dynamitefit.md),
[`as_draws_df.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as_draws-dynamitefit.md),
[`coef.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/coef.dynamitefit.md),
[`confint.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/confint.dynamitefit.md),
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md),
[`get_code()`](https://docs.ropensci.org/dynamite/reference/get_code.md),
[`get_data()`](https://docs.ropensci.org/dynamite/reference/get_data.md),
[`get_parameter_dims()`](https://docs.ropensci.org/dynamite/reference/get_parameter_dims.md),
[`get_parameter_types()`](https://docs.ropensci.org/dynamite/reference/get_parameter_types.md),
[`ndraws.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/ndraws.dynamitefit.md),
[`nobs.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/nobs.dynamitefit.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
get_parameter_names(multichannel_example_fit)
#>  [1] "alpha_g"                 "beta_g_g_lag1"          
#>  [3] "beta_g_logp_lag1"        "sigma_g"                
#>  [5] "alpha_p"                 "beta_p_g_lag1"          
#>  [7] "beta_p_logp_lag1"        "beta_p_b_lag1"          
#>  [9] "alpha_b"                 "beta_b_g_lag1"          
#> [11] "beta_b_logp_lag1"        "beta_b_b_lag1"          
#> [13] "beta_b_b_lag1:logp_lag1" "beta_b_b_lag1:g_lag1"   
```
