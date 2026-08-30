# Plots for `dynamitefit` Objects

Produces the traceplots and the density plots of the model parameters.
Can also be used to plot the time-varying and time-invariant parameters
of the model along with their posterior intervals. See the `plot_type`
argument for details on available plots.

## Usage

``` r
# S3 method for class 'dynamitefit'
plot(
  x,
  plot_type = c("default", "trace", "dag"),
  types = NULL,
  parameters = NULL,
  responses = NULL,
  groups = NULL,
  times = NULL,
  level = 0.05,
  alpha = 0.5,
  facet = TRUE,
  scales = c("fixed", "free"),
  n_params = NULL,
  ...
)
```

## Arguments

- x:

  \[`dynamitefit`\]  
  The model fit object.

- plot_type:

  \[`character(1)`\]  
  What type of plot to draw? The default is `"default"` which draws
  posterior means and intervals of the parameters selected by `types` or
  `parameters`. If both `"types"` and `parameters` are `NULL`, all
  parameters are drawn up to the maximum specified by `n_params`. Option
  `"trace"` instead draws posterior densities and traceplots of the
  parameters. Option `"dag"` instead plots the directed acyclic graph of
  the model formula, see
  [`plot.dynamiteformula()`](https://docs.ropensci.org/dynamite/reference/plot.dynamiteformula.md)
  for the arguments available for this option.

- types:

  \[`character(1)`\]  
  Types of the parameter for which the plots should be drawn. Possible
  options can be found with the function
  [`get_parameter_types()`](https://docs.ropensci.org/dynamite/reference/get_parameter_types.md).
  Ignored if the argument `parameters` is supplied.

- parameters:

  \[`charecter()`\]  
  Parameter name(s) for which the plots should be drawn. Possible
  options can be found with the function
  [`get_parameter_names()`](https://docs.ropensci.org/dynamite/reference/get_parameter_names.md).
  The default is all parameters, limited by `n_params`.

- responses:

  \[[`character()`](https://rdrr.io/r/base/character.html)\]  
  Response(s) for which the plots should be drawn. Possible options are
  `unique(x$priors$response)`. Default is all responses. Ignored if the
  argument `parameters` is supplied.

- groups:

  \[`character(1)`\]  
  Group name(s) for which the plots should be drawn for group-specific
  parameters.

- times:

  \[[`double()`](https://rdrr.io/r/base/double.html)\]  
  Time point(s) for which the plots should be drawn for time-varying
  parameters. By default, all time points are included, up to the
  maximum number of parameters specified by `n_params` starting from the
  first non-fixed time point.

- level:

  \[`numeric(1)`\]  
  Level for posterior intervals. Default is 0.05, leading to 90%
  intervals.

- alpha:

  \[`numeric(1)`\]  
  Opacity level for `geom_ribbon`. Default is 0.5.

- facet:

  \[`logical(1)`\]  
  Should the time-invariant parameters be plotted separately (`TRUE`) or
  in a single plot (`FALSE`)?

- scales:

  \[`character(1)`\]  
  Should y-axis of the panels be `"fixed"` (the default) or `"free"`?
  See
  [`ggplot2::facet_wrap()`](https://ggplot2.tidyverse.org/reference/facet_wrap.html).

- n_params:

  \[[`integer()`](https://rdrr.io/r/base/integer.html)\]  
  A single value or a vector of length 2 specifying the maximum number
  of parameters to plot. If a single value is provided, the same limit
  is used for all parameters. If a vector is supplied, the first element
  defines the maximum number of time-invariant parameters to plot and
  the second the maximum number of time-varying parameters to plot. The
  defaults values are 20 for time-invariant parameters and 3 for
  time-varying parameters. The default value is 5 for
  `plot_type == "trace"`.

- ...:

  Arguments passed to
  [`plot.dynamiteformula()`](https://docs.ropensci.org/dynamite/reference/plot.dynamiteformula.md)
  when using `plot_type = "dag"`.

## Value

A `ggplot` object.

## See also

Drawing plots
[`plot.dynamiteformula()`](https://docs.ropensci.org/dynamite/reference/plot.dynamiteformula.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
plot(gaussian_example_fit, type = "beta")

```
