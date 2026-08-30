# Simulated Data of a Gaussian Response

Simulated data containing a Gaussian response variable `y` with two
covariates. The dataset was generated from a model with time-varying
effects of covariate `x` and the lagged value of the response variable,
time-varying intercept, and time-invariant effect of covariate `z`. The
time-varying coefficients vary according to a spline with 20 degrees of
freedom.

## Usage

``` r
gaussian_example
```

## Format

A data frame with 3000 rows and 5 variables:

- y:

  The response variable.

- x:

  A continuous covariate.

- z:

  A binary covariate.

- id:

  Variable defining individuals (1 to 50).

- time:

  Variable defining the time point of the measurement (1 to 30).

## Source

The data was generated via `gaussian_example.R` in
<https://github.com/ropensci/dynamite/tree/main/data-raw/>

## See also

Example models
[`categorical_example`](https://docs.ropensci.org/dynamite/reference/categorical_example.md),
[`categorical_example_fit`](https://docs.ropensci.org/dynamite/reference/categorical_example_fit.md),
[`gaussian_example_fit`](https://docs.ropensci.org/dynamite/reference/gaussian_example_fit.md),
[`multichannel_example`](https://docs.ropensci.org/dynamite/reference/multichannel_example.md),
[`multichannel_example_fit`](https://docs.ropensci.org/dynamite/reference/multichannel_example_fit.md)
