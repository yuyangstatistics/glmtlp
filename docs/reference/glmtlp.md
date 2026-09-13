# glmtlp: A package for fitting a GLM with l0, l1, and tlp regularization.

The package provides 3 penalties: l0, l1, and tlp and 3 distribution
families: gaussian, binomial, and poisson.

Fit generalized linear models via penalized maximum likelihood. The
regularization path is computed for the l0, lasso, or truncated lasso
penalty at a grid of values for the regularization parameter `lambda` or
`kappa`. Fits linear and logistic regression models.

## Usage

``` r
glmtlp(
  X,
  y,
  family = c("gaussian", "binomial"),
  penalty = c("l0", "l1", "tlp"),
  nlambda = ifelse(penalty == "l0", 50, 100),
  lambda.min.ratio = ifelse(nobs < nvars, 0.05, 0.001),
  lambda = NULL,
  kappa = NULL,
  tau = 0.3 * sqrt(log(nvars)/nobs),
  delta = 2,
  tol = 1e-04,
  weights = NULL,
  penalty.factor = rep(1, nvars),
  standardize = FALSE,
  dc.maxit = 20,
  cd.maxit = 10000,
  nr.maxit = 20,
  ...
)
```

## Arguments

- X:

  Input matrix, of dimension `nobs` x `nvars`; each row is an
  observation vector.

- y:

  Response variable, of length `nobs`. For `family="gaussian"`, it
  should be quantitative; for `family="binomial"`, it should be either a
  factor with two levels or a binary vector.

- family:

  A character string representing one of the built-in families. See
  Details section below.

- penalty:

  A character string representing one of the built-in penalties. `"l0"`
  represents the \\L_0\\ penalty, `"l1"` represents the lasso-type
  penalty (\\L_1\\ penalty), and `"tlp"` represents the truncated lasso
  penalty.

- nlambda:

  The number of `lambda` values. Default is 100.

- lambda.min.ratio:

  The smallest value for `lambda`, as a fraction of `lambda.max`, the
  smallest value for which all coefficients are zero. The default
  depends on the sample size `nobs` relative to the number of variables
  `nvars`. If `nobs > nvars`, the default is `0.0001`, and if
  `nobs < nvars`, the default is `0.01`.

- lambda:

  A user-supplied `lambda` sequence. Typically, users should let the
  program compute its own `lambda` sequence based on `nlambda` and
  `lambda.min.ratio`. Supplying a value of `lambda` will override this.
  WARNING: please use this option with care. `glmtlp` relies on warms
  starts for speed, and it's often faster to fit a whole path than a
  single fit. Therefore, provide a decreasing sequence of `lambda`
  values if you want to use this option. Also, when `penalty = 'l0'`, it
  is not recommended for the users to supply this parameter.

- kappa:

  A user-supplied `kappa` sequence. Typically, users should let the
  program compute its own `kappa` sequence based on `nvars` and `nobs`.
  This sequence is used when `penalty = 'l0'`.

- tau:

  A tuning parameter used in the TLP-penalized regression models.
  Default is `0.3 * sqrt(log(nvars)/nobs)`.

- delta:

  A tuning parameter used in the coordinate majorization descent
  algorithm. See Yang, Y., & Zou, H. (2014) in the reference for more
  detail.

- tol:

  Tolerance level for all iterative optimization algorithms.

- weights:

  Observation weights. Default is 1 for each observation.

- penalty.factor:

  Separate penalty factors applied to each coefficient, which allows for
  differential shrinkage. Default is 1 for all variables.

- standardize:

  Logical. Whether or not standardize the input matrix `X`; default is
  `TRUE`.

- dc.maxit:

  Maximum number of iterations for the DC (Difference of Convex
  Functions) programming; default is 20.

- cd.maxit:

  Maximum number of iterations for the coordinate descent algorithm;
  default is 10^4.

- nr.maxit:

  Maximum number of iterations for the Newton-Raphson method; default is
  500.

- ...:

  Additional arguments.

## Value

An object with S3 class `"glmtlp"`.

- beta:

  a `nvars x length(kappa)` matrix of coefficients when
  `penalty = 'l0'`; or a `nvars x length(lambda)` matrix of coefficients
  when `penalty = c('l1', 'tlp')`.

- call:

  the call that produces this object.

- family:

  the distribution family used in the model fitting.

- intercept:

  the intercept vector, of `length(kappa)` when `penalty = 'l0'` or
  `length(lambda)` when `penalty = c('l1', 'tlp')`.

- lambda:

  the actual sequence of `lambda` values used. Note that the length may
  be smaller than the provided `nlambda` due to removal of saturated
  values.

- penalty:

  the penalty type in the model fitting.

- penalty.factor:

  the penalty factor for each coefficient used in the model fitting.

- tau:

  the tuning parameter used in the model fitting, available when
  `penalty = 'tlp'`.

## Details

The sequence of models indexed by `lambda` (when
`penalty = c('l1', 'tlp')`) or `kappa` (when `penalty = 'l0'`) is fit by
the coordinate descent algorithm.

The objective function for the `"gaussian"` family is: \$\$1/2
RSS/nobs + \lambda\*penalty,\$\$ and for the other models it is:
\$\$-loglik/nobs + \lambda\*penalty.\$\$ Also note that, for
`"gaussian"`, `glmtlp` standardizes y to have unit variance (using
1/(n-1) formula).

\## Details on `family` option

`glmtlp` currently only supports built-in families, which are specified
by a character string. For all families, the returned object is a
regularization path for fitting the generalized linear regression
models, by maximizing the corresponding penalized log-likelihood.
`glmtlp(..., family="binomial")` fits a traditional logistic regression
model for the log-odds.

\## Details on `penalty` option

The built-in penalties are specified by a character string. For `l0`
penalty, `kappa` sequence is used for generating the regularization
path, while for `l1` and `tlp` penalty, `lambda` sequence is used for
generating the regularization path.

## glmtlp functions

\`glmtlp()\`, \`cv.glmtlp()\`

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

Useful links:

- <https://yuyangyy.com/glmtlp/>

`print`, `predict`, `coef` and `plot` methods, and the `cv.glmtlp`
function.

## Author

**Maintainer**: Yu Yang <yuyang.stat@gmail.com>
([ORCID](https://orcid.org/0000-0001-7355-6702)) \[copyright holder\]

Authors:

- Chunlin Li <chunlin@iastate.edu>
  ([ORCID](https://orcid.org/0000-0003-2989-8785)) \[copyright holder\]

- Chong Wu ([ORCID](https://orcid.org/0000-0002-8400-1785)) \[copyright
  holder\]

Other contributors:

- Xiaotong Shen \[thesis advisor, copyright holder\]

- Wei Pan \[thesis advisor, copyright holder\]

Chunlin Li, Yu Yang, Chong Wu\
Maintainer: Yu Yang <yang6367@umn.edu>

## Examples

``` r

# Gaussian
X <- matrix(rnorm(100 * 20), 100, 20)
y <- rnorm(100)
fit1 <- glmtlp(X, y, family = "gaussian", penalty = "l0")
fit2 <- glmtlp(X, y, family = "gaussian", penalty = "l1")
fit3 <- glmtlp(X, y, family = "gaussian", penalty = "tlp")

# Binomial

X <- matrix(rnorm(100 * 20), 100, 20)
y <- sample(c(0, 1), 100, replace = TRUE)
fit <- glmtlp(X, y, family = "binomial", penalty = "l1")
```
