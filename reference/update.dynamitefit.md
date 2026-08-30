# Update a dynamite Model

Note that using a different backend for the original model fit and when
updating can lead to an error due to different naming in `cmdstanr` and
`rstan` sampling arguments.

## Usage

``` r
# S3 method for class 'dynamitefit'
update(
  object,
  dformula = NULL,
  data = NULL,
  priors = NULL,
  recompile = NULL,
  ...
)
```

## Arguments

- object:

  \[`dynamitefit`\]  
  The model fit object.

- dformula:

  \[`dynamiteformula`\]  
  Updated model formula. By default the original formula is used.

- data:

  \[`data.frame`,
  [`tibble::tibble`](https://tibble.tidyverse.org/reference/tibble.html),
  or
  [`data.table::data.table`](https://rdrr.io/pkg/data.table/man/data.table.html)\]  
  Data for the updated model. By default original data is used.

- priors:

  \[`data.frame`\]  
  Updated priors. By default the priors of the original model are used.

- recompile:

  \[`logical(1)`\]  
  Should the model be recompiled? If `NULL` (default), tries to avoid
  recompilation. Recompilation is forced when the model formula or the
  priors are changed, or if the new data contains missing values in a
  channel which did not contain missing values in the original data.
  Recompilation is also forced in case the backend previous or new
  backend is `cmdstanr`.

- ...:

  Additional parameters to `dynamite`.

## Value

An updated `dynamitefit` object.

## See also

Model fitting
[`dynamice()`](https://docs.ropensci.org/dynamite/reference/dynamice.md),
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md),
[`get_priors()`](https://docs.ropensci.org/dynamite/reference/get_priors.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
if (FALSE) { # \dontrun{
# re-estimate the example fit without thinning:
# As the model is compiled on Windows, this will fail on other platforms
if (identical(.Platform$OS.type, "windows")) {
  fit <- update(gaussian_example_fit, thin = 1)
}
} # }
```
