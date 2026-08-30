# Simulated Categorical Multivariate Panel Data

A simulated data containing multiple individuals with two categorical
response variables.

## Usage

``` r
categorical_example
```

## Format

A data frame with 2000 rows and 5 variables:

- id:

  Variable defining individuals (1 to 100).

- time:

  Variable defining the time point of the measurement (1 to 20).

- x:

  Categorical variable with three levels, A, B, and C.

- y:

  Categorical variable with three levels, a, b, and c.

- z:

  A continuous covariate.

## Source

The data was generated via `categorical_example.R` in
<https://github.com/ropensci/dynamite/tree/main/data-raw/>

## See also

Example models
[`categorical_example_fit`](https://docs.ropensci.org/dynamite/reference/categorical_example_fit.md),
[`gaussian_example`](https://docs.ropensci.org/dynamite/reference/gaussian_example.md),
[`gaussian_example_fit`](https://docs.ropensci.org/dynamite/reference/gaussian_example_fit.md),
[`multichannel_example`](https://docs.ropensci.org/dynamite/reference/multichannel_example.md),
[`multichannel_example_fit`](https://docs.ropensci.org/dynamite/reference/multichannel_example_fit.md)
