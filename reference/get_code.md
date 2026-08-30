# Extract the Stan Code of the dynamite Model

Returns the Stan code of the model. Mostly useful for debugging or for
building a customized version of the model.

## Usage

``` r
get_code(x, ...)

# S3 method for class 'dynamiteformula'
get_code(x, data, time, group = NULL, blocks = NULL, ...)

# S3 method for class 'dynamitefit'
get_code(x, blocks = NULL, ...)
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

- blocks:

  \[[`character()`](https://rdrr.io/r/base/character.html)\]  
  Stan block names to extract. If `NULL`, extracts the full model code.

## Value

The Stan model blocks as a `character` string.

## See also

Model outputs
[`as.data.frame.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.frame.dynamitefit.md),
[`as.data.table.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.table.dynamitefit.md),
[`as_draws_df.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as_draws-dynamitefit.md),
[`coef.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/coef.dynamitefit.md),
[`confint.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/confint.dynamitefit.md),
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md),
[`get_data()`](https://docs.ropensci.org/dynamite/reference/get_data.md),
[`get_parameter_dims()`](https://docs.ropensci.org/dynamite/reference/get_parameter_dims.md),
[`get_parameter_names()`](https://docs.ropensci.org/dynamite/reference/get_parameter_names.md),
[`get_parameter_types()`](https://docs.ropensci.org/dynamite/reference/get_parameter_types.md),
[`ndraws.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/ndraws.dynamitefit.md),
[`nobs.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/nobs.dynamitefit.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
d <- data.frame(y = rnorm(10), x = 1:10, time = 1:10, id = 1)
cat(get_code(obs(y ~ x, family = "gaussian"),
  data = d, time = "time", group = "id"
))
#> Please install the cmdstanr package to use the CmdStan backend.
#> ℹ Switching to rstan backend.
#> functions {
#> }
#> data {
#>   int<lower=1> T; // number of time points
#>   int<lower=1> N; // number of individuals
#>   int<lower=0> K; // total number of covariates across all channels
#>   array[T] matrix[N, K] X; // covariates as an array of N x K matrices
#>   row_vector[K] X_m; // Means of all covariates at first time point
#>   // number of fixed, varying and random coefficients, and related indices
#>   int<lower=0> K_fixed_y;
#>   int<lower=0> K_y; // K_fixed + K_varying
#>   array[K_fixed_y] int J_fixed_y;
#>   array[K_y] int J_y; // fixed and varying
#>   array[K_fixed_y] int L_fixed_y;
#>   // Parameters of vectorized priors
#>   matrix[K_fixed_y, 2] beta_prior_pars_y;
#>   matrix[N, T] y_y;
#> }
#> transformed data {
#> }
#> parameters {
#>   vector[K_fixed_y] beta_y; // Fixed coefficients
#>   real a_y; // Mean of the first time point
#>   real<lower=0> sigma_y; // SD of the normal distribution
#> }
#> transformed parameters {
#>   // Time-invariant intercept
#>   real alpha_y;
#>   // Define the first alpha using mean a_y
#>   {
#>     vector[K_y] gamma__y;
#>     gamma__y[L_fixed_y] = beta_y;
#>     alpha_y = a_y - X_m[J_y] * gamma__y;
#>   }
#> }
#> model {
#>   a_y ~ normal(-0.63, 2);
#>   beta_y ~ normal(beta_prior_pars_y[, 1], beta_prior_pars_y[, 2]);
#>   sigma_y ~ exponential(1);
#>   {
#>     real ll = 0.0;
#>     vector[K_y] gamma__y;
#>     gamma__y[L_fixed_y] = beta_y;
#>     for (t in 1:T) {
#>       real intercept_y = alpha_y;
#>       ll += normal_id_glm_lupdf(y_y[, t] | X[t][, J_y], intercept_y, gamma__y, sigma_y);
#>     }
#>     target += ll;
#>   }
#> }
#> generated quantities {
#> }
# same as
cat(dynamite(obs(y ~ x, family = "gaussian"),
  data = d, time = "time", group = "id",
  debug = list(model_code = TRUE, no_compile = TRUE)
)$model_code)
#> Please install the cmdstanr package to use the CmdStan backend.
#> ℹ Switching to rstan backend.
#> functions {
#> }
#> data {
#>   int<lower=1> T; // number of time points
#>   int<lower=1> N; // number of individuals
#>   int<lower=0> K; // total number of covariates across all channels
#>   array[T] matrix[N, K] X; // covariates as an array of N x K matrices
#>   row_vector[K] X_m; // Means of all covariates at first time point
#>   // number of fixed, varying and random coefficients, and related indices
#>   int<lower=0> K_fixed_y;
#>   int<lower=0> K_y; // K_fixed + K_varying
#>   array[K_fixed_y] int J_fixed_y;
#>   array[K_y] int J_y; // fixed and varying
#>   array[K_fixed_y] int L_fixed_y;
#>   // Parameters of vectorized priors
#>   matrix[K_fixed_y, 2] beta_prior_pars_y;
#>   matrix[N, T] y_y;
#> }
#> transformed data {
#> }
#> parameters {
#>   vector[K_fixed_y] beta_y; // Fixed coefficients
#>   real a_y; // Mean of the first time point
#>   real<lower=0> sigma_y; // SD of the normal distribution
#> }
#> transformed parameters {
#>   // Time-invariant intercept
#>   real alpha_y;
#>   // Define the first alpha using mean a_y
#>   {
#>     vector[K_y] gamma__y;
#>     gamma__y[L_fixed_y] = beta_y;
#>     alpha_y = a_y - X_m[J_y] * gamma__y;
#>   }
#> }
#> model {
#>   a_y ~ normal(-0.63, 2);
#>   beta_y ~ normal(beta_prior_pars_y[, 1], beta_prior_pars_y[, 2]);
#>   sigma_y ~ exponential(1);
#>   {
#>     real ll = 0.0;
#>     vector[K_y] gamma__y;
#>     gamma__y[L_fixed_y] = beta_y;
#>     for (t in 1:T) {
#>       real intercept_y = alpha_y;
#>       ll += normal_id_glm_lupdf(y_y[, t] | X[t][, J_y], intercept_y, gamma__y, sigma_y);
#>     }
#>     target += ll;
#>   }
#> }
#> generated quantities {
#> }
```
