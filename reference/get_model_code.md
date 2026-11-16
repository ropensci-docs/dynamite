# Get the model code of a Stan model fit

Get the model code of a Stan model fit

## Usage

``` r
get_model_code(x)

# S3 method for class 'stanfit'
get_model_code(x)

# S3 method for class 'CmdStanMCMC'
get_model_code(x)

# S3 method for class 'CmdStanMCMC_CSV'
get_model_code(x)
```

## Arguments

- x:

  A `stanfit` (from `rstan`) or a `CmdStanMCMC` (from `cmdstanr`)
  object.
