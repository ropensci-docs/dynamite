# Model Fit for the Simulated Categorical Multivariate Panel Data

A `dynamitefit` object obtained by running `dynamite` on the
`categorical_example` dataset as


    set.seed(1)
    library(dynamite)
    f <- obs(x ~ z + lag(x) + lag(y), family = "categorical") +
      obs(y ~ z + lag(x) + lag(y), family = "categorical")
    categorical_example_fit <- dynamite(
      f,
      data = categorical_example,
      time = "time",
      group = "id",
      chains = 1,
      refresh = 0,
      thin = 5,
      save_warmup = FALSE
    )

Note the small number of samples due to size restrictions on CRAN.

## Usage

``` r
categorical_example_fit
```

## Format

A `dynamitefit` object.

## Source

The data was generated via `categorical_example_fit.R` in
<https://github.com/ropensci/dynamite/tree/main/data-raw/>

## See also

Example models
[`categorical_example`](https://docs.ropensci.org/dynamite/reference/categorical_example.md),
[`gaussian_example`](https://docs.ropensci.org/dynamite/reference/gaussian_example.md),
[`gaussian_example_fit`](https://docs.ropensci.org/dynamite/reference/gaussian_example_fit.md),
[`multichannel_example`](https://docs.ropensci.org/dynamite/reference/multichannel_example.md),
[`multichannel_example_fit`](https://docs.ropensci.org/dynamite/reference/multichannel_example_fit.md)
