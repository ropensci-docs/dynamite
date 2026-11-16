# Get the algorithm used in a Stan model fit

Get the algorithm used in a Stan model fit

## Usage

``` r
get_algorithm(x)

# S3 method for class 'stanfit'
get_algorithm(x)

# S3 method for class 'CmdStanMCMC'
get_algorithm(x)

# S3 method for class 'CmdStanMCMC_CSV'
get_algorithm(x)
```

## Arguments

- x:

  A `stanfit` (from `rstan`) or a `CmdStanMCMC` (from `cmdstanr`)
  object.
