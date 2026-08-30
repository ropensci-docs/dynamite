# Credible Intervals for dynamite Model Parameters

Extracts credible intervals from `dynamitefit` object.

## Usage

``` r
# S3 method for class 'dynamitefit'
confint(object, parm, level = 0.95, ...)
```

## Arguments

- object:

  \[`dynamitefit`\]  
  The model fit object.

- parm:

  Ignored.

- level:

  \[`numeric(1)`\]  
  Credible interval width.

- ...:

  Ignored.

## Value

The rows of the resulting `matrix` will be named using the following
logic: `{parameter}_{time}_{category}_{group}` where `parameter` is the
name of the parameter, `time` is the time index of the parameter,
`category` specifies the level of the response the parameter is related
to if the response is categorical, and `group` determines which group of
observations the parameter is related to in the case of random effects
and loadings. Non-applicable fields in the this syntax are set to `NA`.

## See also

Model outputs
[`as.data.frame.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.frame.dynamitefit.md),
[`as.data.table.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as.data.table.dynamitefit.md),
[`as_draws_df.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/as_draws-dynamitefit.md),
[`coef.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/coef.dynamitefit.md),
[`dynamite()`](https://docs.ropensci.org/dynamite/reference/dynamite.md),
[`get_code()`](https://docs.ropensci.org/dynamite/reference/get_code.md),
[`get_data()`](https://docs.ropensci.org/dynamite/reference/get_data.md),
[`get_parameter_dims()`](https://docs.ropensci.org/dynamite/reference/get_parameter_dims.md),
[`get_parameter_names()`](https://docs.ropensci.org/dynamite/reference/get_parameter_names.md),
[`get_parameter_types()`](https://docs.ropensci.org/dynamite/reference/get_parameter_types.md),
[`ndraws.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/ndraws.dynamitefit.md),
[`nobs.dynamitefit()`](https://docs.ropensci.org/dynamite/reference/nobs.dynamitefit.md)

## Examples

``` r
data.table::setDTthreads(1) # For CRAN
confint(gaussian_example_fit, level = 0.9)
#>                                     5%          95%
#> alpha_y_1_NA_NA                     NA           NA
#> alpha_y_2_NA_NA            0.007403695  0.102271300
#> alpha_y_3_NA_NA            0.026818291  0.170070435
#> alpha_y_4_NA_NA            0.106474401  0.233709265
#> alpha_y_5_NA_NA            0.202245962  0.329303567
#> alpha_y_6_NA_NA            0.244517471  0.374195388
#> alpha_y_7_NA_NA            0.264860517  0.390203385
#> alpha_y_8_NA_NA            0.364657824  0.481656025
#> alpha_y_9_NA_NA            0.390251045  0.519935516
#> alpha_y_10_NA_NA           0.350259409  0.493769444
#> alpha_y_11_NA_NA           0.339647931  0.479354240
#> alpha_y_12_NA_NA           0.379105151  0.517119896
#> alpha_y_13_NA_NA           0.429418537  0.539820413
#> alpha_y_14_NA_NA           0.449291373  0.565293593
#> alpha_y_15_NA_NA           0.428129253  0.559286081
#> alpha_y_16_NA_NA           0.405106725  0.539687935
#> alpha_y_17_NA_NA           0.406249375  0.527570810
#> alpha_y_18_NA_NA           0.397965134  0.521399476
#> alpha_y_19_NA_NA           0.367372795  0.494963990
#> alpha_y_20_NA_NA           0.326038745  0.447364663
#> alpha_y_21_NA_NA           0.264277527  0.401382307
#> alpha_y_22_NA_NA           0.154396289  0.291815841
#> alpha_y_23_NA_NA           0.006143314  0.125536454
#> alpha_y_24_NA_NA          -0.054135556  0.054412793
#> alpha_y_25_NA_NA          -0.032642637  0.079734051
#> alpha_y_26_NA_NA           0.003366138  0.114504501
#> alpha_y_27_NA_NA           0.004254258  0.127682953
#> alpha_y_28_NA_NA           0.036157275  0.163582552
#> alpha_y_29_NA_NA           0.164811555  0.295641981
#> alpha_y_30_NA_NA           0.648446080  0.812868415
#> beta_y_z_NA_NA_NA          1.948189211  1.986526732
#> delta_y_x_1_NA_NA                   NA           NA
#> delta_y_x_2_NA_NA         -0.206186161 -0.109286886
#> delta_y_x_3_NA_NA         -0.960344923 -0.852211076
#> delta_y_x_4_NA_NA         -0.921221572 -0.840961053
#> delta_y_x_5_NA_NA         -0.951310983 -0.880725964
#> delta_y_x_6_NA_NA         -1.095729609 -1.026302030
#> delta_y_x_7_NA_NA         -1.177665339 -1.103289229
#> delta_y_x_8_NA_NA         -1.032715155 -0.966347567
#> delta_y_x_9_NA_NA         -0.944653473 -0.867048071
#> delta_y_x_10_NA_NA        -1.047478264 -0.974612825
#> delta_y_x_11_NA_NA        -1.227764706 -1.166779539
#> delta_y_x_12_NA_NA        -1.405266345 -1.330408188
#> delta_y_x_13_NA_NA        -1.485610106 -1.425804367
#> delta_y_x_14_NA_NA        -1.487403795 -1.420771444
#> delta_y_x_15_NA_NA        -1.444387469 -1.377682768
#> delta_y_x_16_NA_NA        -1.445068583 -1.389108904
#> delta_y_x_17_NA_NA        -1.457626809 -1.386735478
#> delta_y_x_18_NA_NA        -1.400237607 -1.336903543
#> delta_y_x_19_NA_NA        -1.379003301 -1.315181150
#> delta_y_x_20_NA_NA        -1.389038803 -1.314712353
#> delta_y_x_21_NA_NA        -1.306833190 -1.240175454
#> delta_y_x_22_NA_NA        -1.252919941 -1.176769426
#> delta_y_x_23_NA_NA        -1.278993986 -1.205772470
#> delta_y_x_24_NA_NA        -1.244055184 -1.166417314
#> delta_y_x_25_NA_NA        -1.111023748 -1.032825737
#> delta_y_x_26_NA_NA        -0.936925885 -0.862959308
#> delta_y_x_27_NA_NA        -0.776103894 -0.700284094
#> delta_y_x_28_NA_NA        -0.678539976 -0.597379166
#> delta_y_x_29_NA_NA        -0.682123045 -0.569101335
#> delta_y_x_30_NA_NA        -0.264287731 -0.169723560
#> delta_y_y_lag1_1_NA_NA              NA           NA
#> delta_y_y_lag1_2_NA_NA     0.027708475  0.112279697
#> delta_y_y_lag1_3_NA_NA     0.124036033  0.205535682
#> delta_y_y_lag1_4_NA_NA     0.130764498  0.185517352
#> delta_y_y_lag1_5_NA_NA     0.136492033  0.184274517
#> delta_y_y_lag1_6_NA_NA     0.158736589  0.201304342
#> delta_y_y_lag1_7_NA_NA     0.188315647  0.231988190
#> delta_y_y_lag1_8_NA_NA     0.214798375  0.248758601
#> delta_y_y_lag1_9_NA_NA     0.259610197  0.300849347
#> delta_y_y_lag1_10_NA_NA    0.338752175  0.384461000
#> delta_y_y_lag1_11_NA_NA    0.438129961  0.479043146
#> delta_y_y_lag1_12_NA_NA    0.503718900  0.543924974
#> delta_y_y_lag1_13_NA_NA    0.513800912  0.544819902
#> delta_y_y_lag1_14_NA_NA    0.472410979  0.504393001
#> delta_y_y_lag1_15_NA_NA    0.418696779  0.451670605
#> delta_y_y_lag1_16_NA_NA    0.388352290  0.418164448
#> delta_y_y_lag1_17_NA_NA    0.377276206  0.412230701
#> delta_y_y_lag1_18_NA_NA    0.376151400  0.408728180
#> delta_y_y_lag1_19_NA_NA    0.367539514  0.397232588
#> delta_y_y_lag1_20_NA_NA    0.338018022  0.375021582
#> delta_y_y_lag1_21_NA_NA    0.306734768  0.344375855
#> delta_y_y_lag1_22_NA_NA    0.266554888  0.306323389
#> delta_y_y_lag1_23_NA_NA    0.234300050  0.269088275
#> delta_y_y_lag1_24_NA_NA    0.221356890  0.257105545
#> delta_y_y_lag1_25_NA_NA    0.215469673  0.255943805
#> delta_y_y_lag1_26_NA_NA    0.178322712  0.218263071
#> delta_y_y_lag1_27_NA_NA    0.110185573  0.157259300
#> delta_y_y_lag1_28_NA_NA    0.018021725  0.074707567
#> delta_y_y_lag1_29_NA_NA   -0.072873647 -0.008145746
#> delta_y_y_lag1_30_NA_NA    0.009613330  0.099267763
#> nu_y_alpha_NA_NA_1        -0.153441765 -0.033595205
#> nu_y_alpha_NA_NA_2        -0.114870358  0.001979568
#> nu_y_alpha_NA_NA_3         0.027001837  0.147718391
#> nu_y_alpha_NA_NA_4        -0.017882874  0.095613705
#> nu_y_alpha_NA_NA_5        -0.106008453  0.014380618
#> nu_y_alpha_NA_NA_6         0.055439424  0.185745872
#> nu_y_alpha_NA_NA_7        -0.023017193  0.100281578
#> nu_y_alpha_NA_NA_8        -0.142686495 -0.022266947
#> nu_y_alpha_NA_NA_9        -0.084264491  0.033376400
#> nu_y_alpha_NA_NA_10       -0.134529718 -0.018109105
#> nu_y_alpha_NA_NA_11        0.051982517  0.167357658
#> nu_y_alpha_NA_NA_12       -0.086175678  0.030890085
#> nu_y_alpha_NA_NA_13       -0.036082414  0.087035765
#> nu_y_alpha_NA_NA_14       -0.057670042  0.054336202
#> nu_y_alpha_NA_NA_15       -0.132213481 -0.014068792
#> nu_y_alpha_NA_NA_16        0.119076150  0.238364409
#> nu_y_alpha_NA_NA_17        0.005173991  0.116344265
#> nu_y_alpha_NA_NA_18       -0.291157831 -0.163215599
#> nu_y_alpha_NA_NA_19       -0.012703344  0.106147747
#> nu_y_alpha_NA_NA_20       -0.032483541  0.086243555
#> nu_y_alpha_NA_NA_21       -0.183771946 -0.053739117
#> nu_y_alpha_NA_NA_22       -0.070798438  0.047959732
#> nu_y_alpha_NA_NA_23       -0.188532118 -0.060967399
#> nu_y_alpha_NA_NA_24       -0.148277532 -0.033708367
#> nu_y_alpha_NA_NA_25       -0.100581578  0.005855686
#> nu_y_alpha_NA_NA_26       -0.186197499 -0.070342890
#> nu_y_alpha_NA_NA_27        0.024401632  0.133348063
#> nu_y_alpha_NA_NA_28       -0.098378355  0.017171792
#> nu_y_alpha_NA_NA_29       -0.146828123 -0.024331134
#> nu_y_alpha_NA_NA_30        0.041307799  0.171703260
#> nu_y_alpha_NA_NA_31       -0.090290989  0.042255943
#> nu_y_alpha_NA_NA_32       -0.105095753  0.021553250
#> nu_y_alpha_NA_NA_33       -0.005556545  0.106832600
#> nu_y_alpha_NA_NA_34        0.035048445  0.156329137
#> nu_y_alpha_NA_NA_35       -0.026070957  0.094503926
#> nu_y_alpha_NA_NA_36        0.035325901  0.168270253
#> nu_y_alpha_NA_NA_37        0.013239204  0.145541801
#> nu_y_alpha_NA_NA_38       -0.060052740  0.057836406
#> nu_y_alpha_NA_NA_39       -0.061488674  0.052916808
#> nu_y_alpha_NA_NA_40       -0.093290477  0.027073552
#> nu_y_alpha_NA_NA_41       -0.125930729 -0.012921516
#> nu_y_alpha_NA_NA_42       -0.035912809  0.089873419
#> nu_y_alpha_NA_NA_43       -0.169963190 -0.051560721
#> nu_y_alpha_NA_NA_44        0.152443044  0.272365234
#> nu_y_alpha_NA_NA_45        0.021529702  0.149746237
#> nu_y_alpha_NA_NA_46       -0.113245738  0.011313838
#> nu_y_alpha_NA_NA_47       -0.047594049  0.083274829
#> nu_y_alpha_NA_NA_48       -0.089941051  0.029704044
#> nu_y_alpha_NA_NA_49       -0.015592602  0.105131029
#> nu_y_alpha_NA_NA_50       -0.077733483  0.055182110
#> sigma_y_NA_NA_NA           0.191890368  0.204777832
#> sigma_nu_y_alpha_NA_NA_NA  0.079070615  0.112890854
#> tau_y_x_NA_NA_NA           0.257309584  0.507787183
#> tau_y_y_lag1_NA_NA_NA      0.076722283  0.138777375
#> tau_alpha_y_NA_NA_NA       0.138715827  0.292341726
```
