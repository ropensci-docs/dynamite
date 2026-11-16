# Deprecated Functions in the dynamite Package

These functions are provided for compatibility with older versions of
the package. They will eventually be completely removed.

## Usage

``` r
plot_betas(x, ...)
plot_deltas(x,  ...)
plot_nus(x, ...)
plot_lambdas(x,  ...)
plot_psis(x, ...)
```

## Arguments

- x:

  \[`dynamitefit`\]  
  The model fit object.

- ...:

  Not used.

## Value

A `ggplot` object.

## Details

- `plot_betas` is now called via `plot(., types = "beta")`

- `plot_deltas` is now called via `plot(., types = "delta")`

- `plot_nus` is now called via `plot(., types = "nu")`

- `plot_lambdas` is now called via `plot(., types = "lambda")`

- `plot_psis` is now called via `plot(., types = "psi")`

## See also

See
[`plot.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/plot.dynamitefit.md)
for documentation of the parameters of these functions
