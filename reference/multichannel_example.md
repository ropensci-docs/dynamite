# Simulated Multivariate Panel Data

A simulated multichannel data containing multiple individuals with
multiple response variables of different distributions.

## Usage

``` r
multichannel_example
```

## Format

A data frame with 3000 rows and 5 variables:

- id:

  Variable defining individuals (1 to 50).

- time:

  Variable defining the time point of the measurement (1 to 20).

- g:

  Response variable following gaussian distribution.

- p:

  Response variable following Poisson distribution.

- b:

  Response variable following Bernoulli distribution.

## Source

The data was generated via `multichannel_example.R` in
<https://github.com/ropensci/dynamite/tree/main/data-raw/>

## See also

Example models
[`categorical_example`](https://docs.ropensci.org/dynamite/reference/categorical_example.md),
[`categorical_example_fit`](https://docs.ropensci.org/dynamite/reference/categorical_example_fit.md),
[`gaussian_example`](https://docs.ropensci.org/dynamite/reference/gaussian_example.md),
[`gaussian_example_fit`](https://docs.ropensci.org/dynamite/reference/gaussian_example_fit.md),
[`multichannel_example_fit`](https://docs.ropensci.org/dynamite/reference/multichannel_example_fit.md)
