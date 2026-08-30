# Model Formula for dynamite

Defines a new observational or a new auxiliary channel for the model
using standard R formula syntax. Formulas of individual response
variables can be joined together via `+`. See 'Details' and the package
vignettes for more information. The function `obs` is a shorthand alias
for `dynamiteformula`, and `aux` is a shorthand alias for
`dynamiteformula(formula, family = "deterministic")`.

## Usage

``` r
dynamiteformula(formula, family, link = NULL)

obs(formula, family, link = NULL)

aux(formula)

# S3 method for class 'dynamiteformula'
e1 + e2

# S3 method for class 'dynamiteformula'
print(x, ...)
```

## Arguments

- formula:

  \[`formula`\]  
  An R formula describing the model.

- family:

  \[`character(1)`\]  
  The family name. See 'Details' for the supported families.

- link:

  \[`character(1)`\]  
  The name of the link function to use or `NULL`. See details for
  supported link functions and default values of specific families.

- e1:

  \[`dynamiteformula`\]  
  A model formula specification.

- e2:

  \[`dynamiteformula`\]  
  A model formula specification.

- x:

  \[`dynamiteformula`\]  
  The model formula.

- ...:

  Ignored.

## Value

A `dynamiteformula` object.

## Details

Currently the dynamite package supports the following distributions for
the observations:

- Categorical: `categorical` (with a softmax link using the first
  category as reference). See the documentation of the
  `categorical_logit_glm` in the Stan function reference manual
  <https://mc-stan.org/users/documentation/>.

- Multinomial: `multinomial` (softmax link, first category is
  reference).

- Gaussian: `gaussian` (identity link, parameterized using mean and
  standard deviation).

- Multivariate Gaussian: `mvgaussian` (identity link, parameterized
  using mean vector, standard deviation vector and the Cholesky
  decomposition of the correlation matrix).

- Poisson: `poisson` (log-link, with an optional known offset variable).

- Negative-binomial: `negbin` (log-link, using mean and dispersion
  parameterization, with an optional known offset variable). See the
  documentation on `NegBinomial2` in the Stan function reference manual.

- Bernoulli: `bernoulli` (logit-link).

- Binomial: `binomial` (logit-link).

- Exponential: `exponential` (log-link).

- Gamma: `gamma` (log-link, using mean and shape parameterization).

- Beta: `beta` (logit-link, using mean and precision parameterization).

- Student t: `student` (identity link, parameterized using degrees of
  freedom, location and scale)

The models in the dynamite package are defined by combining the
channel-specific formulas defined via R formula syntax. Each channel is
defined via the `obs` function, and the channels are combined with `+`.
For example a formula
`obs(y ~ lag(x), family = "gaussian") + obs(x ~ z, family = "poisson")`
defines a model with two channels; first we declare that `y` is a
Gaussian variable depending on a previous value of `x` (`lag(x)`), and
then we add a second channel declaring `x` as Poisson distributed
depending on some exogenous variable `z` (for which we do not define any
distribution).

Number of trials for binomial channels should be defined via a `trials`
model component, e.g., `obs(y ~ x + trials(n), family = "binomial")`,
where `n` is a data variable defining the number of trials. For
multinomial channels, the number of trials is automatically defined to
be the sum of the observations over the categories, but can also be
defined using the `trials` component, for example for prediction.

