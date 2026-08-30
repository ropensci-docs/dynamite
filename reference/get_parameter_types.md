# Get Parameter Types of the dynamite Model

Extracts all parameter types of used in the `dynamitefit` object. See
[`as.data.frame.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.frame.dynamitefit.md)
for explanations of different types.

## Usage

``` r
get_parameter_types(x, ...)

# S3 method for class 'dynamitefit'
get_parameter_types(x, ...)
```

## Arguments

- x:

  \[`dynamitefit`\]  
  The model fit object.

- ...:

  Ignored.

## Value

A `character` vector with all parameter types of the input model.

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
[`get_parameter_names()`](https://docs.ropensci.org/dynamite/reference/get_parameter_names.md),
[`ndraws.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/ndraws.dynamitefit.md),
[`nobs.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/nobs.dynamitefit.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
get_parameter_types(multichannel_example_fit)
#> [1] "alpha" "beta"  "sigma"
```
