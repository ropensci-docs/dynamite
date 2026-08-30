# Get Parameter Dimensions of the dynamite Model

Extracts the names and dimensions of all parameters used in the
`dynamite` model. See also
[`get_parameter_types()`](https://docs.ropensci.org/dynamite/reference/get_parameter_types.md)
and
[`get_parameter_names()`](https://docs.ropensci.org/dynamite/reference/get_parameter_names.md).
The returned dimensions match those of the `stanfit` element of the
`dynamitefit` object. When applied to `dynamiteformula` objects, the
model is compiled and sampled for 1 iteration to get the parameter
dimensions.

## Usage

``` r
get_parameter_dims(x, ...)

# S3 method for class 'dynamiteformula'
get_parameter_dims(x, data, time, group = NULL, ...)

# S3 method for class 'dynamitefit'
get_parameter_dims(x, ...)
```

## Arguments

- x:

  \[`dynamiteformula` or `dynamitefit`\]  
  The model formula or an existing `dynamitefit` object. See
  [`dynamiteformula()`](https://docs.ropensci.org/dynamite/reference/dynamiteformula.md)
  and
  [`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md).

- ...:

  Ignored.

- data:

  \[`data.frame`,
  [`tibble::tibble`](https://tibble.tidyverse.org/reference/tibble.html),
  or
  [`data.table::data.table`](https://rdrr.io/pkg/data.table/man/data.table.html)\]  
  The data that contains the variables in the model in long format.
  Supported column types are `integer`, `logical`, `double`, and
  `factor`. Columns of type `character` will be converted to factors.
  Unused factor levels will be dropped. The `data` can contain missing
  values which will simply be ignored in the estimation in a case-wise
  fashion (per time-point and per channel). Input `data` is converted to
  channel specific matrix representations via
  [`stats::model.matrix.lm()`](https://rdrr.io/r/stats/model.matrix.html).

- time:

  \[`character(1)`\]  
  A column name of `data` that denotes the time index of observations.
  If this variable is a factor, the integer representation of its levels
  are used internally for defining the time indexing.

- group:

  \[`character(1)`\]  
  A column name of `data` that denotes the unique groups or `NULL`
  corresponding to a scenario without any groups. If `group` is `NULL`,
  a new column `.group` is created with constant value `1L` is created
  indicating that all observations belong to the same group. In case of
  name conflicts with `data`, see the `group_var` element of the return
  object to get the column name of the new variable.

## Value

A named list with all parameter dimensions of the input model.

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
[`get_parameter_names()`](https://docs.ropensci.org/dynamite/reference/get_parameter_names.md),
[`get_parameter_types()`](https://docs.ropensci.org/dynamite/reference/get_parameter_types.md),
[`ndraws.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/ndraws.dynamitefit.md),
[`nobs.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/nobs.dynamitefit.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
get_parameter_dims(multichannel_example_fit)
#> $beta_g
#> [1] 2
#> 
#> $a_g
#> [1] 1
#> 
#> $sigma_g
#> [1] 1
#> 
#> $beta_p
#> [1] 3
#> 
#> $a_p
#> [1] 1
#> 
#> $beta_b
#> [1] 5
#> 
#> $a_b
#> [1] 1
#> 
```
