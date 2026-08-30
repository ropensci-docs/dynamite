# Estimate a Bayesian Dynamic Multivariate Panel Model With Multiple Imputation

Applies multiple imputation using
[`mice::mice()`](https://amices.org/mice/reference/mice.html) to the
supplied `data` and fits a dynamic multivariate panel model to each
imputed data set using
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md).
Posterior samples from each imputation run are combined. When using wide
format imputation, the long format `data` is automatically converted to
a wide format before imputation to preserve the longitudinal structure,
and then converted back to long format for estimation.

## Usage

``` r
dynamice(
  dformula,
  data,
  time,
  group = NULL,
  priors = NULL,
  backend = "rstan",
  verbose = TRUE,
  verbose_stan = FALSE,
  stanc_options = list("O0"),
  threads_per_chain = 1L,
  grainsize = NULL,
  custom_stan_model = NULL,
  interval = 1L,
  debug = NULL,
  mice_args = list(),
  impute_format = "wide",
  keep_imputed = FALSE,
  stan_csv_dir = tempdir(),
  ...
)
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

- interval:

  \[`integer(1)`\]  
  This arguments acts as an offset for the evaluation of lagged
  observations when measurements are not available at every time point.
  For example, if measurements are only available at every second time
  point, setting `interval = 2` means that a lag of order `k` will
  instead use the observation at `2 * k` time units in the past. The
  default value is `1` meaning that there is a one-to-one correspondence
  between the lag order and the time scale. For expert users only.

- debug:

  \[[`list()`](https://rdrr.io/r/base/list.html)\]  
  A named list of form `name = TRUE` indicating additional objects in
  the environment of the `dynamite` function which are added to the
  return object. Additionally, values `no_compile = TRUE` and
  `no_sampling = TRUE` can be used to skip the compilation of the Stan
  code and sampling steps respectively. This can be useful for debugging
  when combined with `model_code = TRUE`, which adds the Stan model code
  to the return object.

- mice_args:

  \[[`list()`](https://rdrr.io/r/base/list.html)\]  
  Arguments passed to
  [`mice::mice()`](https://amices.org/mice/reference/mice.html)
  excluding `data`.

- impute_format:

  \[`character(1)`\]  
  Format of the data that will be passed to the imputation method.
  Should be either `"wide"` (the default) or `"long"` corresponding to
  wide format and long format imputation.

- keep_imputed:

  \[`logical(1)`\]  
  Should the imputed datasets be kept in the return object? The default
  is `FALSE`. If `TRUE`, the imputations will be included in the
  `imputed` field in the return object that is otherwise `NULL`.

- stan_csv_dir:

  \[`character(1)`\]  
  A directory path to output the Stan .csv files when `backend` is
  `"cmdstanr"`. The files are saved here via `$save_output_files()` to
  avoid garbage collection between sampling runs with different imputed
  datasets.

- ...:

  For
  [`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md),
  additional arguments to
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

## See also

Model fitting
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md),
[`get_priors()`](https://docs.ropensci.org/dynamite/reference/get_priors.md),
[`update.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/update.dynamitefit.md)
