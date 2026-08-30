# Compute Lagging Values of an Imputed Response

Function for computing the lagged values of an imputed response in
`mice`.

## Usage

``` r
mice.impute.lag(y, ry, x, wy = NULL, group_val, group_var, resp, ...)
```

## Arguments

- y:

  Vector to be imputed

- ry:

  Logical vector of length `length(y)` indicating the the subset `y[ry]`
  of elements in `y` to which the imputation model is fitted. The `ry`
  generally distinguishes the observed (`TRUE`) and missing values
  (`FALSE`) in `y`.

- x:

  Numeric design matrix with `length(y)` rows with predictors for `y`.
  Matrix `x` may have no missing values.

- wy:

  Logical vector of length `length(y)`. A `TRUE` value indicates
  locations in `y` for which imputations are created.

- group_val:

  \[[`vector()`](https://rdrr.io/r/base/vector.html)\]  
  Values of the grouping variable.

- group_var:

  \[`character(1)`\]  
  Name of the grouping variable.

- resp:

  \[`character(1)`\]  
  Name of the response variable.

- ...:

  Other named arguments.
