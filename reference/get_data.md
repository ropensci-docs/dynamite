# Extract the Model Data of the dynamite Model

Returns the input data to the Stan model. Mostly useful for debugging.

## Usage

``` r
get_data(x, ...)

# S3 method for class 'dynamiteformula'
get_data(x, data, time, group = NULL, ...)

# S3 method for class 'dynamitefit'
get_data(x, ...)
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

A `list` containing the input data to Stan.

## See also

Model outputs
[`as.data.frame.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.frame.dynamitefit.md),
[`as.data.table.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.table.dynamitefit.md),
[`as_draws_df.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as_draws-dynamitefit.md),
[`coef.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/coef.dynamitefit.md),
[`confint.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/confint.dynamitefit.md),
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md),
[`get_code()`](https://docs.ropensci.org/dynamite/reference/get_code.md),
[`get_parameter_dims()`](https://docs.ropensci.org/dynamite/reference/get_parameter_dims.md),
[`get_parameter_names()`](https://docs.ropensci.org/dynamite/reference/get_parameter_names.md),
[`get_parameter_types()`](https://docs.ropensci.org/dynamite/reference/get_parameter_types.md),
[`ndraws.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/ndraws.dynamitefit.md),
[`nobs.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/nobs.dynamitefit.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
d <- data.frame(y = rnorm(10), x = 1:10, time = 1:10, id = 1)
str(get_data(obs(y ~ x, family = "gaussian"),
  data = d, time = "time", group = "id"
))
#> Please install the cmdstanr package to use the CmdStan backend.
#> ℹ Switching to rstan backend.
#> List of 24
#>  $ K_fixed_y        : int 1
#>  $ K_varying_y      : int 0
#>  $ K_random_y       : int 0
#>  $ K_y              : int 1
#>  $ J_fixed_y        : int [1(1d)] 1
#>   ..- attr(*, "dimnames")=List of 1
#>   .. ..$ : chr "x"
#>  $ J_varying_y      : int[0 (1d)] 
#>  $ J_y              : int [1(1d)] 1
#>   ..- attr(*, "dimnames")=List of 1
#>   .. ..$ : chr "x"
#>  $ J_random_y       : int[0 (1d)] 
#>  $ L_fixed_y        : int [1(1d)] 1
#>  $ L_varying_y      : int[0 (1d)] 
#>  $ obs_y            : int [1, 1:10] 1 1 1 1 1 1 1 1 1 1
#>  $ n_obs_y          : int [1:10] 1 1 1 1 1 1 1 1 1 1
#>  $ t_obs_y          : int [1:10(1d)] 1 2 3 4 5 6 7 8 9 10
#>  $ T_obs_y          : int 10
#>  $ y_y              : num [1, 1:10] 0.3898 -0.6212 -2.2147 1.1249 -0.0449 ...
#>  $ beta_prior_pars_y: num [1, 1:2] 0 0.66
#>  $ N                : int 1
#>  $ K                : int 1
#>  $ X                : num [1:10, 1, 1] 1 2 3 4 5 6 7 8 9 10
#>  $ M                : int 0
#>  $ P                : num 0
#>  $ T                : int 10
#>  $ X_m              : num [1(1d)] 1
#>  $ grainsize        : num 10
```
