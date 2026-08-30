# Model Fit for the Simulated Data of a Gaussian Response

A `dynamitefit` object obtained by running `dynamite` on the
`gaussian_example` dataset as


    set.seed(1)
    library(dynamite)
    gaussian_example_fit <- dynamite(
      obs(y ~ -1 + z + varying(~ x + lag(y)) + random(~1), family = "gaussian") +
        random_spec() + splines(df = 20),
      data = gaussian_example,
      time = "time",
      group = "id",
      iter = 2000,
      warmup = 1000,
      thin = 10,
      chains = 2,
      cores = 2,
      refresh = 0,
      save_warmup = FALSE,
      pars = c("omega_alpha_1_y", "omega_raw_alpha_y", "nu_raw", "nu", "L",
        "sigma_nu", "a_y"),
      include = FALSE
    )

Note the very small number of samples due to size restrictions on CRAN.

## Usage

``` r
gaussian_example_fit
```

## Format

A `dynamitefit` object.

## Source

The data was generated via `gaussian_example_fit.R` in
<https://github.com/ropensci/dynamite/tree/main/data-raw/>

## See also

Example models
[`categorical_example`](https://docs.ropensci.org/dynamite/reference/categorical_example.md),
[`categorical_example_fit`](https://docs.ropensci.org/dynamite/reference/categorical_example_fit.md),
[`gaussian_example`](https://docs.ropensci.org/dynamite/reference/gaussian_example.md),
[`multichannel_example`](https://docs.ropensci.org/dynamite/reference/multichannel_example.md),
[`multichannel_example_fit`](https://docs.ropensci.org/dynamite/reference/multichannel_example_fit.md)
