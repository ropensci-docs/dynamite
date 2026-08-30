# Model Fit for the Simulated Multivariate Panel Data

A `dynamitefit` object obtained by running `dynamite` on the
`multichannel_example` dataset as


    set.seed(1)
    library(dynamite)
    f <- obs(g ~ lag(g) + lag(logp), family = "gaussian") +
      obs(p ~ lag(g) + lag(logp) + lag(b), family = "poisson") +
      obs(b ~ lag(b) * lag(logp) + lag(b) * lag(g), family = "bernoulli") +
      aux(numeric(logp) ~ log(p + 1))
    multichannel_example_fit <- dynamite(
      f,
      data = multichannel_example,
      time = "time",
      group = "id",
      chains = 1,
      cores = 1,
      iter = 2000,
      warmup = 1000,
      init = 0,
      refresh = 0,
      thin = 5,
      save_warmup = FALSE
    )

Note the small number of samples due to size restrictions on CRAN.

## Usage

``` r
multichannel_example_fit
```

## Format

A `dynamitefit` object.

## Source

THe data was generated via `multichannel_example_fit.R` in
<https://github.com/ropensci/dynamite/tree/main/data-raw/>

## See also

Example models
[`categorical_example`](https://docs.ropensci.org/dynamite/reference/categorical_example.md),
[`categorical_example_fit`](https://docs.ropensci.org/dynamite/reference/categorical_example_fit.md),
[`gaussian_example`](https://docs.ropensci.org/dynamite/reference/gaussian_example.md),
[`gaussian_example_fit`](https://docs.ropensci.org/dynamite/reference/gaussian_example_fit.md),
[`multichannel_example`](https://docs.ropensci.org/dynamite/reference/multichannel_example.md)
