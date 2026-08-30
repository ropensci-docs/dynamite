# Get `pars_oi` of a Stan model fit

Get `pars_oi` of a Stan model fit

## Usage

``` r
get_pars_oi(x)

# S3 method for class 'stanfit'
get_pars_oi(x)

# S3 method for class 'CmdStanMCMC'
get_pars_oi(x)

# S3 method for class 'CmdStanMCMC_CSV'
get_pars_oi(x)
```

## Arguments

- x:

  A `stanfit` (from `rstan`) or a `CmdStanMCMC` (from `cmdstanr`)
  object.
