# Cross-validation for glmtlp

Performs k-fold cross-validation for l0, l1, or TLP-penalized regression
models over a grid of values for the regularization parameter `lambda`
(if `penalty="l0"`) or `kappa` (if `penalty="l0"`).

## Usage

``` r
cv.glmtlp(X, y, ..., seed = NULL, nfolds = 10, obs.fold = NULL, ncores = 1)
```

## Arguments

- X:

  input matrix, of dimension `nobs` x `nvars`, as in `glmtlp`.

- y:

  response, of length nobs, as in `glmtlp`.

- ...:

  Other arguments that can be passed to `glmtlp`.

- seed:

  the seed for reproduction purposes

- nfolds:

  number of folds; default is 10. The smallest value allowable is
  `nfolds=3`

- obs.fold:

  an optional vector of values between 1 and `nfolds` identifying what
  fold each observation is in. If supplied, `nfolds` can be missing.

- ncores:

  number of cores utilized; default is 1. If greater than 1, then
  `doParallel::foreach` will be used to fit each fold; if equal to 1,
  then for loop will be used to fit each fold. Users don't have to
  register parallel clusters outside.

## Value

an object of class `"cv.glmtlp"` is returned, which is a list with the
ingredients of the cross-validation fit.

- call:

  the function call

- cv.mean:

  The mean cross-validated error - a vector of length `length(kappa)` if
  `penalty = "l0"` and `length{lambda}` otherwise.

- cv.se:

  estimate of standard error of `cv.mean`.

- fit:

  a fitted glmtlp object for the full data.

- idx.min:

  the index of the `lambda` or `kappa` sequence that corresponding to
  the smallest cv mean error.

- kappa:

  the values of `kappa` used in the fits, available when
  `penalty = 'l0'`.

- kappa.min:

  the value of `kappa` that gives the minimum `cv.mean`, available when
  `penalty = 'l0'`.

- lambda:

  the values of `lambda` used in the fits.

- lambda.min:

  value of `lambda` that gives minimum `cv.mean`, available when penalty
  is 'l1' or 'tlp'.

- null.dev:

  null deviance of the model.

- obs.fold:

  the fold id for each observation used in the CV.

## Details

The function calls `glmtlp` `nfolds`+1 times; the first call to get the
`lambda` or `kappa` sequence, and then the rest to compute the fit with
each of the folds omitted. The cross-validation error is based on
deviance (check here for more details). The error is accumulated over
the folds, and the average error and standard deviation is computed.

When `family = "binomial"`, the fold assignment (if not provided by the
user) is generated in a stratified manner, where the ratio of 0/1
outcomes are the same for each fold.

## References

Shen, X., Pan, W., & Zhu, Y. (2012). *Likelihood-based selection and
sharp parameter estimation. Journal of the American Statistical
Association, 107(497), 223-232.*\
Shen, X., Pan, W., Zhu, Y., & Zhou, H. (2013). *On constrained and
regularized high-dimensional regression. Annals of the Institute of
Statistical Mathematics, 65(5), 807-832.*\
Li, C., Shen, X., & Pan, W. (2021). *Inference for a Large Directed
Graphical Model with Interventions. arXiv preprint arXiv:2110.03805.*\
Yang, Y., & Zou, H. (2014). *A coordinate majorization descent algorithm
for l1 penalized learning. Journal of Statistical Computation and
Simulation, 84(1), 84-95.*\
Two R package Github: *ncvreg* and *glmnet*.

## See also

`glmtlp` and `plot`, `predict`, and `coef` methods for `"cv.glmtlp"`
objects.

## Author

Chunlin Li, Yu Yang, Chong Wu\
Maintainer: Yu Yang <yang6367@umn.edu>

## Examples

``` r

# Gaussian
X <- matrix(rnorm(100 * 20), 100, 20)
y <- rnorm(100)
cv.fit <- cv.glmtlp(X, y, family = "gaussian", penalty = "l1", seed=2021)

# Binomial
X <- matrix(rnorm(100 * 20), 100, 20)
y <- sample(c(0,1), 100, replace = TRUE)
cv.fit <- cv.glmtlp(X, y, family = "binomial", penalty = "l1", seed=2021)
```
