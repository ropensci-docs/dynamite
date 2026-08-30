# Print the results from the LFO

Prints the summary of the leave-future-out cross-validation.

## Usage

``` r
# S3 method for class 'lfo'
print(x, ...)
```

## Arguments

- x:

  \[`lfo`\]  
  Output of the `lfo` method.

- ...:

  Ignored.

## Value

Returns `x` invisibly.

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
# \donttest{
# Please update your rstan and StanHeaders installation before running
# on Windows
if (!identical(.Platform$OS.type, "windows")) {
  # This gives warnings due to the small number of iterations
  suppressWarnings(lfo(gaussian_example_fit, L = 20))
}
#> Estimating model with 20 time points.
#> Error in rstan::stan_model(model_code = model_code): Boost not found; call install.packages('BH')
# }
```
