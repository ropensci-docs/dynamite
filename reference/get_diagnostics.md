# Get the diagnostics of a Stan model fit

Get the diagnostics of a Stan model fit

## Usage

``` r
get_diagnostics(x)

# S3 method for class 'stanfit'
get_diagnostics(x)

# S3 method for class 'CmdStanMCMC'
get_diagnostics(x)

# S3 method for class 'CmdStanMCMC_CSV'
get_diagnostics(x)
```

## Arguments

- x:

  A `stanfit` (from `rstan`) or a `CmdStanMCMC` (from `cmdstanr`)
  object.