Multivariate channels are defined by providing a single formula for all
components or by providing component-specific formulas separated by a
`|`. The response variables that correspond to the components should be
joined by [`c()`](https://rdrr.io/r/base/c.html). For instance, the
following would define `c(y1, y2)` as multivariate gaussian with `x` as
a predictor for the mean of the first component and `x` and `z` as
predictors for the mean of the second component:
`obs(c(y1, y2) ~ x | x + z, family = "mvgaussian")`. A multinomial
channel should only have a single formula.

In addition to declaring response variables via `obs`, we can also use
the function `aux` to define auxiliary channels which are deterministic
functions of other variables. The values of auxiliary variables are
computed dynamically during prediction, making the use of lagged values
and other transformations possible. The function `aux` also does not use
the `family` argument, which is automatically set to `deterministic` and
is a special channel type of `obs`. Note that lagged values of
deterministic `aux` channels do not imply fixed time points. Instead
they must be given starting values using a special function `init` that
directly initializes the lags to specified values, or by `past` which
computes the initial values based on an R expression. Both `init` and
`past` should appear on the right hand side of the model formula,
separated from the primary defining expression via `|`.

The formula within `obs` can also contain an additional special function
`varying`, which defines the time-varying part of the model equation, in
which case we could write for example
`obs(x ~ z + varying(~ -1 + w), family = "poisson")`, which defines a
model equation with a constant intercept and time-invariant effect of
`z`, and a time-varying effect of `w`. We also remove the duplicate
intercept with `-1` in order to avoid identifiability issues in the
model estimation (we could also define a time varying intercept, in
which case we would write
`obs(x ~ -1 + z + varying(~ w), family = "poisson)`). The part of the
formula not wrapped with `varying` is assumed to correspond to the fixed
part of the model, so
`obs(x ~ z + varying(~ -1 + w), family = "poisson")` is actually
identical to
`obs(x ~ -1 + fixed(~ z) + varying(~ -1 + w), family = "poisson")` and
`obs(x ~ fixed(~ z) + varying(~ -1 + w), family = "poisson")`.

When defining varying effects, we also need to define how the these
time-varying regression coefficient behave. For this, a `splines`
component should be added to the model, e.g.,
`obs(x ~ varying(~ -1 + w), family = "poisson) + splines(df = 10)`
defines a cubic B-spline with 10 degrees of freedom for the time-varying
coefficient corresponding to the `w`. If the model contains multiple
time-varying coefficients, same spline basis is used for all
coefficients, with unique spline coefficients and their standard
deviation.

If the desired model contains lagged predictors of each response in each
channel, these can be quickly added to the model as either
time-invariant or time-varying predictors via
[`lags()`](https://docs.ropensci.org/dynamite/reference/lags.md) instead
of writing them manually for each channel.

It is also possible to define group-specific (random) effects term using
the special syntax `random()` similarly as `varying()`. For example,
`random(~1)` leads to a model where in addition to the common intercept,
each individual/group has their own intercept with zero-mean normal
prior and unknown standard deviation analogously with the typical mixed
models. An additional model component
[`random_spec()`](https://docs.ropensci.org/dynamite/reference/random_spec.md)
can be used to define whether the random effects are allowed to
correlate within and across channels and whether to use centered or
noncentered parameterization for the random effects.

## See also

Model formula construction
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md),
[`lags()`](https://docs.ropensci.org/dynamite/reference/lags.md),
[`lfactor()`](https://docs.ropensci.org/dynamite/reference/lfactor.md),
[`random_spec()`](https://docs.ropensci.org/dynamite/reference/random_spec.md),
[`splines()`](https://docs.ropensci.org/dynamite/reference/splines.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
# A single gaussian response channel with a time-varying effect of 'x',
# and a time-varying effect of the lag of 'y' using B-splines with
# 20 degrees of freedom for the coefficients of the time-varying terms.
obs(y ~ -1 + varying(~x), family = "gaussian") +
  lags(type = "varying") +
  splines(df = 20)
#>   Family   Formula             
#> y gaussian y ~ -1 + varying(~x)
#> 
#> Lagged responses added as varying predictors with: k = 1

# A two-channel categorical model with time-invariant predictors
# here, lag terms are specified manually
obs(x ~ z + lag(x) + lag(y), family = "categorical") +
  obs(y ~ z + lag(x) + lag(y), family = "categorical")
#>   Family      Formula                
#> x categorical x ~ z + lag(x) + lag(y)
#> y categorical y ~ z + lag(x) + lag(y)

# The same categorical model as above, but with the lag terms
# added using 'lags'
obs(x ~ z, family = "categorical") +
  obs(y ~ z, family = "categorical") +
  lags(type = "fixed")
#>   Family      Formula
#> x categorical x ~ z  
#> y categorical y ~ z  
#> 
#> Lagged responses added as fixed predictors with: k = 1

# A multichannel model with a gaussian, Poisson and a Bernoulli response and
# an auxiliary channel for the logarithm of 'p' plus one
obs(g ~ lag(g) + lag(logp), family = "gaussian") +
  obs(p ~ lag(g) + lag(logp) + lag(b), family = "poisson") +
  obs(b ~ lag(b) * lag(logp) + lag(b) * lag(g), family = "bernoulli") +
  aux(numeric(logp) ~ log(p + 1))
#>      Family        Formula                                 
#> g    gaussian      g ~ lag(g) + lag(logp)                  
#> p    poisson       p ~ lag(g) + lag(logp) + lag(b)         
#> b    bernoulli     b ~ lag(b) * lag(logp) + lag(b) * lag(g)
#> logp deterministic numeric(logp) ~ log(p + 1)              

data.table::setDTthreads(1) # For CRAN
obs(y ~ x, family = "gaussian") + obs(z ~ w, family = "exponential")
#>   Family      Formula
#> y gaussian    y ~ x  
#> z exponential z ~ w  

data.table::setDTthreads(1) # For CRAN
x <- obs(y ~ x + random(~ 1 + lag(d)), family = "gaussian") +
  obs(z ~ varying(~w), family = "exponential") +
  aux(numeric(d) ~ log(y) | init(c(0, 1))) +
  lags(k = 2) +
  splines(df = 5) +
  random_spec(correlated = FALSE)
#> Warning: Both time-constant and time-varying intercept specified:
#> ℹ Defaulting to time-varying intercept.
print(x)
#>   Family        Formula                            
#> y gaussian      y ~ x + random(~1 + lag(d))        
#> z exponential   z ~ varying(~w)                    
#> d deterministic numeric(d) ~ log(y) | init(c(0, 1))
#> 
#> Lagged responses added as fixed predictors with: k = 2
#> Random effects added for response(s): y
```
