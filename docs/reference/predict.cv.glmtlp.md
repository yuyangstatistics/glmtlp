# Predict Method for a "cv.glmtlp" Object.

Makes predictions for a cross-validated glmtlp model, using the stored
`"glmtlp"` object, and the optimal value chosen for `lambda`.

## Usage

``` r
# S3 method for class 'cv.glmtlp'
predict(
  object,
  X,
  type = c("link", "response", "class", "coefficients", "numnzs", "varnzs"),
  lambda = NULL,
  kappa = NULL,
  which = object$idx.min,
  ...
)

# S3 method for class 'cv.glmtlp'
coef(object, lambda = NULL, kappa = NULL, which = object$idx.min, ...)
```

## Arguments

- object:

  Fitted `"cv.glmtlp"` object.

- X:

  X Matrix of new values for `X` at which predictions are to be made.
  Must be a matrix.

- type:

  Type of prediction to be made. For `"gaussian"` models, type `"link"`
  and `"response"` are equivalent and both give the fitted values. For
  `"binomial"` models, type `"link"` gives the linear predictors and
  type `"response"` gives the fitted probabilities. Type
  `"coefficients"` computes the coefficients at the provided values of
  `lambda` or `kappa`. Note that for `"binomial"` models, results are
  returned only for the class corresponding to the second level of the
  factor response. Type `"class"` applies only to `"binomial"` models,
  and gives the class label corresponding to the maximum probability.
  Type `"numnz"` gives the total number of non-zero coefficients for
  each value of `lambda` or `kappa`. Type `"varnz"` gives a list of
  indices of the nonzero coefficients for each value of `lambda` or
  `kappa`.

- lambda:

  Value of the penalty parameter `lambda` at which predictions are to be
  made Default is NULL.

- kappa:

  Value of the penalty parameter `kappa` at which predictions are to be
  made. Default is NULL.

- which:

  Index of the penalty parameter `lambda` or `kappa` sequence at which
  predictions are to be made. Default is the `idx.min` stored in the
  `cv.glmtp` object.

- ...:

  Additional arguments.

## Value

The object returned depends on `type`.

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

`print`, `predict`, `coef` and `plot` methods, and the `cv.glmtlp`
function.

## Author

Chunlin Li, Yu Yang, Chong Wu\
Maintainer: Yu Yang <yang6367@umn.edu>

## Examples

``` r
X <- matrix(rnorm(100 * 20), 100, 20)
y <- rnorm(100)
cv.fit <- cv.glmtlp(X, y, family = "gaussian", penalty = "l1")
predict(cv.fit, X = X[1:5, ])
#> [1] 0.04564887 0.04564887 0.04564887 0.04564887 0.04564887
coef(cv.fit)
#>  intercept         V1         V2         V3         V4         V5         V6 
#> 0.04564887 0.00000000 0.00000000 0.00000000 0.00000000 0.00000000 0.00000000 
#>         V7         V8         V9        V10        V11        V12        V13 
#> 0.00000000 0.00000000 0.00000000 0.00000000 0.00000000 0.00000000 0.00000000 
#>        V14        V15        V16        V17        V18        V19        V20 
#> 0.00000000 0.00000000 0.00000000 0.00000000 0.00000000 0.00000000 0.00000000 
predict(cv.fit, X = X[1:5, ], lambda = 0.1)
#> [1] -0.06388020 -0.09547175  0.26124340 -0.20715326  0.18311460
```
