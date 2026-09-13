# Generate lambda sequence.

Generate lambda sequence.

## Usage

``` r
setup_lambda(X, y, weights, lambda.min.ratio, nlambda)
```

## Arguments

- X:

  Input matrix, of dimension `nobs` x `nvars`; each row is an
  observation vector.

- y:

  Response variable, of length `nobs`. For `family="gaussian"`, it
  should be quantitative; for `family="binomial"`, it should be either a
  factor with two levels or a binary vector.

- weights:

  Observation weights.

- lambda.min.ratio:

  The smallest value for `lambda`, as a fraction of `lambda.max`, the
  smallest value for which all coefficients are zero. The default
  depends on the sample size `nobs` relative to the number of variables
  `nvars`.

- nlambda:

  The number of `lambda` values.
