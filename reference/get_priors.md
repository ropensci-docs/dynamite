# Get Prior Definitions of a dynamite Model

Extracts the priors used in the `dynamite` model as a data frame. You
can then alter the priors by changing the contents of the `prior` column
and supplying this data frame to `dynamite` function using the argument
`priors`. See vignettes for details.

## Usage

``` r
get_priors(x, ...)

# S3 method for class 'dynamiteformula'
get_priors(x, data, time, group = NULL, ...)

# S3 method for class 'dynamitefit'
get_priors(x, ...)
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

A `data.frame` containing the prior definitions.

## Note

Only the `prior` column of the output should be altered when defining
the user-defined priors for `dynamite`.

## See also

Model fitting
[`dynamice()`](https://docs.ropensci.org/dynamite/reference/dynamice.md),
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md),
[`update.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/update.dynamitefit.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
d <- data.frame(y = rnorm(10), x = 1:10, time = 1:10, id = 1)
get_priors(obs(y ~ x, family = "gaussian"),
  data = d, time = "time", group = "id"
)
#> Please install the cmdstanr package to use the CmdStan backend.
#> ℹ Switching to rstan backend.
#>   parameter response            prior  type category
#> 1   alpha_y        y normal(0.075, 2) alpha         
#> 2  beta_y_x        y  normal(0, 0.66)  beta         
#> 3   sigma_y        y   exponential(1) sigma         
```
