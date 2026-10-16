# Get the maximum treedepth of chains of a Stan model fit

Get the maximum treedepth of chains of a Stan model fit

## Usage

``` r
get_max_treedepth(x)

# S3 method for class 'stanfit'
get_max_treedepth(x)

# S3 method for class 'CmdStanMCMC'
get_max_treedepth(x)

# S3 method for class 'CmdStanMCMC_CSV'
get_max_treedepth(x)
```

## Arguments

- x:

  A `stanfit` (from `rstan`) or a `CmdStanMCMC` (from `cmdstanr`)
  object.
