# Diagnostic Plot for Pareto k Values from LFO

Plots Pareto k values per each time point (with one point per group),
together with a horizontal line representing the used threshold.

## Usage

``` r
# S3 method for class 'lfo'
plot(x, ...)
```

## Arguments

- x:

  \[`lfo`\]  
  Output of the `lfo` method.

- ...:

  Ignored.

## Value

A `ggplot` object.

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
# \donttest{
# Please update your rstan and StanHeaders installation before running
# on Windows
if (!identical(.Platform$OS.type, "windows")) {
  # This gives warnings due to the small number of iterations
  plot(suppressWarnings(
    lfo(gaussian_example_fit, L = 20, chains = 1, cores = 1)
  ))
}
#> Estimating model with 20 time points.
#> Error in rstan::stan_model(model_code = model_code): Boost not found; call install.packages('BH')
# }
```
