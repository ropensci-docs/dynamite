# Get the elapsed time of a Stan model fit

Get the elapsed time of a Stan model fit

## Usage

``` r
get_elapsed_time(x)

# S3 method for class 'stanfit'
get_elapsed_time(x)

# S3 method for class 'CmdStanMCMC'
get_elapsed_time(x)

# S3 method for class 'CmdStanMCMC_CSV'
get_elapsed_time(x)
```

## Arguments

- x:

  A `stanfit` (from `rstan`) or a `CmdStanMCMC` (from `cmdstanr`)
  object.
