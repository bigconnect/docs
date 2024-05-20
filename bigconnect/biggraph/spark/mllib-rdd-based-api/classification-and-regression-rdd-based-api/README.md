# Classification and Regression - RDD-based API

The `spark.mllib` package supports various methods for binary classification, multiclass classification, and regression analysis. The table below outlines the supported algorithms for each type of problem.

| Problem Type              | Supported Methods                                                                                                          |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| Binary Classification     | linear SVMs, logistic regression, decision trees, random forests, gradient-boosted trees, naive Bayes                      |
| Multiclass Classification | logistic regression, decision trees, random forests, naive Bayes                                                           |
| Regression                | linear least squares, Lasso, ridge regression, decision trees, random forests, gradient-boosted trees, isotonic regression |

More details for these methods can be found here:

* Linear models
  * classification (SVMs, logistic regression)
  * linear regression (least squares, Lasso, ridge)
* Decision trees
* Ensembles of decision trees
  * random forests
  * gradient-boosted trees
* Naive Bayes
* Isotonic regression
