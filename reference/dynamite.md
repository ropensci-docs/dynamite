# Estimate a Bayesian Dynamic Multivariate Panel Model

Fit a Bayesian dynamic multivariate panel model (DMPM) using Stan for
Bayesian inference. The dynamite package supports a wide range of
distributions and allows the user to flexibly customize the priors for
the model parameters. The dynamite model is specified using standard R
formula syntax via
[`dynamiteformula()`](https://docs.ropensci.org/dynamite/reference/dynamiteformula.md).
For more information and examples, see 'Details' and the package
vignettes.

The `formula` method returns the model definition as a quoted
expression.

Information on the estimated `dynamite` model can be obtained via
[`print()`](https://rdrr.io/r/base/print.html) including the following:
The model formula, the data, the smallest effective sample sizes,
largest Rhat and summary statistics of the time-invariant and
group-invariant model parameters.

The [`summary()`](https://rdrr.io/r/base/summary.html) method provides
statistics of the posterior samples of the model; this is an alias of
[`as.data.frame.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.frame.dynamitefit.md)
with `summary = TRUE`.

## Usage

``` r
dynamite(
  dformula,
  data,
  time,
  group = NULL,
  priors = NULL,
  backend = "cmdstanr",
  verbose = TRUE,
  verbose_stan = FALSE,
  stanc_options = list("O0"),
  threads_per_chain = 1L,
  grainsize = NULL,
  custom_stan_model = NULL,
  debug = NULL,
  interval = 1L,
  ...
)

# S3 method for class 'dynamitefit'
formula(x, ...)

# S3 method for class 'dynamitefit'
print(x, full_diagnostics = FALSE, ...)

# S3 method for class 'dynamitefit'
summary(object, ...)
```

## Arguments

- dformula:

  \[`dynamiteformula`\]  
  The model formula. See
  [`dynamiteformula()`](https://docs.ropensci.org/dynamite/reference/dynamiteformula.md)
  and 'Details'.

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

- priors:

  \[`data.frame`\]  
  An optional data frame with prior definitions. See
  [`get_priors()`](https://docs.ropensci.org/dynamite/reference/get_priors.md)
  and 'Details'.

- backend:

  \[`character(1)`\]  
  Defines the backend interface to Stan, should be either `"cmdstanr"`
  (the default) or `"rstan"`. Note that `cmdstanr` needs to be installed
  separately as it is not on CRAN. It also needs the actual `CmdStan`
  software. See <https://mc-stan.org/cmdstanr/> for details. Defaults to
  `"rstan"` if `"cmdstanr"` cannot be used.

- verbose:

  \[`logical(1)`\]  
  All warnings and messages are suppressed if set to `FALSE`. Defaults
  to `TRUE`. Setting this to `FALSE` will also disable checks for
  perfect collinearity in the model matrix.

- verbose_stan:

  \[`logical(1)`\]  
  This is the `verbose` argument for
  [`rstan::sampling()`](https://mc-stan.org/rstan/reference/stanmodel-method-sampling.html).
  Defaults to `FALSE`.

- stanc_options:

  \[[`list()`](https://rdrr.io/r/base/list.html)\]  
  This is the `stanc_options` argument passed to the compile method of a
  `CmdStanModel` object via `cmdstan_model()` when
  `backend = "cmdstanr"`. Defaults to `list("O0")`. To enable level one
  compiler optimizations, use `list("O1")`. See
  <https://mc-stan.org/cmdstanr/reference/cmdstan_model.html> for
  details.

- threads_per_chain:

  \[`integer(1)`\]  
  A Positive integer defining the number of parallel threads to use
  within each chain. Default is `1`. See
  [`rstan::rstan_options()`](https://mc-stan.org/rstan/reference/rstan_options.html)
  and <https://mc-stan.org/cmdstanr/reference/model-method-sample.html>
  for details.

- grainsize:

  \[`integer(1)`\]  
  A positive integer defining the suggested size of the partial sums
  when using within-chain parallelization. Default is number of time
  points divided by `threads_per_chain`. Setting this to `1` leads the
  workload division entirely to the internal scheduler. The performance
  of the within-chain parallelization can be sensitive to the choice of
  `grainsize`, see Stan manual on reduce-sum for details.

- custom_stan_model:

  \[`character(1)`\]  
  An optional character string that either contains a customized Stan
  model code or a path to a `.stan` file that contains the code. Using
  this will override the generated model code. For expert users only.

- debug:

  \[[`list()`](https://rdrr.io/r/base/list.html)\]  
  A named list of form `name = TRUE` indicating additional objects in
  the environment of the `dynamite` function which are added to the
  return object. Additionally, values `no_compile = TRUE` and
  `no_sampling = TRUE` can be used to skip the compilation of the Stan
  code and sampling steps respectively. This can be useful for debugging
  when combined with `model_code = TRUE`, which adds the Stan model code
  to the return object.

- interval:

  \[`integer(1)`\]  
  This arguments acts as an offset for the evaluation of lagged
  observations when measurements are not available at every time point.
  For example, if measurements are only available at every second time
  point, setting `interval = 2` means that a lag of order `k` will
  instead use the observation at `2 * k` time units in the past. The
  default value is `1` meaning that there is a one-to-one correspondence
  between the lag order and the time scale. For expert users only.

- ...:

  For `dynamite()`, additional arguments to
  [`rstan::sampling()`](https://mc-stan.org/rstan/reference/stanmodel-method-sampling.html)
  or the `$sample()` method of the `CmdStanModel` object (see
  <https://mc-stan.org/cmdstanr/reference/model-method-sample.html>),
  such as `chains` and `cores` (`chains` and `parallel_chains` in
  `cmdstanr`). For [`summary()`](https://rdrr.io/r/base/summary.html),
  additional arguments to
  [`as.data.frame.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.frame.dynamitefit.md).
  For [`print()`](https://rdrr.io/r/base/print.html), further arguments
  to the print method for tibbles (see
  [tibble::formatting](https://tibble.tidyverse.org/reference/formatting.html)).
  Not used for [`formula()`](https://rdrr.io/r/stats/formula.html).

- x:

  \[`dynamitefit`\]  
  The model fit object.

- full_diagnostics:

  By default, the effective sample size (ESS) and Rhat are computed only
  for the time- and group-invariant parameters
  (`full_diagnostics = FALSE`). Setting this to `TRUE` computes ESS and
  Rhat values for all model parameters, which can take some time for
  complex models.

- object:

  \[`dynamitefit`\]  
  The model fit object.

## Value

`dynamite` returns a `dynamitefit` object which is a list containing the
following components:

- `stanfit`  
  A `stanfit` object, see
  [`rstan::sampling()`](https://mc-stan.org/rstan/reference/stanmodel-method-sampling.html)
  for details.

- `dformulas`  
  A list of `dynamiteformula` objects for internal use.

- `data`  
  A processed version of the input `data`.

- `data_name`  
  Name of the input data object.

- `stan`  
  A `list` containing various elements related to Stan model
  construction and sampling.

- `group_var`  
  Name of the variable defining the groups.

- `time_var`  
  Name of the variable defining the time index.

- `priors`  
  Data frame containing the used priors.

- `backend`  
  Either `"rstan"` or `"cmdstanr"` indicating which package was used in
  sampling.

- `permutation`  
  Randomized permutation of the posterior draws.

- `call`  
  Original function call as an object of class `call`.

`formula` returns a quoted expression.

`print` returns `x` invisibly.

`summary` returns a `data.frame`.

## Details

The best-case scalability of `dynamite` in terms of data size should be
approximately linear in terms of number of time points and and number of
groups, but as wall-clock time of the MCMC algorithms provided by Stan
can depend on the discrepancy of the data and the model (and the
subsequent shape of the posterior), this can vary greatly.

## References

Santtu Tikka and Jouni Helske (2025). dynamite: An R Package for Dynamic
Multivariate Panel Models. *Journal of Statistical Software*, 115(5),
1-42, <doi:10.18637/jss.v115.i05>.

Jouni Helske and Santtu Tikka (2022). Estimating Causal Effects from
Panel Data with Dynamic Multivariate Panel Models. *Advances in Life
Course Research*, 60, 100617. <doi:10.1016/j.alcr.2024.100617>.

## See also

Model fitting
[`dynamice()`](https://docs.ropensci.org/dynamite/reference/dynamice.md),
[`get_priors()`](https://docs.ropensci.org/dynamite/reference/get_priors.md),
[`update.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/update.dynamitefit.md)

Model formula construction
[`dynamiteformula()`](https://docs.ropensci.org/dynamite/reference/dynamiteformula.md),
[`lags()`](https://docs.ropensci.org/dynamite/reference/lags.md),
[`lfactor()`](https://docs.ropensci.org/dynamite/reference/lfactor.md),
[`random_spec()`](https://docs.ropensci.org/dynamite/reference/random_spec.md),
[`splines()`](https://docs.ropensci.org/dynamite/reference/splines.md)

Model outputs
[`as.data.frame.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.frame.dynamitefit.md),
[`as.data.table.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.table.dynamitefit.md),
[`as_draws_df.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as_draws-dynamitefit.md),
[`coef.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/coef.dynamitefit.md),
[`confint.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/confint.dynamitefit.md),
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
# \donttest{
# Please update your rstan and StanHeaders installation before running
# on Windows
if (!identical(.Platform$OS.type, "windows")) {
  fit <- dynamite(
    dformula = obs(y ~ -1 + varying(~x), family = "gaussian") +
      lags(type = "varying") +
      splines(df = 20),
    gaussian_example,
    "time",
    "id",
    chains = 1,
    refresh = 0
  )
}
#> Please install the cmdstanr package to use the CmdStan backend.
#> ℹ Switching to rstan backend.
#> Error in rstan::stan_model(model_code = model_code): Boost not found; call install.packages('BH')
# }

data.table::setDTthreads(1) # For CRAN
formula(gaussian_example_fit)
#> obs(y ~ -1 + z + varying(~x + lag(y)) + random(~1), family = "gaussian") + 
#>     splines(df = 20, degree = 3, lb_tau = 0, noncentered = FALSE, 
#>         override = FALSE) + random_spec(correlated = FALSE, noncentered = TRUE)

data.table::setDTthreads(1) # For CRAN
print(gaussian_example_fit)
#> Model:
#>   Family   Formula                                       
#> y gaussian y ~ -1 + z + varying(~x + lag(y)) + random(~1)
#> 
#> Correlated random effects added for response(s): y
#> 
#> Data: gaussian_example (Number of observations: 1450)
#> Grouping variable: id (Number of groups: 50)
#> Time index variable: time (Number of time points: 30)
#> 
#> NUTS sampler diagnostics:
#> 
#> No divergences, saturated max treedepths or low E-BFMIs.
#> 
#> Smallest bulk-ESS: 72 (alpha_y[28])
#> Smallest tail-ESS: 81 (omega_alpha_y_d3)
#> Largest Rhat: 1.035 (delta_y_y_lag1[28])
#> 
#> Elapsed time (seconds):
#>         warmup sample
#> chain:1  6.996  4.193
#> chain:2  7.302  4.083
#> 
#> Summary statistics of the time- and group-invariant parameters:
#> # A tibble: 113 × 10
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
#> # ℹ 103 more rows

data.table::setDTthreads(1) # For CRAN
summary(gaussian_example_fit,
  types = "beta",
  probs = c(0.05, 0.1, 0.9, 0.95)
)
#> # A tibble: 1 × 12
#>   parameter  mean     sd    q5   q10   q90   q95  time group category response
#>   <chr>     <dbl>  <dbl> <dbl> <dbl> <dbl> <dbl> <int> <int> <chr>    <chr>   
#> 1 beta_y_z   1.97 0.0122  1.95  1.95  1.98  1.99    NA    NA NA       y       
#> # ℹ 1 more variable: type <chr>
```
