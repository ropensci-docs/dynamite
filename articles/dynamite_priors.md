# Priors for dynamic multivariate panel models

## Default prior distributions in dynamite

The default priors in `dynamite` are chosen to be relatively
uninformative (i.e., weakly informative) such that they provide
computational stability by slightly regularizing the posterior. The
motivation behind the default priors is thus similar to other popular
Stan-based R packages such as `brms`[^1] and `rstanarm`[^2].

Define \sigma_x=\max(1, \text{SD}(x)), where \text{SD}(x) is the
standard deviation of the predictor variable x over groups and non-fixed
time points (see section “Lagged responses and predictors” in the main
vignette for more information:
[`vignette("dynamite", package = "dynamite")`](https://docs.ropensci.org/dynamite/articles/dynamite.md)).
Define also \sigma_y = \begin{cases} \max(1, \text{SD}(y)), &\text{if
family is gaussian or student} \\ 1, &\text{otherwise} \end{cases},
where \text{SD}(y) is the standard deviation of the response variable as
over groups and non-fixed time points.

### Regression coefficients

We define the default prior for a time-invariant regression coefficients
\beta as well for the first time-varying coefficient \delta_1 as
zero-mean normal distribution with standard deviation of
2\sigma_y/\sigma_x. The two maximums used in definitions of \sigma_y and
\sigma_x ensure that the prior standard deviation is at least 2.

### Intercept

As the posterior correlations between the intercept \alpha and the
regression coefficients \beta and \delta can be cause computational
inefficiencies with gradient-based sampling algorithms of Stan, sampling
of the intercept is performed indirectly via parameter a, so that the
intercept \alpha (or \alpha_1 in case of time-varying intercept) is
constructed as \alpha = a - \bar X^\beta_1\beta - \bar
X_1^\delta\delta_1, where \bar X^\beta and \bar X^\delta are the means
of the corresponding predictors at first (non-fixed) time point. The
prior is then defined for a as a \sim \text{N}(\bar y, 2\sigma_y), where
\bar y is mean of the response variable values at first time point after
applying the link function (except in case of categorical and
multinomial response where \bar y is set to zero).

### Standard deviation parameters

The prior for the standard deviation parameter \tau of the random walk
prior of the spline coefficients is half-normal with the standard
deviation of 2\sigma_y/\sigma_x (with \sigma_x=1 in case of a
time-varying intercept). The same prior distribution is also used for
the the standard deviations of the random effects. Note that this prior
can be too uninformative in some cases especially for the \tau
parameters. A boundary (zero) avoiding \text{Gamma}(2, z) prior with
suitable value of z can often work better for \tau when the default
prior leads to divergences (alternatively, in some occasions switching
between centered and noncentered parameterization in
[`splines()`](https://docs.ropensci.org/dynamite/reference/splines.md)
can help).

### Correlation matrices

The correlation matrix of the random effects and latent factors uses a
\text{LKJ}(1) prior with the Cholesky parameterization, see Stan
documentation of `lkj_corr_cholesky` for details[^3]. The default
corresponds to uniform distribution over valid correlation matrices.

### Parameters related to latent factors

When defining latent factor term with `nonzero_lambda = TRUE`, priors
are set for \zeta and \kappa, with defaults \kappa \sim N(0, 1) and
\kappa \sim Beta(2, 2). These are used to define standard deviation
\sigma\_\lambda of the loadings as \sigma\_\lambda = \kappa \zeta, and
\tau\_\psi = (1 - \kappa)\zeta, the standard deviation of the random
walk prior of \psi_t.

In case where `nonzero_lambda = FALSE`, \tau\_\psi is fixed to one, and
prior is set directly on \$\_\$ as N(0, 1). In both cases, prior on
first \psi_1 \sim N(0, 1).

### Family-specific parameters

For standard deviation parameter of gaussian and student’s t responses,
we use exponential prior with a rate parameter
\frac{1}{2\max(1,\sigma_y)}. The degrees-of-freedom parameter of the
student’s t-distribution has a \text{Gamma}(2, 0.1) prior, whereas for
other family specific parameters we set \phi \sim \text{Exponential}(1).

## Defining priors of dynamite models

While
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md)
can be used with the default prior choices, we recommend to check
whether the defaults make sense for your particular problem and to
codify them accordingly based on the domain knowledge.

Any univariate unbounded continuous distributions supported by Stan can
be used as a prior for univariate model parameters (in case the
parameter is constrained to be positive, the distribution is
automatically truncated). Naturally, any univariate distribution bounded
to the positive real line can be used as a prior for parameters
constrained to be positive (e.g., a standard deviation parameter). See
the Stan function reference at
<https://mc-stan.org/users/documentation/> for possible distributions.
For custom priors, you should first get the default priors with the
[`get_priors()`](https://docs.ropensci.org/dynamite/reference/get_priors.md)
function, and then modify the `priors` column of the obtained data frame
before supplying it to the
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md)
function:

``` r

f <- obs(y ~ -1 + z + varying(~ x + lag(y)) + random(~1 + z), "gaussian") +
  random_spec(correlated = TRUE) + splines(df = 20)
p <- get_priors(f, data = gaussian_example, time = "time", group = "id")
#> Please install the cmdstanr package to use the CmdStan backend.
#> ℹ Switching to rstan backend.
p
#>           parameter response                prior      type category
#> 1  sigma_nu_y_alpha        y       normal(0, 3.1)  sigma_nu         
#> 2      sigma_nu_y_z        y       normal(0, 3.1)  sigma_nu         
#> 3           alpha_y        y     normal(1.5, 3.1)     alpha         
#> 4       tau_alpha_y        y       normal(0, 3.1) tau_alpha         
#> 5          beta_y_z        y       normal(0, 3.1)      beta         
#> 6         delta_y_x        y       normal(0, 3.1)     delta         
#> 7    delta_y_y_lag1        y       normal(0, 1.8)     delta         
#> 8           tau_y_x        y       normal(0, 3.1)       tau         
#> 9      tau_y_y_lag1        y       normal(0, 1.8)       tau         
#> 10          sigma_y        y    exponential(0.65)     sigma         
#> 11             L_nu          lkj_corr_cholesky(1)         L
```

``` r

p$prior[p$type == "sigma_nu"] <- "normal(0, 1)" # change prior for sigma_nu
p$prior[p$parameter == "sigma_y"] <- "student_t(3, 0, 2)" # prior for sigma_y
p
#>           parameter response                prior      type category
#> 1  sigma_nu_y_alpha        y         normal(0, 1)  sigma_nu         
#> 2      sigma_nu_y_z        y         normal(0, 1)  sigma_nu         
#> 3           alpha_y        y     normal(1.5, 3.1)     alpha         
#> 4       tau_alpha_y        y       normal(0, 3.1) tau_alpha         
#> 5          beta_y_z        y       normal(0, 3.1)      beta         
#> 6         delta_y_x        y       normal(0, 3.1)     delta         
#> 7    delta_y_y_lag1        y       normal(0, 1.8)     delta         
#> 8           tau_y_x        y       normal(0, 3.1)       tau         
#> 9      tau_y_y_lag1        y       normal(0, 1.8)       tau         
#> 10          sigma_y        y   student_t(3, 0, 2)     sigma         
#> 11             L_nu          lkj_corr_cholesky(1)         L
```

``` r

fit <- dynamite(
  f,
  data = gaussian_example,
  time = "time",
  group = "id",
  priors = p
)
```

[^1]: https://CRAN.R-project.org/package=brms

[^2]: https://CRAN.R-project.org/package=rstanarm

[^3]: https://mc-stan.org/docs/functions-reference/cholesky-lkj-correlation-distribution.html
