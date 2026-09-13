# Simulate a binomial data set

Simulate a data set with binary response following the logistic
regression model.

## Usage

``` r
gen.binomial.data(n, p, rho = 0, kappa = 5, beta.type = 1, seed = 2021)
```

## Arguments

- n:

  Sample size.

- p:

  Number of covariates.

- rho:

  The parameter defining the AR(1) correlation matrix.

- kappa:

  The number of nonzero coefficients.

- beta.type:

  Numeric indicator for choosing the beta type. For `beta.type = 1`, the
  true coefficient vector has `kappa` components being 1, roughly
  equally distributed between 1 to `p`. For `beta.type = 2`, the first
  `kappa` values are 1, and the rest are 0. For `beta.type = 3`, the
  first `kappa` values are equally-spaced values from 10 to 0.5, and the
  rest are 0. For `beta.type = 4`, the first `kappa` values are the
  first `kappa` values in c(-10, -6, -2, 2, 6, 10), and the rest are 0.
  For `beta.type = 5`, the first `kappa` values are 1, and the rest
  decay exponentially to 0 with base 0.5.

- seed:

  The seed for reproducibility. Default is 2021.

## Value

A list containing the simulated data.

- X:

  the covariate matrix, of dimension `n` x `p`.

- y:

  the response, of length `n`.

- beta:

  the true coefficients, of length `p`.

## Examples

``` r
bin_data <- gen.binomial.data(n = 200, p = 20, seed = 2021)
head(bin_data$X)
#>              V1         V2         V3         V4         V5          V6
#> [1,] -0.1224600  0.2701953 -0.6745562  0.1492579  0.5534009  0.80959540
#> [2,]  0.5524566 -1.3432502  0.2381266 -0.3123636 -1.1167970  0.09941633
#> [3,]  0.3486495 -0.8488889  0.5450859 -1.2595894  0.5740307 -2.34302131
#> [4,]  0.3596322 -0.4076079 -0.4488515  0.0519813  1.2043346  0.67652598
#> [5,]  0.8980537 -0.6661505  0.9712467  0.2044272  0.7274956 -3.61147374
#> [6,] -1.9225695 -0.1032374 -1.5471639  1.3869823 -0.7023848 -0.16416799
#>               V7         V8         V9        V10        V11        V12
#> [1,] -0.89020680 -1.1028167  1.3645402  0.4982807  0.3147181 -0.6066470
#> [2,]  0.96022096  0.4343786  0.4568313  0.9032203  1.2000068  1.0565220
#> [3,]  1.26888856  1.2058157  2.1983623 -0.4181034 -0.8072166  1.3920142
#> [4,]  0.25411730 -1.0206682 -0.6805850  1.4747203 -0.6763614 -1.3792104
#> [5,]  0.03379941  1.8178839 -0.9349661 -2.0133506  0.7913844 -0.1406768
#> [6,]  0.03272896 -0.4072112  0.9655597  1.6591117  0.6367649 -1.1186803
#>             V13        V14         V15         V16         V17       V18
#> [1,] -2.8645250 -0.4444342  0.08738679  0.07521845 -0.28434897 1.0566142
#> [2,] -0.4067150  0.8172733 -1.23844170 -0.76200580 -0.04859536 1.4287991
#> [3,] -0.9921254 -0.7647219  0.60099751 -0.82983377 -1.20895180 1.2845802
#> [4,]  1.5503138 -0.2771623 -2.68766095 -0.75836338  0.48379191 0.1668427
#> [5,] -0.1953912  1.2854050 -0.91182644 -1.55430328  2.42388384 1.2601177
#> [6,]  3.1064929 -0.9233080 -0.77375020 -1.62213762  1.74302228 0.9810522
#>             V19        V20
#> [1,]  0.7012877 -0.8384305
#> [2,]  0.4267106  2.2454396
#> [3,] -1.8019905 -0.1551519
#> [4,]  0.4479343  1.8887326
#> [5,] -1.3472316  1.7914329
#> [6,]  0.3858948  0.2903452
head(bin_data$y)
#> [1] 0 1 0 1 0 1
head(bin_data$beta)
#> [1] 1 0 0 0 0 1
```
