# Get the number of draws of a Stan model fit

Get the number of draws of a Stan model fit

## Usage

``` r
get_ndraws(x)

# S3 method for class 'stanfit'
get_ndraws(x)

# S3 method for class 'CmdStanMCMC'
get_ndraws(x)

# S3 method for class 'CmdStanMCMC_CSV'
get_ndraws(x)
```

## Arguments

- x:

  A `stanfit` (from `rstan`) or a `CmdStanMCMC` (from `cmdstanr`)
  object.
