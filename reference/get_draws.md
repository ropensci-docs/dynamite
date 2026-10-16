# Get the draws of a Stan model fit

Get the draws of a Stan model fit

## Usage

``` r
get_draws(x, pars)

# S3 method for class 'stanfit'
get_draws(x, pars)

# S3 method for class 'CmdStanMCMC'
get_draws(x, pars)

# S3 method for class 'CMdStanMCMC_CSV'
get_draws(x, pars)
```

## Arguments

- x:

  A `stanfit` (from `rstan`) or a `CmdStanMCMC` (from `cmdstanr`)
  object.

- pars:

  A `character` vector of parameter names.
