# Get the number of chains of a Stan model fit

Get the number of chains of a Stan model fit

## Usage

``` r
get_nchains(x)

# S3 method for class 'stanfit'
get_nchains(x)

# S3 method for class 'CmdStanMCMC'
get_nchains(x)

# S3 method for class 'CmdStanMCMC_CSV'
get_nchains(x)
```

## Arguments

- x:

  A `stanfit` (from `rstan`) or a `CmdStanMCMC` (from `cmdstanr`)
  object.
