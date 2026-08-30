# Add Lagged Responses as Predictors to Each Channel of a dynamite Model

Adds the lagged value of the response of each channel specified via
[`dynamiteformula()`](https://docs.ropensci.org/dynamite/reference/dynamiteformula.md)
as a predictor to each channel. The added predictors can be either
time-varying or time-invariant.

## Usage

``` r
lags(k = 1L, type = c("fixed", "varying", "random"))
```

## Arguments

- k:

  \[[`integer()`](https://rdrr.io/r/base/integer.html)\]  
  Values lagged by `k` units of time of each observed response variable
  will be added as a predictor for each channel. Should be a positive
  (unrestricted) integer.

- type:

  \[`integer(1)`\]  
  Either `"fixed"` or `"varying"` which indicates whether the
  coefficients of the added lag terms should vary in time or not.

## Value

An object of class `lags`.

## See also

Model formula construction
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md),
[`dynamiteformula()`](https://docs.ropensci.org/dynamite/reference/dynamiteformula.md),
[`lfactor()`](https://docs.ropensci.org/dynamite/reference/lfactor.md),
[`random_spec()`](https://docs.ropensci.org/dynamite/reference/random_spec.md),
[`splines()`](https://docs.ropensci.org/dynamite/reference/splines.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
obs(y ~ -1 + varying(~x), family = "gaussian") +
  lags(type = "varying") + splines(df = 20)
#>   Family   Formula             
#> y gaussian y ~ -1 + varying(~x)
#> 
#> Lagged responses added as varying predictors with: k = 1

# A two-channel categorical model with time-invariant predictors
# here, lag terms are specified manually
obs(x ~ z + lag(x) + lag(y), family = "categorical") +
  obs(y ~ z + lag(x) + lag(y), family = "categorical")
#>   Family      Formula                
#> x categorical x ~ z + lag(x) + lag(y)
#> y categorical y ~ z + lag(x) + lag(y)

# The same categorical model as above, but with the lag terms
# added using 'lags'
obs(x ~ z, family = "categorical") +
  obs(y ~ z, family = "categorical") +
  lags(type = "fixed")
#>   Family      Formula
#> x categorical x ~ z  
#> y categorical y ~ z  
#> 
#> Lagged responses added as fixed predictors with: k = 1
```
